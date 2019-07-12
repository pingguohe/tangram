---
title: "NImage"
permalink: /docs/virtualview/nimage
excerpt: "NText"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<NImage>`

原生图片组件，支持加载本地图片或者网络图片，支持所有的缩放模式。

> 业务方需要提供一个 IImageLoaderAdapter 才能加载图片。

### 子组件
原子组件

### 属性

|名称|类型|默认值|描述|支持表达式|
|---|---|---|---|---|
|src|string|无|图片资源，本地资源名或远程图片地址|是|
|scaleType|enum(fit_start/fit_xy/matrix/center/center_crop/center_inside/fit_center/fit_end)|fit_xy|缩放模式(iOS仅支持部分模式，详见代码)|否|

### 事件

支持click、longclick、exposure。

### 代码示例

```
<NImage
    layoutWidth="match_parent"
    layoutHeight="181"
    scaleType="fit_xy"
    src="tk_shadow_bg"
    />
```

### 图示

![](https://gw.alicdn.com/tfs/TB1kf_ofiqAXuNjy1XdXXaYcVXa-270-480.png)
