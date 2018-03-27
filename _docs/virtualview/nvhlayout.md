---
title: "NVHLayout"
permalink: /docs/virtualview/nvhlayout
excerpt: "NVHLayout"
modified: 2018-03-27T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<NVHLayout>`

实体的线性布局，主要提供了布局协议，不支持通过数据动态创建子组件，需要在布局模板里直接写子组件。虚拟的子组件会绘制到它的 canvas 上而不是整个大容器的 canvas，实体的子组件也会添加到它内部，因此在 borderRadius 属性的作用下会裁剪内部区域，除此之外与 `VHLayout` 的功能完全一致。

### 子组件
容器组件

### 属性

|名称|类型|默认值|描述|
|---|---|---|---|
|orientation|enum(H/V)|H|水平方向布局，垂直方向布局|

### 事件

支持exposure。

### 代码示例

```
<NVHLayout
	 orientation="V"
    layoutWidth="match_parent"
    layoutHeight="wrap_content">
    <VText
	    id="1"
	    dataTag="title"
	    textSize="12"
	    textColor="#333333"
	    layoutWidth="wrap_content"
	    layoutHeight="wrap_content" />
    <VText
	    id="2"
	    dataTag="title"
	    textSize="12"
	    textColor="#333333"
	    layoutWidth="wrap_content"
	    layoutHeight="wrap_content" />
</NVHLayout>
```  

### 图示

![](https://gw.alicdn.com/tfs/TB1Ns1ahLDH8KJjy1XcXXcpdXXa-270-480.png)