---
title: "Slider"
permalink: /docs/virtualview/slider
excerpt: "Slider"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<Slider>`

实体水平滚动的组件容器，支持内部组件的回收复用。

### 子组件
容器组件

### 属性

|名称|类型|默认值|描述|支持表达式|
|---|---|---|---|---|
|orientation|enum(H/V)|H|滚动方向，H：水平滚动，V：垂直滚动（暂不支持）|否|
|itemWidth|int/float|0|容器内组件的宽度，必填|否|
|span|int/float|0|item之间的间距|否|
|onScroll|expr|无|表达式，滚动一次回调执行的逻辑|否|
|dataTag|jsonArray|无|容器内组件数据，描述内部子组件的类型与数据|是|

### 事件

无

### 代码示例

```
<Slider
    paddingLeft="10"
    paddingRight="10"
    id="10"
    dataTag="vList"
    orientation="H"
    layoutWidth="match_parent"
    layoutHeight="130" />
```

### 图示

![](https://gw.alicdn.com/tfs/TB1HnmchLDH8KJjy1XcXXcpdXXa-272-480.gif)
