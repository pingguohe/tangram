---
title: "使用自定义服务"
permalink: /docs/android/use-service-manager
excerpt: "使用自定义服务"
modified: 2016-11-03T10:01:43-04:00
redirect_from:
  - /theme-setup/
---

Tangram 内部虽然内置了几个扩展模块的实现或者接口，然而在业务开发过程中必然会涉及到使用业务方自己的功能模块。为了让业务组件能方便地访问所需要的功能模块，解耦框架与业务开发，这些功能模块都可以视为一种自定义服务，注册到 Tangram 框架内部。

## 注册方式

注册的时候调用```TangramEngine```的```public <S> void register(@NonNull Class<S> type, @NonNull S service)```方法。

## 使用方式

在使用的时候可以从组件（布局也有）model 里拿到service 对象，调用```serviceManager.getService(XXX.class)```方法。

## 注意事项

如果注入的服务会存在潜在的持有系统资源等情况，应当在页面退出时主动销毁，防止内存泄漏。