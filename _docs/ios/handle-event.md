---
title: "交互事件回调"
permalink: /docs/ios/handle-event
excerpt: "交互事件回调"
modified: 2018-02-24T10:01:43-04:00
sidebar:
  title: "iOS 使用指南"
  nav: ios-docs
---

VirtualView 里目前定义了两种常用的交互事件处理：

- 点击
- 长按

## 为什么没有曝光事件？

因为由组件自身去 hack 它的父容器，判断是 UITableView 还是 UICollectionView 之类，再去截取 contentOffset 事件来判断自身有没有被展示，是很 dirty 而且很不稳定的做法。所以曝光应该是跟业务强相关，由业务层来封装完成的逻辑。

## 1. XML 模板中标记对应 node 为可点击

如果你的 VV 组件是要支持点击的，那么在 XML 模板中，需要将支持点击的元素以及模板根元素都设置成可点击的，具体做法就是添加对应的 flag 属性：

```
flag="flag_clickable|flag_longclickable"
```

## 2. VV 底层处理点击事件

VV 底层在有 flag_clickable 标记时，会添加一个 UITapGestureRecognizer。

在有 flag_longclickable 标记时，会添加一个 UILongPressGestureRecognizer。

两个事件都是在结束触摸时才会触发。

要注意的是长按事件有额外做一个判断保护，就是长按的同时拖拽的距离超过 VV_LONG_PRESS_CANCEL_DISTANCE（默认值是 10dp）后，长按事件将不会被触发。这是为了保障在列表滚动时不会频繁的误触发长按事件。

对应的事件触发时，会按照逻辑尝试调用 VVViewContainerDelegate 里的回调：

```
- (void)virtualViewClickedWithAction:(NSString *)action andValue:(NSString *)value;
- (void)virtualView:(VVBaseNode *)node clickedWithAction:(NSString *)action andValue:(NSString *)value;
- (void)virtualViewLongPressedWithAction:(NSString *)action andValue:(NSString *)value;
- (void)virtualView:(VVBaseNode *)node longPressedWithAction:(NSString *)action andValue:(NSString *)value;
```

比如在点击事件触发时，VV 会优先尝试调用 virtualView:clickedWithAction:andValue: 回调，如果 delegate 没有实现它的话，再去尝试调用 virtualViewClickedWithAction:andValue:。

**很重要**：所以说，点击事件的两个回调永远只会触发里面的一个！按需实现其中的一个即可！

长按事件同理。

## 3. 上层实现回调，处理对应逻辑

具体可以参考开源工程 demo 里的 [ContainerViewController](https://github.com/alibaba/VirtualView-iOS/blob/master/VirtualViewDemo/VirtualViewDemo/ContainerViewController.m)。

## 4. action 和 value 的属性值

action 属性是 VV 组件的一个基础属性，支持直接设置，也支持数据绑定：

```
<FrameLayout 
        flag="flag_exposure|flag_clickable|flag_longclickable"
        action="action1"
        layoutWidth="match_parent"
        layoutHeight="match_parent"
        background="#ffffff" />
```

```
<FrameLayout 
        flag="flag_exposure|flag_clickable|flag_longclickable"
        action="${action}"
        layoutWidth="match_parent"
        layoutHeight="match_parent"
        background="#ffffff" />
```

这个值会在回调中原样传递。通常来说这个值足够用来处理交互逻辑了，也可以用作埋点之类。

通常来说 action 用来标记被点击的位置是哪里，如果点击后跳转的位置是不固定的，或者需要携带一些参数，那么就需要用到 actionValue。

actionValue 是以 action 为 key（注意只是 key，不支持 keyPath），从所绑定的数据里获取的。以上文的第一个 FrameLayout 为例，如果此时的绑定数据为：

```
{
    "action1" : "http://tangram.pingguohe.net/",
    "title" : "test"
}
```

那么这个 FrameLayout 的 action 为 "action1"，actionValue 为 "http://tangram.pingguohe.net/"。这时触发了点击事件的话，这个 actionValue 就会传递到 delegate 回调的 value 值上去，以供读取使用。