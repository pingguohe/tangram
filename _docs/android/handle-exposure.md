---
title: "处理曝光"
permalink: /docs/android/handle-exposure
excerpt: "处理曝光"
modified: 2016-11-03T10:01:43-04:00
redirect_from:
  - /theme-setup/
---

Tangram 认为的组件曝光的时机就是被 RecyclerView 的 Adapter 绑定数据的那个时候，也就是即将滑动到屏幕范围内。在这个时候业务上可能需要有一些处理，因此提供了接口定义并整合到框架里 —— ```ExposureSupport```。它定义了3个层面的曝光接口，一是曝光布局，二是曝光组件整体区域，三是曝光组件局部区域。业务方实现子类，并针对三个层面的曝光做分别的实现。同样有几点说明：

布局的整体曝光回调接口是，```public abstract void onExposure(@NonNull Card card, int offset, int position);```。

组件的整体曝光稍微复杂一点：

+ 建议开启优化模式（同样会绕过反射） —— ```setOptimizedMode(true)```，这样曝光接口被统一回调到```public void defaultExposureCell(@NonNull View targetView, @NonNull BaseCell cell, int type)```方法里，业务方根据组件类型、View的类型即组件类型做曝光处理。

+ 如果不开启优化模式，```ExposureSupport ```内部会跟进被点击的组件类型和 View 的类型来做路由，它要求业务方在```ExposureSupport ```的实现类里为每个组件的点击提供以 **```onExposureXXX```** 或者 **```onXXXExposure```** 为命名规范的点击处理方法，并且参数列表是```View targetView, BaseCell cell, int type```。这种方式效率会低一些，针对采用自定义 model 开发组件的时候有用。

组件的局部区域曝光和整体曝光类似：

+ 建议开启优化模式（同样会绕过反射） —— ```setOptimizedMode(true)```，这样曝光接口被统一回调到```public void defaultTrace(@NonNull View targetView, @NonNull BaseCell cell, int type)```方法里，业务方根据组件类型、View的类型即组件类型做曝光处理。
+ 如果不开启优化模式，```ExposureSupport ```内部会跟进被点击的组件类型和 View 的类型来做路由，它要求业务方在```ExposureSupport ```的实现类里为每个组件的点击提供以 **```onTraceXXX```** 或者 **```onXXXTrace```** 为命名规范的点击处理方法，并且参数列表是```View targetView, BaseCell cell, int type```。这种方式效率会低一些，针对采用自定义 model 开发组件的时候有用。前两个层面的曝光调用都是框架层调用，而组件局部曝光，则需要业务方在组件逻辑里自行调用，方式如下：

```
ExposureSupport exposureSupport = serviceManager.getService(ExposureSupport.class);
if (exposureSupport != null) {
    exposureSupport.onTrace(view, cell, type);
}
```