---
title: "VLine"
permalink: /docs/virtualview/vline
excerpt: "VLine"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<VLine>`

虚拟化线条，支持实线、虚线，可以使用横向显示、竖向显示。

### 子组件
原子组件

### 属性

|名称|类型|默认值|描述|
|---|---|---|---|
|color|string|black|线条颜色|
|paintWidth|int|1|线条宽度|
|orientation|enum(H/V)|H|线条方向，H：横向，V：竖向|
|style(iOS暂不支持)|enum(solid/dash)|solid|线条样式，solid：实线，dash：虚线|
|dashEffect(iOS暂不支持)|string|[3, 5, 3, 5]|虚线样式数组，元素必须是偶数个，以实线宽度-虚线宽度...的顺序写|
|gravity|enum(left/right/top/bottom/v_center/h_center)|left\|top|描述内容的对齐，比如文字在文本组件里的位置、原子组件在容器里的位置，left：靠左，right：靠右，top：靠上，bottom：靠底，v_center：垂直方向居中，h_center：水平方向居中，可用`或`组合描述|

### 事件

不支持click、longclick。

### 代码示例

```
<VLine
    layoutWidth="match_parent"
    layoutHeight="0.5"
    paintWidth="0.5"
    orientation="H"
    style="dash"
    dashEffect="[10,6,10,6]"
    color="#D0D0D0" />
```  

### 图示

![](https://gw.alicdn.com/tfs/TB1ByrofiqAXuNjy1XdXXaYcVXa-270-480.png)