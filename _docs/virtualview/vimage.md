---
title: "VImage"
permalink: /docs/virtualview/vimage
excerpt: "VImage"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<VImage>`

虚拟化实现的图片组件，支持加载本地图片或者网络图片，支持基本的缩放模式。

> 注意图片组件只在demo中有实际意义，实际工程中应当使用自定义的图片组件注册到框架来，这样可以和应用图片特性结合起来。

### 子组件
原子组件

### 属性

|名称|类型|默认值|描述|
|---|---|---|---|
|src|string|无|图片资源，本地资源名或远程图片地址|
|scaleType|enum(fit_start/fit_xy/matrix)|fit_xy|缩放模式|

### 事件

不支持click、longclick。

### 代码示例

```
<VImage
    layoutWidth="match_parent"
    layoutHeight="181"
    scaleType="fit_xy"
    src="tk_shadow_bg"
    />
```  