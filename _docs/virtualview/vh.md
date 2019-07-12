---
title: "VH"
permalink: /docs/virtualview/vh
excerpt: "VH"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<VH>`

实体的线性布局，支持通过数据动态创建子组件。

### 子组件
容器组件

### 属性

|名称|类型|默认值|描述|支持表达式|
|---|---|---|---|---|
|orientation|enum(H/V)|H|水平方向布局，垂直方向布局|否|
|itemWidth|int/float|0|组件的宽度，当itemWidth=0时，采用wrap_content的方式计算组件宽度|是|
|itemHeight|int/float|0|组件的高度，当itemHeight =0时，采用wrap_content的方式计算组件高度|是|
|itemMargin|int/float|0|组件间距|是|
|dataTag|jsonArray|无|容器内组件数据，描述内部子组件的类型与数据|是|

### 事件

支持子组件显示时的exposure。

### 代码示例

```
<VH
    dataTag="vList"
    orientation="V"
    layoutWidth="match_parent"
    layoutHeight="wrap_content"/>
```

### 图示

![](https://gw.alicdn.com/tfs/TB1pwTpfiqAXuNjy1XdXXaYcVXa-270-480.png)