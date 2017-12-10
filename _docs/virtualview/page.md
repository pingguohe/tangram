---
title: "Page"
permalink: /docs/virtualview/page
excerpt: "Page"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<Page>`

翻页滚动的组件，与`Scroller`和`Slider`的区别在于它是有页面效果，一页一页滚动，而`Scroller`、`Slider`是可连续滚动。

### 子组件
容器组件

### 属性

|名称|类型|默认值|描述|
|---|---|---|---|
|orientation|enum(H/V)|V|滚动方向，H：水平滚动，V：垂直滚动|
|onPageFlip|expr|无|表达式，翻页的时候触发加载的逻辑|
|autoSwitch|boolean|false|true：自动翻页滚动，false：不自动滚动|
|canSlide|boolean|true|true：响应手势滑动，false：不响应手势滑动|
|stayTime|int|2000|单位ms，自动滚动的间隔|
|autoSwitchTime|int|500|单位ms，滚动时动画的持续时间|
|animatorTime|int|100|单位ms，手势滑动时，松手后复位或者滚动到下一页的动画时间|
|dataTag|jsonArray|无|容器内组件数据，描述内部子组件的类型与数据|
|layoutOrientation|enum(normal/reverse)|normal|水平滚动：normal是从左往右布局，reverse是从右往左布局；垂直滚动:normal是从上往下布局，reverse是从下往上布局|
nimationStyle|enum(linear/decelerate/accelerate/accelerateDecelerate/spring)|linear|线性、减速、加速、先加速后减速、弹簧|

### 事件

支持子组件显示时的exposure。

### 代码示例

```
<Page
    autoSwitch="true"
    dataTag="vList"
    stayTime="2500"
    canSlide="false"
    orientation="V"
    layoutRatio="1"
    layoutWidth="0"
    layoutHeight="33.5" />
``` 

### 图示

![](https://gw.alicdn.com/tfs/TB1p8vrfiqAXuNjy1XdXXaYcVXa-272-480.gif)