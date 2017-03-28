---
title: "瀑布流布局"
permalink: /docs/layout-support/waterflow-layout
excerpt: "瀑布流布局"
modified: 2016-11-03T10:01:43-04:00
redirect_from:
  - /theme-setup/
---

## 示意图

![](https://gw.alicdn.com/tfs/TB1FBXHQpXXXXXPXpXXXXXXXXXX-449-412.png)


| 字段 | 意义 | 类型 | 样例 |
| --- | --- | --- | --- |
| margin | 卡片的外间距，顺序是上右下左 | Array(内部是Number或者String)/String | [9,9,9,9],"[9,9,9,9]",["9","9","9","9"]均可，建议使用第一种 |
| vGap | 垂直方向上，每个组件的间距 | String/Number | "9" |
| hGap | 水平方向上，每个组件的间距 | String/Number | "9" |
| bgColor | 背景色 | String | "#FFFFFF"|
| column | 列数| String/Number| "3"或3 |

