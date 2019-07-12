---
title: "NRatioLayout"
permalink: /docs/virtualview/nratiolayout
excerpt: "NRatioLayout"
modified: 2018-03-27T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<NRatioLayout>`

实体的线性布局，其子组件支持写 layoutRatio 属性来声明在父容器空间上占用的比例，声明过layoutRatio的组件按比例分配宽或高，未声明 layoutRatio 的组件占用剩余的空间。不支持通过数据动态创建子组件，需要在布局模板里直接写子组件。虚拟的子组件会绘制到它的 canvas 上而不是整个大容器的 canvas，实体的子组件也会添加到它内部，因此在 borderRadius 属性的作用下会裁剪内部区域，除此之外与 `RatioLayout` 的功能完全一致。

### 子组件
容器组件

### 属性

|名称|类型|默认值|描述|支持表达式|
|---|---|---|---|---|
|orientation|enum(H/V)|H|水平方向布局，垂直方向布局|否|
|layoutRatio|int/float|0.0|在水平方向或者垂直方向的权重。这是个在基础元素上就有的属性，但是只有RatioLayout会使用它|否|

### 事件

支持exposure

### 代码示例

```
<NRatioLayout
        flag="flag_draw|flag_event|flag_exposure|flag_clickable"
        action="action"
        orientation="V"
        layoutWidth="wrap_content"
        layoutHeight="match_parent">
    <NRatioLayout
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
    </NRatioLayout>

    <NRatioLayout
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
    </NRatioLayout>

</NRatioLayout>
```

### 图示

![](https://gw.alicdn.com/tfs/TB1bwTpfiqAXuNjy1XdXXaYcVXa-270-480.png)

![](https://gw.alicdn.com/tfs/TB1cwTpfiqAXuNjy1XdXXaYcVXa-270-480.png)