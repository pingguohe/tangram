---
title: "VGraph"
permalink: /docs/virtualview/vgraph
excerpt: "VGraph"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<VGraph>`

虚拟化图片组件，显示圆形、矩形。当为圆形时，区域宽高由layoutWidth、layoutParam或者diameterX、diameterY决定。

### 子组件
原子组件

### 属性

|名称|类型|默认值|描述|支持表达式|
|---|---|---|---|---|
|color|string|无|图形颜色|否|
|type|enum(circle/rect)|circle|图形形状，circle：圆形，rect：矩形|否|
|paintStyle|enum(stroke/fill)|无|填充方式，stroke：绘制边缘，fill：填充满图片内部区域|否|
|paintWidth|int|无|画笔的宽度|否|
|diameterX|int/float|0|图形区域的宽度，当type=circle时，半径 = diameterX / 2|否|
|diameterY|int/float|0|图形区域的高度|否|

### 事件

不支持click、longclick。

### 代码示例

```
<VGraph
    diameterX="50"
    diameterY="50"
    color="black"
    type="circle"
    paintStyle="fill"
    paintWidth="2"
    layoutWidth="100"
    layoutHeight="100"/>
```

### 图示

![](https://gw.alicdn.com/tfs/TB19fiXhLDH8KJjy1XcXXcpdXXa-270-480.png)

![](https://gw.alicdn.com/tfs/TB1Y2qXhLDH8KJjy1XcXXcpdXXa-270-480.png)

![](https://gw.alicdn.com/tfs/TB1Q2uXhLDH8KJjy1XcXXcpdXXa-270-480.png)