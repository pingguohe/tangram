---
title: "事件"
permalink: /docs/virtualview/event
excerpt: "事件"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
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