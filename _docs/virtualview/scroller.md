---
title: "Scroller"
permalink: /docs/virtualview/scroller
excerpt: "Scroller"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `<Scroller>`

页面级别的容器组件，在Android上采用Recycler+StaggeredGridLayoutManager实现。通过数据驱动绑定其他组件。

### 子组件
容器组件

### 属性

| 名称 | 类型 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- |
| orientation | enum(H/V) | V | 滚动方向，H：水平滚动，V：垂直滚动 |
| mode | enum(staggeredGrid/linear) | linear | 容器内组件布局，staggeredGrid：瀑布流，linear：线性布局 |
| onAutoRefresh | expr | 无 | 表达式，页面滚动到尾部时触发的加载更多的逻辑 |
| autoRefreshThreshold | int | 触发加载更多数据的提前量，按组件位置算，比如容器内总共有50个组件，当加载到第50-autoRefreshThreshold 个组件的时候触发加载更多逻辑 |  |
| span | int/float | 无 | 瀑布流模式下组件间距，当orientation = V时，是水平方向的间距；当orientatin = H时，是垂直方向的间距 |
| lineSpace | int | 0 | 暂时未使用 |
| firstSpace | int | 0 | 第一个组件前的间距 |
| lastSpace | int | 0 | 最后一个组件后间距 |
| dataTag | jsonArray | 无 | 容器内组件数据，描述内部子组件的类型与数据 |


### 事件

支持子组件显示时的exposure。

### 代码示例

```
<Scroller
    paddingLeft="10"
    paddingRight="10"
    id="10"
    dataTag="vList"
    orientation="H"
    layoutWidth="match_parent"
    layoutHeight="130" />
```

