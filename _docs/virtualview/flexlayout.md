---
title: "FlexLayout"
permalink: /docs/virtualview/flexlayout
excerpt: "FlexLayout"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<FlexLayout>`

虚拟化的Flex布局，Flex协议的虚拟化实现。但是只实现了部分功能，与标准的Flex布局协议还存在一些差距。不支持通过数据动态创建子组件，需要在布局模板里直接写子组件。

### 子组件
容器组件

### 属性

|名称|类型|默认值|描述|
|---|---|---|---|
|flex-direction|enum(row/column/row-reverse/column-reverse)|row|布局方向(主轴方向)。|
|flex-wrap|enum(nowrap/wrap/wrap-reverse)|nowrap|沿主轴方向无法完全排布时候，是否进行换行。|
|justify-content|enum(flex-start/center/flex-end/space-between/space-around|flex-start|项目沿主轴方向的对齐方式。|
|align-content|stretch/flex-start/center/flex-end/space-between/space-around|stretch|对父容器内多个行在交叉轴上进行排列。|
|align-items|stretch/flex-start/center/flex-end/baseline|stretch|项目沿交叉轴方向的对其方式。|

在子组件上的flex属性协议还未实现

### 事件

支持exposure。

### 代码示例
 