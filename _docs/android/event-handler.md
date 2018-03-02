---
title: "事件处理"
permalink: /docs/android/event-handler
excerpt: "事件处理"
modified: 2017-12-08T10:01:43-04:00
sidebar:
  title: "Android 使用指南"
  nav: android-docs
---

## 事件类型

VirtualView 里定义了几种常用类型事件：
+ 点击
+ 长按
+ 触摸
+ 曝光

为了方便与其他框架对接，目前三种类型的事件只会在实体 View 上触发，如果没有实体 View，那事件最终绑定到它的宿主容器上触发。

曝光事件是在组件绑定数据要准备显示的时候触发。

## 声明事件

这里有个约定，默认情况下组件是不触发事件的，只有在模板里显示地声明了才具备触发能力。事件声明属性字段是 `flag`，其值可以通过 `|` 组合一次性声明多个。对应于四种事件类型，模板里的枚举定义分别是：
+ `flag_clickable`
+ `flag_longclickable`
+ `flag_touchable`
+ `flag_exposure`

举个例子：`flag="flag_exposure|flag_clickable"`

## 触发事件

根据之前的约定，要触发事件的时候，要先判断一下是否声明了触发事件的标记。在组件解析、创建的时候，这些标记位都会被解析处理，并提供了几个接口直接访问，对应于四种事件类型，分别是：
+ `isClickable()`
+ `isLongClickable()`
+ `isTouchable()`
+ `supportExposure()`

触发方式：

```
if (isClickable()) {
	ret = mContext.getEventManager().emitEvent(EventManager.TYPE_Click, EventData.obtainData(mContext, this));
}
```

通过 `EventManager` 来发送事件，参数 1 是事件类型，参数 2 是封装了当前组件 model 对象（代码中的 this）的数据。

## 事件处理

事件处理就涉及业务逻辑了，需要外部注册处理器。也是通过 `EventManager` 来注册，一种类型的事件注册一个处理器就可以了，如果注册了多个，那么在事件触发的时候，会回调每一个处理器。

```
vafContext.getEventManager().register(EventManager.TYPE_Click, new IEventProcessor() {

    @Override
    public boolean process(EventData data) {
        //handle here
        return true;
    }
});
vafContext.getEventManager().register(EventManager.TYPE_Exposure, new IEventProcessor() {

    @Override
    public boolean process(EventData data) {
        //handle here
        return true;
    }
});
```

在处理函数里，可以获取到的资源有：

+ 触发事件的组件 model：`data.mVB`
+ 组件绑定的原始 JSON 数据：`(JSONObject)data.mVB.getViewCache().getComponentData();`

如果消化了事件，就返回 `true`，否则返回 `false`。

针对不同的控件，点击处理要做不同的逻辑，一般可以用以下方式来分发处理不同的点击事件：

`String action = data.mVB.getAction()`

获取所点击的控件的 action 字段，它是在 XML 目标里描述的，可以是一个常量值，也可从绑定的数据到数据。建议 actio 只是一个动作描述协议，比如跳转是一个 URL 格式的地址，其他的可以是自定义的协议；然后针对不同的协议地址，注册不同的处理模块，在自定义的 `IEventProcessor` 里分发处理。