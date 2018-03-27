---
title: "NGridLayout"
permalink: /docs/virtualview/ngridlayout
excerpt: "NGridLayout"
modified: 2018-03-27T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<NGridLayout>`

实体的网格布局，主要提供了布局协议，不支持通过数据动态创建子组件，需要在布局模板里直接写子组件。虚拟的子组件会绘制到它的 canvas 上而不是整个大容器的 canvas，实体的子组件也会添加到它内部，因此在 borderRadius 属性的作用下会裁剪内部区域，除此之外与 `GridLayout` 的功能完全一致。

### 子组件
容器组件

### 属性

|名称|类型|默认值|描述|
|---|---|---|---|
|colCount|int|2|列数|
|itemHeight|int|0|组件的高度|
|itemVerticalMargin|int/float|0|垂直间距，即行之间的间距|
|itemHorizontalMargin|int/float|0|水平间距，即列之间的间距|

### 事件

支持exposure

### 代码示例

```
<NGridLayout
  colCount="2"
  layoutWidth="match_parent"
  layoutHeight="wrap_content"
  itemHorizontalMargin="5"
  itemVerticalMargin="5"
  background="#FFFFFF"
  paddingTop="7"
  paddingRight="7"
  paddingBottom="7"
  paddingLeft="7">
  <TMNImage
	layoutWidth="match_parent"
	layoutHeight="70"
	ratio="1"
	scaleType="center_crop"
	src="${data[0]}"/>
  <TMNImage
	layoutWidth="match_parent"
	layoutHeight="70"
	ratio="1"
	scaleType="center_crop"
	src="${data[1]}"/>
  <TMNImage
	layoutWidth="match_parent"
	layoutHeight="70"
	ratio="1"
	scaleType="center_crop"
	src="${data[2]}"/>
  <TMNImage
	layoutWidth="match_parent"
	layoutHeight="70"
	ratio="1"
	scaleType="center_crop"
	src="${data[3]}"/>
  <TMNImage
	layoutWidth="match_parent"
	layoutHeight="70"
	ratio="1"
	scaleType="center_crop"
	src="${data[4]}"/>
  <TMNImage
	layoutWidth="match_parent"
	layoutHeight="70"
	ratio="1"
	scaleType="center_crop"
	src="${data[5]}"/>
  </NGridLayout>
```  

### 图示

![](https://gw.alicdn.com/tfs/TB1aMTpfiqAXuNjy1XdXXaYcVXa-270-480.png)