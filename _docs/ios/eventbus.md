---
title: "事件总线"
permalink: /docs/ios/eventbus
excerpt: "事件总线"
modified: 2016-11-03T10:01:43-04:00
sidebar:
  title: "iOS 使用指南"
  nav: ios-docs
---


事件总线(TangramBus)用于组件和Controller，layout之间的通信。

通常，事件总线(TangramBus)的持有者是Controller中TangramBus
，在组件，布局中的TangramBus都声明成weak即可。

## Controller的声明

在Controller中需要声明一个strong的TangramBus

````objc
//时间总线
@property (nonatomic, strong) TangramBus  *tangramBus;
````

## 注册事件

在初始化TangramView的时候需要注册事件

注册事件有两种，一种是关注发起者，一种是不关注发起者

关注发起者，是指需要关心是那一个组件发起的事件，可以根据registAction时候的identifier来区分

不关注发起者，任何组件抛出的同一种topic事件都可以接收，比如点击

关注发起者的注册方法：

     - (void)registerAction:(NSString *)action ofExecuter:(id)executer onEventTopic:(NSString *)topic fromPosterIdentifier:(NSString *)identifier

不关注发起者的注册方法：

    - (void)registerAction:(NSString *)action ofExecuter:(id)executer onEventTopic:(NSString *)topic

|参数|类型|说明|
|----|-----|----|
|action|NSString|实际上是通过NSSelectorFromString去找executer对应的方法，如果传的是nil,会执行executeWithContext(TangramActionProcol中定义的方法)|
|executer|id|执行者，会对executer调用performSelector|
|eventTopic|NSString|关注主题，关注何种类型的Topic事件|
|posterIdentifier|NSString|发起者id，关注某种发起者，写nil的意思是不关注发起者|

````objc
//事件总线注册
- (void)registEventInEventBus
{
     [self.tangramBus registerAction:@"countImageTimeEnd:" ofExecuter:self onEventTopic:@"countImgTimeEnd" fromPosterIdentifier:@"countImg"];
     [self.tangramBus registerAction:@"countImageClick:" ofExecuter:self onEventTopic:@"countImgTimeClick" fromPosterIdentifier:@"countImg"];
}
````

## 绑定总线

生成View的时候，使用


组件内写一个weak的对TangramBus的引用，实现``setTangramBus``这个方法
在使用下面这两个方法去生成View，在Helper内会自动绑定参数中的tangramBBus。

````
+(UIView *)refreshElement:(UIView *)element byModel:(NSObject<TangramItemModelProtocol> *)model
                   layout:(UIView<TangramLayoutProtocol> *)layout
               tangramBus:(TangramBus *)tangramBus;
````

````
+(UIView *)elementByModel:(NSObject<TangramItemModelProtocol> *)model
                   layout:(UIView<TangramLayoutProtocol> *)layout
               tangramBus:(TangramBus *)tangramBus;

````

## 发起事件

事件发起需要在任何地方去post事件

下面是在组件中抛出事件的样例，组件已绑定了tangramView

````objc
  TangramEvent *dismissEvent = [[TangramEvent alloc]initWithTopic:@"countImgTimeEnd" withTangramView:(TangramView *)self.atLayout.superview posterIdentifier:@"countImg" andPoster:self];
        [self.tangramBus postEvent:dismissEvent];

````

## 响应事件

响应时间的方法会带上一个TangramContext的参数，可以拿到poster，TangramView，和TangramEvent，TangramEvent里面有相关的参数


````objc
- (void)countImageTimeEnd:(TangramContext *)context
{
    [self dismissCountImg:context.poster];
}

- (void)countImageClick:(TangramContext *)context
{
    [self dismissCountImg:context.poster];
}
````