---
title: "与 RxJava 配合使用"
permalink: /docs/android/reactive-tangram
excerpt: "与 RxJava 配合使用"
modified: 2018-05-05T10:01:43-04:00
sidebar:
  title: "Android 使用指南"
  nav: android-docs
---

## 响应式支持

新版本 Tangram 中，Tangram 支持与 RxJava 配合使用，可以与业务层一起做响应式实现。Tangram 的响应式支持主要有以下几个方面：

+ Tangram 输出事件的响应式改造：包括点击事件、Banner 的滚动事件、倒计时事件、布局异步加载事件等；
+ Tangram 消化数据的响应式改造：包括设置页面数据、插入布局数据、刷新布局等；
+ Tangram 数据解析流程的支持：解析布局数据、解析组件数据；
+ Tangram 组件绑定生命周期的管理：允许组件通过响应式的方式异步绑定数据，并自动取消订阅；

下面详细介绍每个环节的使用。

## 开启响应式特性

Tangram 内部 provided 依赖了：

```
provided 'io.reactivex.rxjava2:rxjava:2.1.12'
provided 'io.reactivex.rxjava2:rxandroid:2.0.2'
```

provided 是为了编译通过，最终的依赖需要业务层自行 compile 依赖，这样对于不想要使用 RxJava 的人来说就不会额外引入多余的包。添加完依赖之后还需要代码中显示开启：

```
engine.setSupportRx(true);
```

这样内部内部流程和相应的接口才能切换到响应式逻辑。

## 输出事件的响应式接入

像点击、滚动、倒计时、异步加载数据等，都是在 Tangram 页面的使用过程中由用户或者框架产生，传统的响应这些事件的方式是提供异步回调，而响应式改造之后提供了 `Observable` 接口，使用方可以获取到一个 `Observable`，并以此为一个事件流的起点，执行自己的业务逻辑。当然，在实现上，这些 `Observable` 本质上还是封装了传统的回调接口，然后触发事件的发送。

### 点击事件

只针对 native 组件提供支持，`BaseCell` 里提供两个接口用来产生点击事件的 `Observable`：

+ `public CellClickObservable click(View view)`
+ `public CellClickObservable click(View view, ClickExposureCellOp rxClickExposureEvent)`

其实也可以完全自己封装一个 `Observable`，使用这两个接口的好处是内部做了缓存，在复用过程中可以使用同一个 `Observable` 对象，避免重复创建开销；第二个方法里的参数 `ClickExposureCellOp rxClickExposureEvent` 代码点击事件的信息，内部包含组件信息，点击的view，以及附加参数。

配合自定义的 `SimpleClickSupport`，做业务处理，建议在自定义的 `SimpleClickSupport`  里做取消订阅管理。比如：

```
public class MyClickSupport extends SimpleClickSupport {
	
	private CompositeDisposable mCompositeDisposable = new CompositeDisposable();
	
	public void addDisposable(Disposable d) {
        mCompositeDisposable.add(d);
    }

    @Override
    public void destroy() {
        super.destroy();
        mCompositeDisposable.clear();
    }
}
```

然后在组件的绑定数据流程里可以这么写：

```
MyClickSupport clickSupport = (MyClickSupport)cell.serviceManager.getService(SimpleClickSupport.class);
clickSupport.addDisposable(
    cell.click(this)
        .observeOn(Schedulers.computation())
        //可以穿插做一些耗时的转换操作
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(new Consumer() {
        		@Override
            public void accept(Intent intent) throws Exception {
            	//执行最后的逻辑
            }
        })
);
```

### 曝光事件

与点击事件类似，也有类似的接口：

+ `public void exposure(View targetView)`
+ `public void exposure(View targetView, ClickExposureCellOp rxClickExposureEvent)`

### 倒计时事件

倒计时仍然使用 `TimerSupport`，默认接口保持不变，只要开启了响应式特性（`setSupportRx(true)`），内部实现采用响应式的方式，同时封装了两个接口可以直接使用：

+ `public Observable<Long> getTickObservable(int interval, boolean intermediate)`：按秒执行倒计时，发送消息流；
+ `public Observable<Long> getTickObservable(int interval, int total, boolean intermediate)`：按秒按秒执行倒计时，发送消息流，总共执行 total 秒；

### Banner 滚动事件

在 `BannerSupport` 里，新增接口：

+ `public Observable<Integer> observeSelected(String id)`：选中某一页的事件流；
+ `public Observable<ScrollEvent> observeScrolled(String id)`：滚动过程的事件流；
+ `public Observable<Integer> observeScrollStateChanged(String id)`：滚动状态变化的事件流；

参数 id 是 container-banner 布局卡片的 id，因为一个页面可能有多个 container-banner，需要分别监听，同样需要注意自己管理订阅状态，在页面退出的时候取消订阅，避免内存泄露；

举个例子：

```
Disposable dsp1 = bannerSupport.observeSelected("banner1").subscribe(new Consumer<Integer>() {
    @Override
    public void accept(Integer integer) throws Exception {
        Log.d("TangramActivity", "1 selected " + integer);
    }
});
mCompositeDisposable.add(dsp1);
Disposable dsp3 = bannerSupport.observeScrollStateChanged("banner2").subscribe(new Consumer<Integer>() {
    @Override
    public void accept(Integer integer) throws Exception {
        Log.d("TangramActivity", "state changed " + integer);
    }
});
mCompositeDisposable.add(dsp3);
Disposable dsp4 = bannerSupport.observeScrolled("banner2").subscribe(new Consumer<ScrollEvent>() {
    @Override
    public void accept(ScrollEvent scrollEvent) throws Exception {
        Log.d("TangramActivity", "scrolled " + scrollEvent.toString());
    }
});
mCompositeDisposable.add(dsp4);
```

### 布局异步加载事件

在 `CardLoadSupport` 里，原本有触发 Card 异步加载数据，或者加载更多的接口，新增了两对响应式的接口：

+ `public Observable<Card> observeCardLoading()`：Card 内容的异步加载 `Observable`，业务方拿到它之后可以对接网络加载、数据解析等一系列流程，然后将结果封装成一个 `LoadGroupOp`，提供到下面的接口使用；
+ `public Consumer<LoadGroupOp> asDoLoadFinishConsumer()`：异步加载的 `Consumer`，当然你也可以选择不使用它，自己定义一个 `Consumer`；参数 `LoadGroupOp` 里封装了原始的 `Card` 对象，以及一个 `List<BaseCell>` 结果，`List<BaseCell>` 不为空代表成功，否则代表失败；

举个例子：

```
Observable<Card> loadCardObservable = cardLoadSupport.observeCardLoading();
Disposable dsp6 = loadCardObservable
    .observeOn(Schedulers.io())
    .map(new Function<Card, LoadGroupOp>() {

    @Override
    public LoadGroupOp apply(Card card) throws Exception {
        Log.d("TangramActivity", "loadCardObservable in map");
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        JSONArray cells = new JSONArray();
        for (int i = 0; i < 10; i++) {
            try {
                JSONObject obj = new JSONObject();
                obj.put("type", 1);
                obj.put("msg", "async loaded");
                JSONObject style = new JSONObject();
                style.put("bgColor", "#FF1111");
                obj.put("style", style.toString());
                cells.put(obj);
            } catch (JSONException e) {
                e.printStackTrace();
            }
        }
        List<BaseCell> result = engine.parseComponent(cells);
        LoadGroupOp loadGroupOp = new LoadGroupOp(card, result);
        return loadGroupOp;
    }
}).observeOn(AndroidSchedulers.mainThread())
    .subscribe(cardLoadSupport.asDoLoadFinishConsumer());
mCompositeDisposable.add(dsp6);
```

+ `public Observable<Card> observeCardLoadingMore()`：Card 加载更多的 `Observable`，业务方拿到它之后可以对接网络加载、数据解析等一系列流程，然后将结果封装成一个 `LoadMoreOp `，提供到下面的接口使用；
+ `public Consumer<LoadMoreOp> asDoLoadMoreFinishConsumer()`：异步加载更多的 `Consumer`，当然你也可以选择不使用它，自己定义一个 `Consumer`；参数 `LoadMoreOp ` 里封装了原始的 `Card` 对象，一个 `List<BaseCell>` 结果，一个是否还有下一页的参数，true 代表还有下一页，否则代表结束；

举个例子：

```
Observable<Card> loadMoreObservable = cardLoadSupport.observeCardLoadingMore();
Disposable dsp7 = loadMoreObservable.observeOn(Schedulers.io())
    .map(new Function<Card, LoadMoreOp>() {
        @Override
        public LoadMoreOp apply(Card card) throws Exception {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            Log.w("TangramActivity", "loadMoreObservable " + card.load + " page " + card.page);
            JSONArray cells = new JSONArray();
            for (int i = 0; i < 9; i++) {
                try {
                    JSONObject obj = new JSONObject();
                    obj.put("type", 1);
                    obj.put("msg", "async page loaded, params: " + card.getParams().toString());
                    cells.put(obj);
                } catch (JSONException e) {
                    e.printStackTrace();
                }
            }
            List<BaseCell> cs = engine.parseComponent(cells);
            //mock loading 6 pages
            LoadMoreOp loadMoreOp = new LoadMoreOp(card, cs, card.page != 6);
            return loadMoreOp;
        }
    }).observeOn(AndroidSchedulers.mainThread()).subscribe(cardLoadSupport.asDoLoadMoreFinishConsumer());
mCompositeDisposable.add(dsp7);
```

## 输入数据的响应式接入

Tangram 是由数据驱动页面展示，它提供了一系列接口用来接收数据从而展示页面，包括整体刷新，局部更新等，这个环节可以作为一个数据流的终点来消费响应式流，因此设计了一系列 `Consumer` 接口来作为最终的观察者，响应数据变化。

### 整体页面刷新

### 插入卡片布局

### 插入组件

### 局部刷新

## 解析数据的支持

解析数据的过程就是从 json 数据到 `List<Card>` 或者 `List<BaseCell>` 的过程，这其实就是对应与响应式流程里的一个 map 过程。这里重构了解析逻辑，除了原来的解析成一个列表的接口，`TangramEngine` 新增了解析单个组件或者布局的接口。具体如下：

+ `public List<Card> parseData(@Nullable JSONArray data)`：解析出一个布局列表；
+ `public List<BaseCell> parseComponent(@Nullable JSONArray data)`：解析出一个组件列表；
+ `public List<BaseCell> parseComponent(@Nullable Card parent, @Nullable JSONArray data)`：解析出一个组件列表，提供父节点布局，支持嵌套布局地解析；
+ `public Card parseSingleData(@Nullable JSONObject data)`：解析出一个布局；
+ `public L parseSingleComponent(@Nullable C parent, @Nullable O data)`：解析出一个组件对象，提供父节点布局，支持嵌套布局地解析；

因此可以直接在 map 操作符里调用上述接口来转换；

也可以使用 compose 方法，调用 `TangramEngine` 里的 `ObservableTransformer` 相关接口：

+ `public ObservableTransformer<ParseGroupsOp, List<Card>> getGroupTransformer()`：解析出一个布局列表，`ParseGroupsOp` 封装了解析需要的原始数据和资源；
+ `public ObservableTransformer<ParseComponentsOp, List<BaseCell>> getComponentTransformer()`：解析出一个组件列表，`ParseComponentsOp ` 封装了解析需要的原始数据和资源；
+ `public ObservableTransformer<ParseSingleGroupOp, Card> getSingleGroupTransformer()`：解析出一个布局，`ParseSingleGroupOp ` 封装了解析需要的原始数据和资源；
+ `public ObservableTransformer<ParseSingleComponentOp, BaseCell> getSingleComponentTransformer()`：解析出一个组件对象，`ParseSingleComponentOp` 封装了解析需要的原始数据和资源；

## 组件绑定数据的支持

组件绑定数据的支持主要在于对 native 组件生命周期的管理，native 组件绑定数据有一个 `public void postBindView(BaseCell cell)` 和 `public void postUnBindView(BaseCell cell)` 时机，分别对应于组件进度屏幕绑定数据和，滑出屏幕待回收的时机。

当开启响应式特性的时候，Tangram 内部会维护每个组件的这两个生命周期事件，并根据这个生命周期来辅助支持组件里的绑定数据的时候，响应式流自动取消订阅的能力。如果你用过 RxLifeCycle，就知道这个功能的作用了，其工作原理就是参考自 RxLifeCycle 的设计思路。

关键使用方法就是：

1. 获取组件的 lifeCycleProvider  对象：`LifeCycleProviderImpl<BDE> lifeCycleProvider = cell.getLifeCycleProvider();`；
2. 在响应式流里绑定流到组件滑出屏幕的时机：`compose(lifeCycleProvider.<String>bindUntil(BDE.UNBIND))`；

举个例子：

```
public void postBindView(BaseCell cell) {
    if (cell.serviceManager != null && cell.serviceManager.supportRx()) {
        LifeCycleProviderImpl<BDE> lifeCycleProvider = cell.getLifeCycleProvider();
        Observable.just(cell).map(new Function<BaseCell, String>() {
            @Override
            public String apply(BaseCell cell) throws Exception {
            	//模拟绑定数据过程中耗时的任务，可以异步处理；
                Thread.sleep(500L);
                int pos = cell.pos;
                return cell.id + " pos: " + pos + " " + cell.parent.getClass().getSimpleName() + " " + cell
                    .optStringParam("title");
            }
        }).subscribeOn(Schedulers.computation())
        .observeOn(AndroidSchedulers.mainThread())
        //绑定该流到组件滑出屏幕为止，也就是组件内部触发了 BDE.UNBIND 事件；这样可以自动取消订阅，防止内存泄露；
        .compose(lifeCycleProvider.<String>bindUntil(BDE.UNBIND))
        .subscribe(new Consumer<String>() {
            @Override
            public void accept(String s) throws Exception {
                titleTextView.setText(s);
            }
        });
    }
}
```

## 页面加载案例