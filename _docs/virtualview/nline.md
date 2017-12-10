---
title: "NLine"
permalink: /docs/virtualview/nline
excerpt: "NLine"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<NLine>`

实体进度条组件，支持实线、虚线，可以使用横向显示、竖向显示。

### 子组件
原子组件

### 属性

|名称|类型|默认值|描述|
|---|---|---|---|
|color|string|black|线条颜色|
|paintWidth|int|1|线条宽度|
|orientation|enum(H/V)|H|线条方向，H：横向，V：竖向|
|style|enum(solid/dash)|solid|线条样式，solid：实线，dash：虚线|
|dashEffect|string|[3, 5, 3, 5]|虚线样式数组，元素必须是偶数个，以实线宽度-虚线宽度...的顺序写|

### 事件

不支持click、longclick。

### 代码示例

```
<NLine
    layoutWidth="match_parent"
    layoutHeight="0.5"
    paintWidth="0.5"
    orientation="H"
    style="dash"
    dashEffect="[10,6,10,6]"
    color="#D0D0D0" />
``` 

### 图示

![](https://gw.alicdn.com/tfs/TB1JJyXhLDH8KJjy1XcXXcpdXXa-270-480.png)