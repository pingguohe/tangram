---
title: "加载数据"
permalink: /docs/android/load-data
excerpt: "加载数据"
modified: 2016-11-03T10:01:43-04:00
sidebar:
  title: "Android 使用指南"
  nav: android-docs
---

Tangram 的页面的数据无法一次性返回，有些区块布局内的数据需要异步加载、甚至分页加载。Tangram 里内置了封装了异步加载的逻辑，需要各个层面配合完成，这里加以说明：

## 布局model 的load，loadParams，loadType，hasMore

+ load 是接口名称，表示这个布局需要执行异步加载的接口。
+ loadParams 是异步加载接口的常规参数字典，需要在调用接口时透传。
+ loadType 是异步加载的方式，-1表示需要异步加载，1表示需要异步加载且有分页。
+ hasMore 与 loadType 配合，当 loadType = 1 的时候表示分页是否结束。

```
{
	"id": "Shop",
	"load": "queryShop",
	"loadType": "-1",
	"type": "container-oneColumn"
}
```

## setPreLoadNumber(int preLoadNumber)

调用`TangramEngine`上的`setPreLoadNumber(int preLoadNumber)`方法，设置触发卡片预加载的时机，默认 preLoadNumber 是5，表示在滑动过程中，提前去触发可见范围之外5块布局以内的异步加载逻辑。可以通过这个接口调整预加载的范围。

## onScrolled()

在recyclerView 的 onScrollListener里调用`TangramEngine`上的`onScrolled()`方法，触发预加载的逻辑。

```
mRecyclerView.getRefreshableView().setOnScrollListener(new RecyclerView.OnScrollListener() {

    @Override
    public void onScrollStateChanged(RecyclerView recyclerView, int newState) {
    }

    @Override
    public void onScrolled(RecyclerView recyclerView, int dx, int dy) {
        super.onScrolled(recyclerView, dx, dy);
        engine.onScrolled();
    }
});
```

## CardLoadSupport与AsyncLoader，AsyncPageLoader

在配置完上述异步加载的基础设置之后，提供一个自定义的`CardLoadSupport`服务，该服务需要提供一个自定义的`AsyncLoader`和`AsyncPageLoader`。

`AsyncLoader`的`loadData(final Card card, @NonNull final LoadedCallback loadedCallback)`方法回调是卡片异步加载的入口。加载完成之后通过`LoadedCallback`的回写接口告知布局加载是否完成。

`AsyncPageLoader`的`loadData(int page, @NonNull Card card, @NonNull final LoadedCallback callback)`方法是回调卡片分页加载的入口。加载完成之后通过`LoadedCallback`的回写接口告知布局加载是否完成，是否还有下一页。

```
CardLoadSupport mCardLoadSupport = new CardLoadSupport(new AsyncLoader() {
    @Override
    public void loadData(final Card card, @NonNull final LoadedCallback callback) {
        //...
    }
    }, new AsyncPageLoader() {

    @Override
    public void loadData(int page, @NonNull Card card,
                         @NonNull LoadedCallback callback) {
        //...
    }
});
CardLoadSupport.setInitialPage(1);
engine.addCardLoadSupport(mCardLoadSupport);
```

AsyncLoader加载成功之后回调示例：

```
List<BaseCell> cells = engine.parseComponent(jsonObject.optJSONArray("items"));
callback.finish(cells);
```

AsyncPageLoader加载成功之后回调示例：

```
List<BaseCell> cells = engine.parseComponent(jsonObject.optJSONArray("items"));
callback.finish(cells, itemHashMore);
```