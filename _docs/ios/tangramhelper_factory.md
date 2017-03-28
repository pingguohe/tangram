---
title: "Tangram各类Factory说明"
permalink: /docs/ios/tangramhelper_factory
excerpt: "Tangram各类Factory说明"
modified: 2016-11-03T10:01:43-04:00
sidebar:
  title: "iOS 使用指南"
  nav: ios-docs
---

TangramDefaultDataSouceHelper串联起来了三个工厂，使得Helper可以大幅度简化代码。

## 实现 TangramLayoutFactoryProtocol 的工厂

实现 TangramLayoutFactoryProtocol 的工厂，提供了把NSDictionary转换成layout实例的功能

默认已经提供了TangramDefaultLayoutFactory,并实现了protocol中所有必选和可选的方法

#### `+ (UIView<TangramLayoutProtocol> *)layoutByDict:(NSDictionary *)dict`

传入一个NSDictionary，解析出来一个layout并返回。TangramDefaultLayoutFactory支持的样式可以参见Demo。

#### `(void)registLayoutType:(NSString *)type className:(NSString *)layoutClassName;`

可选方法，传入一个type和布局的classname，注册layout的type到工厂中

#### `+ (NSString *)layoutClassNameByType:(NSString *)type;`

传入type 返回layout的name

## 实现 TangramItemModelFactoryProtocol 的工厂

实现 TangramItemModelFactoryProtocol的工厂，提供了把NSDictionary转换成itemModel实例的功能

默认提供的是TangramDefaultItemModelFactory

#### `+ (NSObject<TangramItemModelProtocol> *)itemModelByDict:(NSDictionary *)dict;`

传入NSDictionary 生成一个itemModel

#### `+ (void)registElementType:(NSString *)type className:(NSString *)elementClassName;`

传入一个type和组件的classname，注册element的type到工厂中

## 实现 TangramElementFactoryProtocol 的工厂

实现 TangramElementFactoryProtocol的工厂，提供了把Model转换成组件实例的功能

默认已经提供了TangramDefaultElementFactory

#### `+ (UIView *)elementByModel:(NSObject<TangramItemModelProtocol> *)model;`

传入一个Model 生成一个element，TangramDefaultElementFactory 需要传入 TangramDefaultItemMode

#### `+ (UIView *)refreshElement:(UIView *)element byModel:(NSObject<TangramItemModelProtocol> *)model;`

根据itemModel刷新视图


