---
title: "VH2Layout"
permalink: /docs/virtualview/vh2layout
excerpt: "VH2Layout"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<VH2Layout>`

虚拟化的线性布局，与`VHLayout`的区别是它支持子组件分别从top、left、right、bottom四个方向进行布局。不支持通过数据动态创建子组件，需要在布局模板里直接写子组件。

### 子组件
容器组件

### 属性

|名称|类型|默认值|描述|
|---|---|---|---|
|orientation|enum(H/V)|H|水平方向布局，垂直方向布局|
|layoutParam.layoutDirection|enum(left/top/right/bottom)|left|当oriention=H时，支持组件分布从left与right两个方向布局，这样可以实现靠左、靠右的效果；当orientation=V时，支持组件分布从top与bottom两个方向布局，这样可以实现靠上、靠下的效果；默认值是left，在orientation=V的情况下，必须显示指定top或者bottom|

### 事件

支持exposure。

### 代码示例

```
<VH2Layout
    flag="flag_draw"
    layoutWidth="match_parent"
    layoutHeight="wrap_content"
    layoutRatio="74">
    <VText
        layoutWidth="wrap_content"
        layoutHeight="wrap_content"
        dataTag="price"
        textSize="15"
        textStyle="bold"
        layoutMarginLeft="12"
        layoutGravity="left|v_center"
        textColor="#231F1F"/>
    <NImage
        id="2"
        action="cart"
        dataTag="shopCart"
        flag="flag_event|flag_clickable"
        scaleType="fit_xy"
        layoutWidth="19"
        layoutHeight="19"
        layoutMarginRight="12"
        layoutGravity="right|v_center"
        layoutDirection="right"
        src="tm_homepage_shopping_cart"/>
</VH2Layout>
```  