---
title: "使用布局"
permalink: /docs/android/use-card
excerpt: "使用布局"
modified: 2016-11-03T10:01:43-04:00
redirect_from:
  - /theme-setup/
---

对于布局来说，内置布局一般满足大部分场景，如果有特殊需求，可自定义实现。需要实现`Card`和`LayoutHelper`。`Card`是自定义布局的 model，`LayoutHelper`是自定义布局的实现，基于[`vlayout`](https://github.com/alibaba/vlayout)框架。

`Card`主要需要实现以下几个方法：

+ `protected void parseStyle(@Nullable JSONObject data)`，解析卡片样式属性。

+ `public LayoutHelper convertLayoutHelper(@Nullable LayoutHelper helper)`，创建该布局对应实现`LayoutHelper`。
åå
`LayoutHelper`主要实现以下几个方法：

+ `public void layoutViews(RecyclerView.Recycler recycler, RecyclerView.State state, LayoutStateWrapper layoutState, LayoutChunkResult result, LayoutManagerHelper helper)`，实现真正的布局逻辑，每次布局至少完成一整行view的布局。
+ `public int computeAlignOffset(int offset, boolean isLayoutEnd, boolean useAnchor, LayoutManagerHelper helper)`，计算该`LayoutHelper`的与相邻`LayoutHelper`之间的间距，这个间距通常就是margin 与 padding 之和。
+ `public void beforeLayout(RecyclerView.Recycler recycler, RecyclerView.State state, LayoutManagerHelper helper)`，可选实现，在布局之前调用。
+ `public void afterLayout(RecyclerView.Recycler recycler, RecyclerView.State state, int startPosition, int endPosition, int scrolled, LayoutManagerHelper helper)`，可选实现，在布局之后调用。

另外实现`LayoutHelper`的时候不需要直接集成它，有以下几种选择：

+ ```BaseLayoutHelper```：像```LinearLayoutHelper```、```GridLayoutHelper```等，内部View可以按行回收的布局，可直接继承此类。
+ ```AbstractFullFillLayoutHelper```：有些布局内部的View 并不是从上至下排列的顺序，即 Adatper 里的数据顺序和物理视图顺序不一致，那么可能就不能按数据顺序布局和回收，需要一次性布局、一次性回收。
+ ```FixAreaLayoutHelper```：fix 类型的布局，子节点不随页面滚动而滚动。可参考```FixLayoutHelper```。