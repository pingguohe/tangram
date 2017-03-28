---
title: "组件开发"
permalink: /docs/android/develop-component
excerpt: "组件开发"
modified: 2016-11-03T10:01:43-04:00
sidebar:
  title: "Android 使用指南"
  nav: android-docs
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