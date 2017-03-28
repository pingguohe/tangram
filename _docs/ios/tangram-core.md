---
title: "TangramView核心方法"
permalink: /docs/ios/tangram-core
excerpt: "TangramView核心方法"
modified: 2016-11-03T10:01:43-04:00
sidebar:
  title: "iOS 使用指南"
  nav: ios-docs
---

## Tangram 公共方法

TangramView 继承自 TMMuiLazyScrollView ， TMMuiLazyScrollView的方法都可以调用

### 主要公共方法：

##### `- (void)setDataSource:(id<TangramViewDatasource>)dataSource;`

设置delegate

##### `- (void)reloadData;`

刷新，数据源有改变的时候调用。调用此方法，当前区域内的视图不会被销毁。

##### `- (void)reloadLayout:(UIView<TangramLayoutProtocol> *)layout;`

仅刷新某一个layout，适用于只有某一个layout有变化的情况

##### `- (void)removeLayoutsAndElements:(BOOL)cleanElement;`

重新生成layout，参数如果是YES，则所有element会被销毁再重新生成。

##### `- (nullable UIView *)dequeueReusableItemWithIdentifier:(nonnull NSString *)reuseIdentifier;`

这个方法是继承自TMMuiLazyScrollView的，传入复用id，获取可复用的视图

### 公共属性

##### `@property   (nonatomic, strong, readonly) NSMutableDictionary     *layoutDict;`

Layout的索引。键：layout下标(0,1.2...), 字符串形式；值：layout实例

## Tangram DataSouce Delegate

类似UITableView，Tangram的Delegate提供全部的数据源和View供Tangram处理。使用Tangram必须实现TangramViewDatasource，包括以下5个方法：

##### `- (NSUInteger)numberOfLayoutsInTangramView:(TangramView *)view;`

返回整个视图的卡片总数(layout的数量)

##### `- (UIView<TangramLayoutProtocol> *)layoutInTangramView:(TangramView *)view atIndex:(NSUInteger)index;`

返回对应index的layout实例。

Tangram要求Layout必须是UIView或其子类，且必须实现TangramLayoutProtocol

Layout可对比UITableView中的section

##### `- (NSUInteger)numberOfItemsInTangramView:(TangramView *)view forLayout:(UIView<TangramLayoutProtocol> *)layout;`

返回对应layout的model数量。

##### ` - (NSObject<TangramItemModelProtocol> *)itemModelInTangramView:(TangramView *)view forLayout:(UIView<TangramLayoutProtocol> *)layout atIndex:(NSUInteger)index;`

返回layout中对应index的Model。这里的index是在layout内部的Index。

##### `- (UIView *)itemInTangramView:(TangramView *)view withModel:(NSObject<TangramItemModelProtocol> *)model forLayout:(UIView<TangramLayoutProtocol> *)layout  atIndex:(NSUInteger)index;`

返回layout中index对应的View。组件实体同布局实体一样，需要业务代码生成并返回给TangramView。Tangram要求item必须是UIVIew或其子类，item可对比UITableView中的cell。

可以像这样`[view dequeueReusableItemWithIdentifier:model.reuseIdentifier];`先使用TangramView的方法先查找有没有可以复用的View。





