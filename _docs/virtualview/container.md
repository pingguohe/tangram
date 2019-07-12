---
title: "Container"
permalink: /docs/virtualview/container
excerpt: "Container"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<Container>`

虚拟化的布局容器，无特殊的布局逻辑，主要是在其他虚拟组件上加一层坑，无特殊功能。支持通过数据动态创建子组件。

### 子组件
容器组件

### 属性

|名称|类型|默认值|描述||
|---|---|---|---|---|
|order|int|0|与dataTag配合使用|否|
|dataTag|jsonArray|无|容器内组件数据，描述内部子组件的类型与数据，取jsonArray的第order个数据来实例化组件加到容器里|是|

### 事件

支持子组件exposure

### 代码示例
