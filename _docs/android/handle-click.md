---
title: "处理点击"
permalink: /docs/android/handle-click
excerpt: "处理点击"
modified: 2016-11-03T10:01:43-04:00
sidebar:
  title: "Android 使用指南"
  nav: android-docs
---

组件 View 的点击处理，可以实现```SimpleClickSupport```，在组件 View 内部调用```setOnClickListener(cell);```，那么组件的点击行为会被回调到```SimpleClickSupport```里统一处理。组件 model 内部逻辑是这样调用的：

```
SimpleClickSupport service = serviceManager.getService(SimpleClickSupport.class);
if (service != null) {
    service.onClick(v, this, this.pos);
}
```

当然使用```SimpleClickSupport```只是一种推荐的使用方式，业务方选择自定义的点击处理也是可以的。

选用```SimpleClickSupport```的时候有几个注意点：

+ 建议开启优化模式（同样会绕过反射） —— ```setOptimizedMode(true)```，这样点击会被统一回调到```public void defaultClick(View v, BaseCell cell, int pos)```方法里，业务方根据组件类型、View的类型即组件类型做点击处理。

+ 如果不开启优化模式，```SimpleClickSupport```内部会跟进被点击的组件类型和 View 的类型来做路由，它要求业务方在```SimpleClickSupport```的实现类里为每个组件的点击提供以 **```onClickXXX```** 或者 **```onXXXClick```** 为命名规范的点击处理方法，并且参数列表是```View targetView, BaseCell cell, int type```或者```View targetView, BaseCell cell, int type, Map<String, Object> params```。这种方式效率会低一些，针对采用自定义 model 开发组件的时候有用。