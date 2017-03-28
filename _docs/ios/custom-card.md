---
title: "自定义布局"
permalink: /docs/ios/custom-card
excerpt: "自定义布局"
modified: 2016-11-03T10:01:43-04:00
sidebar:
  title: "iOS 使用指南"
  nav: ios-docs
---

对于布局来说，需要实现`TangramLayoutProtocol`，包含以下几个方法

## 必选方法

##### `- (TangramLayoutType *)layoutType;`

指定layoutType，不重复即可

##### `- (void)setItemModels:(NSArray *)itemModels;`

设置itemModels，这个方法会被TangramView调用

##### `- (NSArray *)itemModels;`

获取itemModels，取itemModels供计算

##### `- (void)calculateLayout;`

计算布局，把内部每一个itemModel的frame计算出来

同时，根据itemModel的变更，同时变更自己的Frame，主要是变更高度。注意itemModel所提供的margin属性在布局中起到的作用。

## 可选方法

##### `- (CGFloat)marginTop;- (CGFloat)marginRight;- (CGFloat)marginBottom;- (CGFloat)marginLeft;`

边距，这个边距是会向外扩展的，与Model的margin不同

##### `- (NSString *)position;`

用来标记在tangramView 会被特殊处理的layout。比如吸顶，固定，浮动等。

|position|类型|
|----|----|
|top-fixed|顶部固定|
|bottom-fixed|底部固定|
|sticky|吸顶|
|float|浮动可拖|
