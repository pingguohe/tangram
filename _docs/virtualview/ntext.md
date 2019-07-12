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

|名称|类型|默认值|描述|支持表达式|
|---|---|---|---|---|
|text|string|无|文本内容|是|
|textColor|int|黑色|字体颜色|是|
|textSize|int/float|20dp|字号大小|是|
|textStyle|enum(normal/bold/italic/strike/underline)|normal|normal：默认样式，bold：加粗，itlaic：斜体，strike：删除线，underline：下划线|是|
|ellipsize|enum(none/start/marquee/middle/end)|none|截断方式(iOS不支持走马灯)|否|
|lines|int|1|固定行数，设为0表示不固定行数|是|
|maxLines|int|无|最大行数，需要配合lines=0使用|否|
|lineSpaceMultiplier(iOS暂不支持)|int/float|1|行高放大系数，每一行文本高度计算会乘以这个系数|否|
|lineSpaceExtra(iOS暂不支持)|int/float|0|行高额外空间，每一行文本高度计算会加上这个值|否|
|supportHTMLStyle(iOS不支持)|boolean|false|true：采用富文本方式加载，false：按照普通文本加载|否|
|gravity|enum(left/right/top/bottom/v_center/h_center)|left\|top|描述内容的对齐，比如文字在文本组件里的位置、原子组件在容器里的位置，left：靠左，right：靠右，top：靠上，bottom：靠底，v_center：垂直方向居中，h_center：水平方向居中，可用`或`组合描述(iOS暂只支持水平方向)|否|

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

### 图示

![](https://gw.alicdn.com/tfs/TB15tfofiqAXuNjy1XdXXaYcVXa-270-480.png)
