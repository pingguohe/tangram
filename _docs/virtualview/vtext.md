---
title: "VText"
permalink: /docs/virtualview/vtext
excerpt: "VText"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<VText>`

虚拟化实现的文本组件，精简了大量原生文本的特性，只支持以下几个特性：字体颜色、字号大小、字体粗细、支持文本对齐，只能单行显示，不支持分多行。不支持响应点击事件。

### 子组件
原子组件

### 属性

|名称|类型|默认值|描述|
|---|---|---|---|
|text|string|无|文本内容|
|textColor|int|黑色|字体颜色|
|textSize|int/float|20dp|字号大小|
|textStyle|enum(norma/bold/italic/strike)|normal|normal：默认样式，bold：加粗，itlaic：斜体，strike：删除线|
|gravity|enum(left/right/top/bottom/v_center/h_center)|left\|top|描述内容的对齐，比如文字在文本组件里的位置、原子组件在容器里的位置，left：靠左，right：靠右，top：靠上，bottom：靠底，v_center：垂直方向居中，h_center：水平方向居中，可用`或`组合描述(iOS暂只支持水平方向)|

### 事件

不支持click、longclick。

### 代码示例

```
<VText
    id="1"
    text="title"
    textSize="12"
    textColor="#333333"
    layoutWidth="wrap_content"
    layoutHeight="wrap_content" />
``` 

### 图示

![](https://gw.alicdn.com/tfs/TB1EK_ofiqAXuNjy1XdXXaYcVXa-270-480.png)