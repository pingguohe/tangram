---
title: "TangramHelper的使用"
permalink: /docs/ios/tangramhelper_usage
excerpt: "TangramHelper的使用"
modified: 2016-11-03T10:01:43-04:00
sidebar:
  title: "iOS 使用指南"
  nav: ios-docs
---

## 概述

Helper具体是指`TangramDefaultDataSourceHelper`, 这个解析器具备以下功能：

- 快速解析layout(NSDictionary -> layout实例)
- 快速解析Model(NSDictionary -> model实例)
- 从model快速生成element(model实例 -> element实例)
- 用新的model刷新即将被复用的element

这样子TangramDatasource delegate 里面的代码就可以大幅度减少，几行代码就搞定。

TangramDefaultDataSourceHelper实际上是串联起来了三个工厂，默认的是

- TangramDefaultLayoutFactory(实现了TangramLayoutFactoryProtocol)
- TangramDefaultItemModelFactory(实现了TangramItemModelFactoryProtocol)
- TangramDefaultElementFactory(实现了TangramElementFactoryProtocol)

只要实现了对应的protocol，`TangramDefaultDataSourceHelper`是可以替换掉默认的factory的

## 快速解析layout+快速解析model

##### `+(NSArray<UIView<TangramLayoutProtocol> *> *)layoutsWithArray: (NSArray<NSDictionary *> *)dictArray tangramBus: (TangramBus *)tangramBus;`

传入里面是字典的数据和tangrambus，生成layout实例。传入tangrambus之后会让生成的layout都绑定tangrambus.

这个方法里面直接也把model给解析了，同时做了快速解析Model,赋给了layout的itemModels。

在tangram datasource需要返回itemModel的时候，直接把layout的itemModel返回即可，不需要调用其他API

## 从model快速生成element

##### `+(UIView *)elementByModel:(NSObject<TangramItemModelProtocol> *)model layout:(UIView<TangramLayoutProtocol> *)layout tangramBus:(TangramBus *)tangramBus;`

根据Model，快速生成element，并且绑定layout和tangrambus

这里生成的element要根据在factory之前注册组件类型的来生成

## 快速复用element

##### `+(UIView *)refreshElement:(UIView *)element byModel:(NSObject<TangramItemModelProtocol> *)modellayout:(UIView<TangramLayoutProtocol> *)layout tangramBus:(TangramBus *)tangramBus;`

根据Model和可以被复用的element，快速刷新element，并且绑定layout和tangrambus

## 替换工厂

TangramDefaultDataSourceHelper 里面有三个工厂注册方法，包括 
 
 - registLayoutFactoryClassName
 - registItemModelFactoryClassName
 - registElementFactoryClassName

 调用即可替换默认的工厂










