---
title: "VHLayout"
permalink: /docs/virtualview/vhlayout
excerpt: "VHLayout"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<VHLayout>`

虚拟化的线性布局，与`VH`的区别是它是虚拟的，主要提供了布局协议，不支持通过数据动态创建子组件，需要在布局模板里直接写子组件。

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
<VHLayout
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
</VHLayout>
```  

### 图示

![](https://gw.alicdn.com/tfs/TB1Ns1ahLDH8KJjy1XcXXcpdXXa-270-480.png)