---
title: "事件总线"
permalink: /docs/android/eventbus
excerpt: "事件总线"
modified: 2016-11-03T10:01:43-04:00
redirect_from:
  - /theme-setup/
---

事件总线(TangramBus)用于组件和组件、组件和卡片、组件和页面之间的通信。它也是默认被注册到 Tangram 的 serviceManager 里，在组件和 TangramEngine 里都可以获取到，这里单独介绍它的使用。

## BusSupport

消息总线模块，获取方式：

```
BusSupport busSupport = serviceManager.getService(BusSupport.class);
```

## Event 与 EventPool

```Event``` 是消息对象类型，消息发送者构造这样一个对象通过总线来发送。它有四个成员变量：

+ type：String，消息的类型；不能为空；
+ sourceId：String，消息发送者 Id；可以为空；
+ args：ArrayMap<String, String>，消息参数；可以为空；
+ eventContext：EventContext，上下文，包含发送者对象，TangramEngine；可以为空；

获取一个 ```Event``` 对象：```BusSupport.obtainEvent() ```返回一个空事件对象。也可以用 ```BusSupport.obtainEvent(String type, String sourceId, ArrayMap<String, String> args, EventContext eventContext)```。使用这种方式会从 ```EventPool``` 缓存池里获取对象，消息派发完会回收到消息池里。

## EventHandlerWrapper

消息接收者通过 ```EventHandlerWrapper``` 封装再注册到总线模块里。构造接收者的方式：```BusSupport.wrapEventHandler(@NonNull String type, String producer, @NonNull Object subscriber,
            String action)```；四个参数含义分别如下：

+ type：声明接收的消息类型；不为空；
+ producer：消息发送者 Id，如果不为空，消息总线在派发消息的时候，会检查消息的  sourceId 与接收者声明的 producer 是否匹配，匹配时才会派发给该接收者，否则不派发；
+ subscriber：接收者对象，所属类必须声明一个消息处理方法，方法约束见action 的定义；不为空；
+ action：处理函数方法名；若为空，默认回调 subscriber 的```execute(Event event)``` 方法，否则回调以 action 命名的方法，参数类型是 ```Event```；

## 注册/注销消息接收者

调用 ```BusSupport``` 的方法：

```
register(@NonNull EventHandlerWrapper eventHandler)
```

```
unregister(@NonNull EventHandlerWrapper eventHandler)
```

## 发送消息

调用```BusSupport```的方法：

发送一个消息

```
post(@NonNull Event event)
```

发送一组消息

```
post(@NonNull List<Event> eventList)
```

消息最终会在主线程上被派发到接收者那里。