---
title: "组件开发"
permalink: /docs/android/develop-component
excerpt: "组件开发"
modified: 2016-11-03T10:01:43-04:00
redirect_from:
  - /theme-setup/
---

组件分为两层：model 和 View。Tangram 里提供了通用 model 类型`BaseCell`，因此开发组件有两个选择：

1. 采用通用 model，开发自定义 View。
2. 采用自定义 model 和自定义 View

## 通用 model 开发组件

无需关心 model，主要是开发自定义 View。

### 自定义 View 也有两种实现规范：

1. 组件开发模式一，避免了反射调用，性能上更优：
	+ 实现一个自定义View，比如```XXTangramView```；
	+ 实现接口```ITangramViewLifeCycle ```，包含三个方法：
		+ ```public void cellInited(BaseCell cell)```，绑定数据前调用；
		+ ```public void postBindView(BaseCell cell)```，绑定数据时机；
		+ ```public void postUnBindView(BaseCell cell)```，滑出屏幕，解除绑定；
	+ 主要在上述```public void postBindView(BaseCell cell)```完成组件业务逻辑；
2. 组件开发模式二，动态绑定数据：
	+ 实现一个自定义View，比如```XXTangramView```；
	+ 必须添加三个方法，以```@CellRender```注解，功能同模式一，只是被反射调用：
		+ ```public void cellInited(BaseCell cell)```；
		+ ```public void postBindView(BaseCell cell)```；
		+ ```public void postUnBindView(BaseCell cell)```；
	+ 还可以为组件每个属性实现单独的设置方法，而不是在```postBindView```方法里一次性绑定数据，这些方法必须以```@CellRender```注解，框架会在```public void postBindView(BaseCell cell)```调用之前调用这些数据绑定方法；举个例子：
	
	```
	@CellRender(key = "pos") //这里的key=pos表示让框架取原始json数据里pos字段的值传给该方法，原始数据里没有该字段，参数值会是该类型的默认值
    public void setPosition(int pos) {//这里pos的类型要注意，是框架会以该方法声明的类型来取获取原始数据
        textView.setText(cell.id + " pos: " + pos + " " + cell.parent + " " + cell.optParam("msg"));

        if (pos > 57) {
            textView.setBackgroundColor(0x66cccf00 + (pos - 50) * 128);
        } else if (pos % 2 == 0) {
            textView.setBackgroundColor(0xaaaaff55);
        } else {
            textView.setBackgroundColor(0xccfafafa);
        }
    }
	```

这种方式开发的组件在页面初始化的时候调用```public <V extends View> void registerCell(int type, Class<V> viewClz)```方法进行注册。
	
### 在自定义 View 里访问 model 数据的访问：

组件的View对应于一个统一的model，类型是```BaseCell```，要在View里访问 model 的数据，提供了以下一些方法：

```
public boolean hasParam(String key)

public Object optParam(String key)
	
public long optLongParam(String key)
	
public int optIntParam(String key)
	
public Stirng optStringParam(String key)
	
public double optDoubleParam(String key)
	
public boolean optBoolParam(String key)
	
public JsonObject optJsonObjectParam(String key)
	
public JsonArray optJsonArrayParam(String key)
	
```	

这些方法都会先访问```BaseCell```里持有的原始json数据，同时支持访问style节点下的属性；

## 自定义 model 开发组件

采用通用的 model 开发组件，只需要写 View 就可以了，然而需要在每次绑定数据的时候都要取原始 json 里解析一下字段。有时候一个业务方会有一些通用的业务字段定义，每个组件里重复解析会让代码显得冗余，因此也提供了注册自定义 model 的兼容模式开发组件。这个时候就需要写自定义 model 和自定义 View 两部分了。

1. 自定义 model 开发
	+ 实现一个自定义 model 类，继承自 ```BaseCell```，比如```TestCell```。
	+ 实现以下几个方法：

	```
	/** 解析数据业务数据，可以将解析值缓存到成员变量里 */
	public void parseWith(JSONObject data)
	
	/** 解析数据样式数据，可以将解析值缓存到成员变量里 */
	public void parseStyle(@Nullable JSONObject data)
	
	/** 绑定数据到自定义 View */
	public void bindView(@NonNull V view)
	
	/** 绑定数据到 View 之后，可选实现 */
	public void postBindView(@NonNull V view)
	
	/** 校验原始数据，检查组件的合法性 */
	public boolean isValid()
	```
2. 自定义 View 开发
	+ 没有特殊之处；

这种方式开发的组件在页面初始化的时候调用```public <V extends View> void registerCell(int type, @NonNull Class<? extends BaseCell> cellClz, @NonNull Class<V> viewClz)```方法进行注册。
	
## 对比两种开发方式
主要区别在于前者开发自定义 View，后者开发自定义 model。

## 在组件中使用扩展模块
按照上述步骤开发组件，只能完成了基本的数据绑定部分，而真正的业务逻辑还要进一步处理，比如组件的点击、图片的加载等等杂七杂八的业务逻辑；为了解耦 Tangram 与业务功能模块，Tangram 内部并不处理这些逻辑，而是让外部实现并注册到 Tangram 里的serviceManager里。在组件里的 model 里就可以通过它来获取业务功能模块。

注册的时候调用```TangramEngine```的```public <S> void register(@NonNull Class<S> type, @NonNull S service)```方法；

在使用的时候可以从组件（卡片也有）model 里拿到service 对象，调用```serviceManager.getService(XXX.class)```方法；

对于一些常用的功能模块，Tangram 内部定义了接口，并在初始化的时候注册过；而业务方自定义的功能模块则按照规范注册即可。这里介绍内置的扩展功能模块。

### 点击处理

组件 View 的点击处理，可以实现```SimpleClickSupport```，在组件 View 内部调用```setOnClickListener(cell);```，那么组件的点击行为会被回调到```SimpleClickSupport```里统一处理。组件 model 内部逻辑是这样调用的：

```
SimpleClickSupport service = serviceManager.getService(SimpleClickSupport.class);
if (service != null) {
    service.onClick(v, this, this.pos);
}
```

当然使用```SimpleClickSupport```只是一种推荐的使用方式，业务方选择自定义的点击处理也是可以的。

选用```SimpleClickSupport```的时候有几个注意点：建议开启优化模式（同样会绕过反射） —— ```setOptimizedMode(true)```，这样点击会被统一回调到```public void defaultClick(View v, BaseCell cell, int pos)```方法里，业务方根据组件类型、View的类型即组件类型做点击处理。如果不开启优化模式，```SimpleClickSupport```内部会跟进被点击的组件类型和 View 的类型来做路由，它要求业务方在```SimpleClickSupport```的实现类里为每个组件的点击提供以 **```onClickXXX```** 或者 **```onXXXClick```** 为命名规范的点击处理方法，并且参数列表是```View targetView, BaseCell cell, int type```或者```View targetView, BaseCell cell, int type, Map<String, Object> params```。这种方式效率会低一些，针对采用自定义 model 开发组件的时候有用。

### 曝光处理

Tangram 认为的组件曝光的时机就是被 RecyclerView 的 Adapter 绑定数据的那个时候，也就是即将滑动到屏幕范围内。在这个时候业务上可能需要有一些处理，因此提供了接口定义并整合到框架里 —— ```ExposureSupport```。它定义了3个层面的曝光接口，一是曝光卡片，二是曝光组件整体区域，三是曝光组件局部区域。业务方实现子类，并针对三个层面的曝光做分别的实现。同样有几点说明：

卡片的整体曝光回调接口是，```public abstract void onExposure(@NonNull Card card, int offset, int position);```。

组件的整体曝光稍微复杂一点，建议开启优化模式（同样会绕过反射） —— ```setOptimizedMode(true)```，这样曝光接口被统一回调到```public void defaultExposureCell(@NonNull View targetView, @NonNull BaseCell cell, int type)```方法里，业务方根据组件类型、View的类型即组件类型做曝光处理。如果不开启优化模式，```ExposureSupport ```内部会跟进被点击的组件类型和 View 的类型来做路由，它要求业务方在```ExposureSupport ```的实现类里为每个组件的点击提供以 **```onExposureXXX```** 或者 **```onXXXExposure```** 为命名规范的点击处理方法，并且参数列表是```View targetView, BaseCell cell, int type```。这种方式效率会低一些，针对采用自定义 model 开发组件的时候有用。

组件的局部区域曝光和整体曝光类似，建议开启优化模式（同样会绕过反射） —— ```setOptimizedMode(true)```，这样曝光接口被统一回调到```public void defaultTrace(@NonNull View targetView, @NonNull BaseCell cell, int type)```方法里，业务方根据组件类型、View的类型即组件类型做曝光处理。如果不开启优化模式，```ExposureSupport ```内部会跟进被点击的组件类型和 View 的类型来做路由，它要求业务方在```ExposureSupport ```的实现类里为每个组件的点击提供以 **```onTraceXXX```** 或者 **```onXXXTrace```** 为命名规范的点击处理方法，并且参数列表是```View targetView, BaseCell cell, int type```。这种方式效率会低一些，针对采用自定义 model 开发组件的时候有用。前两个层面的曝光调用都是框架层调用，而组件局部曝光，则需要业务方在组件逻辑里自行调用，方式如下：

```
ExposureSupport exposureSupport = serviceManager.getService(ExposureSupport.class);
if (exposureSupport != null) {
    exposureSupport.onTrace(view, cell, type);
}
```

### 定时器模块使用
组件的业务逻辑里难免有涉及到定数触发的逻辑，比如倒计时、定时滚动。Tangram 内置了定时器模块，可以全局复用，防止重复开发。以在组件里使用定时器为例：
在 bindView 或者 postBindView 方法里注册定时器：

```
if (cell.serviceManager != null) {
    TimerSupport timerSupport = cell.serviceManager.getService(TimerSupport.class);
    if (timerSupport != null && !timerSupport.isRegistered(this)) {
    	//第一个参数4是单位秒，第二个参数是接口回调，第三个参数是立即执行
        timerSupport.register(4, this, true);
    }
}
```

实现回调接口：

```
TimerSupport.OnTickListener
```

在 unbindView 或者 postUnbindView 的方法里要记得注销定时器：

```
if (cell.serviceManager != null) {
    TimerSupport timerSupport = cell.serviceManager.getService(TimerSupport.class);
    if (timerSupport != null) {
        timerSupport.unregister(this);
    }
}
```

### 组件开发辅助类
每个组件里可能会有一些重复逻辑，特别是采用通用 model 开发组件的时候，组件的 View 之间一般没有继承体系，为了解决这种问题，建议业务也像```SimpleClickSupport```或者```ExposureSupport```一样将逻辑模块化，通过 serviceManager 注册到框里提供给组件使用。框架里还提供了一个```CellSupport```，暴露了一些基本接口，做通用逻辑处理：

+ ```public abstract boolean isValid(BaseCell cell)```，解析Json数据的时候调用，提前判断组件是否合法有效；无效数据不会给框架去渲染；
+ ```public void bindView(BaseCell cell, View view)```，绑定数据前时调用，比如可以用来绑定contentDespcription，绑定点击事件；
+ ```public void postBindView(BaseCell cell, View view)```，绑定数据后调用；
+ ```public void unBindView(BaseCell cell, View view)```，解除数据绑定时调用，可做一些销毁逻辑；