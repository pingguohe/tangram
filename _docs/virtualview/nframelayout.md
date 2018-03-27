---
title: "NFrameLayout"
permalink: /docs/virtualview/nframelayout
excerpt: "NFrameLayout"
modified: 2018-03-27T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<NFrameLayout>`

实体的帧布局，主要提供了布局协议，不支持通过数据动态创建子组件，需要在布局模板里直接写子组件。虚拟的子组件会绘制到它的 canvas 上而不是整个大容器的 canvas，实体的子组件也会添加到它内部，因此在 borderRadius 属性的作用下会裁剪内部区域，除此之外与 `FrameLayout` 的功能完全一致。

### 子组件
容器组件

### 属性

无自定义属性

### 事件

支持exposure

### 代码示例

```
<NFrameLayout
    flag="flag_draw"
    dataTag="container"
    layoutWidth="match_parent"
    layoutHeight="wrap_content">
    <VHLayout
        orientation="V"
        layoutWidth="match_parent"
        layoutHeight="wrap_content">
        <TMNImage
            flag="flag_draw|flag_event|flag_exposure|flag_clickable"
            id="0"
            dataTag="header"
            action="action"
            layoutWidth="match_parent"
            layoutHeight="wrap_content"
            autoDimDirection="X"
            autoDimX="750"
            autoDimY="110"/>

        <Grid
            dataTag="vList"
            colCount="2"
            layoutWidth="match_parent"
            layoutHeight="wrap_content" />
    </VHLayout>
</NFrameLayout>
```   

### 图示

![](https://gw.alicdn.com/tfs/TB1bMTpfiqAXuNjy1XdXXaYcVXa-270-480.png)