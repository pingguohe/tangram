---
title: "吸顶布局"
permalink: /docs/layout-support/sticky-layout
excerpt: "吸顶布局"
modified: 2016-11-03T10:01:43-04:00
redirect_from:
  - /theme-setup/
---

碰到Tangram的顶端或底端就吸住

示意图

<img src="https://gw.alicdn.com/tfs/TB1ABVdQXXXXXahapXXXXXXXXXX-720-1280.gif" width = "360" height = "640"/>

| 字段 | 意义 | 类型 | 样例 |
| --- | --- | --- | --- |
| margin | 卡片的外间距，顺序是上右下左 | Array\(内部是Number或者String\)\/String | \[9,9,9,9\],"\[9,9,9,9\]",\["9","9","9","9"\]均可，建议使用第一种 |
|offset| 吸住的时候距离顶部或底部的偏移量，单位是系统单位| String/Number | "9"|
|enableScroll|是否平铺&可滚动| Bool | true,"false"|