---
title: "RatioLayout"
permalink: /docs/virtualview/ratiolayout
excerpt: "RatioLayout"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<RatioLayout>`

虚拟化的线性布局，其子组件支持写layoutRatio属性来声明在父容器空间上占用的比例，声明过layoutRatio的组件按比例分配宽或高，未声明layoutRatio的组件占用剩余的空间。不支持通过数据动态创建子组件，需要在布局模板里直接写子组件。

### 子组件
容器组件

### 属性

|名称|类型|默认值|描述|
|---|---|---|---|
|orientation|enum(H/V)|H|水平方向布局，垂直方向布局|
|layoutParam.layoutRatio|int/float|0.0|在水平方向或者垂直方向的权重|

### 事件

支持exposure

### 代码示例

```
<RatioLayout
        flag="flag_draw|flag_event|flag_exposure|flag_clickable"
        action="action"
        orientation="V"
        layoutWidth="wrap_content"
        layoutHeight="match_parent">
    <RatioLayout
            orientation="H"
            layoutWidth="wrap_content"
            layoutMarginBottom="1.5"
            layoutRatio="1"
            layoutHeight="0"
            flag="flag_draw">
        <VText
                dataTag="title1"
                textSize="12"
                textColor="#333333"
                layoutGravity="v_center"
                layoutRatio="1"
                layoutWidth="wrap_content"
                layoutHeight="wrap_content"
                ellipsize="end"/>
    </RatioLayout>

    <RatioLayout
            orientation="H"
            layoutWidth="wrap_content"
            layoutMarginTop="1.5"
            gravity="v_center"
            layoutRatio="1"
            layoutHeight="0"
            flag="flag_draw">
        <VText
                dataTag="title2"
                textSize="12"
                textColor="#333333"
                layoutGravity="v_center"
                layoutRatio="1"
                layoutWidth="wrap_content"
                layoutHeight="wrap_content"
                ellipsize="end"/>
    </RatioLayout>

</RatioLayout>
``` 

### 图示

![](https://gw.alicdn.com/tfs/TB1bwTpfiqAXuNjy1XdXXaYcVXa-270-480.png)

![](https://gw.alicdn.com/tfs/TB1cwTpfiqAXuNjy1XdXXaYcVXa-270-480.png)