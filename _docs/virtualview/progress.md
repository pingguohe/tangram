---
title: "Progress"
permalink: /docs/virtualview/progress
excerpt: "Progress"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<Progress>`

虚拟化进度条组件，横向显示，整体进度由背景色显示，当前进度由前景色显示，总进度为组件的宽度。

### 子组件
原子组件

### 属性

|名称|类型|默认值|描述|
|---|---|---|---|
|initValue|int|0|当前进度，取值范围：[0, 组件宽度]|
|color|string|blue|进度条前景图颜色|

### 事件

不支持click、longclick。

### 代码示例

```
<Progress
    background="#FF0000"
    initValue="70"
    color="#986532"
    layoutWidth="match_parent"
    layoutHeight="20"/>
``` 

### 图示

![](https://gw.alicdn.com/tfs/TB1P4eXhLDH8KJjy1XcXXcpdXXa-270-480.png)