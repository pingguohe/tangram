---
title: "NText"
permalink: /docs/virtualview/ntext
excerpt: "NText"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<NText>`

原生实现的文本组件，通过模板里定义可绑定以下属性：字体颜色、字号大小、字体粗细、支持文本对齐，行数，最大行数，行间距，行间距系数，截断方式。

### 子组件
原子组件

### 属性

|名称|类型|默认值|描述|
|---|---|---|---|
|text|string|无|文本内容|
|textColor|int|黑色|字体颜色|
|textSize|int/float|20dp|字号大小|
|textStyle|enum(normal/bold/italic/strike)|normal|normal：默认样式，bold：加粗，itlaic：斜体，strike：横线|
|ellipsize|enum(start/marquee/middle/end)|无|截断方式|
|lines|int|1|行数|
|maxLines|int|无|最大行数|
|lineSpaceMultiplier|int/float|1|行高放大系数，每一行文本高度计算会乘以这个系数|
|lineSpaceExtra|int/float|0|行高额外空间，每一行文本高度计算会加上这个值|
|supportHTMLStyle|boolean|false|true：采用富文本方式加载，false：按照普通文本加载|

### 事件

支持click、longclick、exposure

### 代码示例

```
<NText
    id="1"
    text="title"
    textSize="12"
    textColor="#333333"
    layoutWidth="wrap_content"
    layoutHeight="wrap_content"
    lineSpaceMultiplier="1.1"
    lines="2"
    flag="flag_event|flag_exposure|flag_clickable"
 />
``` 