---
title: "固定布局"
permalink: /docs/layout-support/fix-layout
excerpt: "固定布局"
modified: 2016-11-03T10:01:43-04:00
sidebar:
  title: "布局详细说明"
  nav: docs
---

## 示意图

<img src="https://gw.alicdn.com/tfs/TB11G8GQXXXXXc9XpXXXXXXXXXX-720-1280.gif" width = "360" height = "640"/>

| 字段 | 意义 | 类型 | 样例 |
| --- | --- | --- | --- |
| align | 相对初始位置来说，初始出现位置的原点， 8(固定底部)默认是bottom_left 9(固定顶部)默认是top_left | String | top_left/top_right/bottom_left/bottom_right |
| x | 相对原点的横向偏移量| String/Number| 9|
| y | 相对原点的纵向偏移量| String/Number| 9|
| bgColor | 背景色 | String | "#FFFFFF"|
|  showType|  前一个显示时显示/前一个消失时显示/永远显示(默认永远显示) | String |  "showOnEnter","showOnLeave","always"|


