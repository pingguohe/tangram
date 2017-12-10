---
title: "Grid"
permalink: /docs/virtualview/grid
excerpt: "Grid"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<Grid>`

实体网格布局容器，支持通过数据动态创建子组件。

### 子组件
容器组件

### 属性

|名称|类型|默认值|描述|
|---|---|---|---|
|colCount|int|2|列数|
|itemHeight|int/float|0|组件的高度，当itemHeight =0时，采用wrap_content的方式计算组件高度|
|itemVerticalMargin|int/float|false|垂直间距，即行之间的间距|
|itemHorizontalMargin|int/float|0|水平间距，即列之间的间距|
|dataTag|jsonArray|无|容器内组件数据，描述内部子组件的类型与数据|

### 事件

支持子组件显示时的exposure。

### 代码示例

```
<Grid
    dataTag="vList"
    colCount="2"
    layoutWidth="match_parent"
    layoutHeight="wrap_content"/>
``` 

### 图示

![](https://gw.alicdn.com/tfs/TB1aI5ahLDH8KJjy1XcXXcpdXXa-270-480.png)