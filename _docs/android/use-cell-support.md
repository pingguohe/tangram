---
title: "使用组件辅助模块"
permalink: /docs/android/use-cell-support
excerpt: "使用组件辅助模块"
modified: 2016-11-03T10:01:43-04:00
sidebar:
  title: "Android 使用指南"
  nav: android-docs
---

每个组件里可能会有一些重复逻辑，特别是采用通用 model 开发组件的时候，组件的 View 之间一般没有继承体系，为了解决这种问题，建议业务也像```SimpleClickSupport```或者```ExposureSupport```一样将逻辑模块化，通过 serviceManager 注册到框里提供给组件使用。框架里还提供了一个```CellSupport```，暴露了一些基本接口，业务方需要继承它并注册到 Tangram 里：

+ ```public abstract boolean isValid(BaseCell cell)```，解析Json数据的时候调用，提前判断组件是否合法有效；无效数据不会给框架去渲染；
+ ```public void bindView(BaseCell cell, View view)```，绑定数据前时调用，比如可以用来绑定contentDespcription，绑定点击事件；
+ ```public void postBindView(BaseCell cell, View view)```，绑定数据后调用；
+ ```public void unBindView(BaseCell cell, View view)```，解除数据绑定时调用，可做一些销毁逻辑；