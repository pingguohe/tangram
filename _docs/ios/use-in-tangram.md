---
title: "在Tangram中使用"
permalink: /docs/ios/use-in-tangram
excerpt: "在Tangram中使用"
modified: 2018-02-24T10:01:43-04:00
sidebar:
  title: "iOS 使用指南"
  nav: ios-docs
---

Tangram 中管理 VV 模板用到了一个类叫做 VVTempleteManager，打错了一个字母，也凑巧和 VV 底层的 VVTemplateManager 命名错开了……后面的版本为了避免歧义，会改名为 TMVVTemplateManager。

在这个类里面，使用 `TangramKitVVElementTypeMap.plist` 文件管理了所有需要加载的 VV 模板文件。如果需要新增其它的 VV 模板，可以在这里面新增。

Tangram 在内部实现了一个 TMVVBaseElement，实现了 Tangram 的各种协议并包装好了 VV 组件，所以在 .plist 文件里注册过的 VV 模板就可以用 Tangram 的数据格式描述了。

需要注意的是 TMVVBaseElement 是一个基础通用组件，它不具备计算 VV 组件宽高的能力。所以需要在 .plist 文件里通过配置固定高度 `height` 的值，或者宽高比 `ratio` 的值，使其可以计算出 VV 组件正确的宽高。

如果有复杂的宽高计算逻辑，那么你需要重载 TMVVBaseElement 并自己实现对应的 heightByModel: 方法，在 .plist 文件里把 `element` 设置成你的新 class。

如果没办法更改 .plist 文件之类，也可以使用 Tangram 所支持的 style.height 方式来设置元素的高度。