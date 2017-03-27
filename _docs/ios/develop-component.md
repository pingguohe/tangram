---
title: "组件开发"
permalink: /docs/ios/develop-component
excerpt: "组件开发"
modified: 2016-11-03T10:01:43-04:00
redirect_from:
  - /theme-setup/
---


## 概述

组件开发仅需创建element(View)，在element(View)中返回高度即可。Tangram提供组件生命周期调用代码的时机，需要element(View)中实现对应的Protocol。

使用TangramDefaultDataSourceHelper的话，JSONObject的字段会用KVC直接映射到View中，需要property的名字和下发JSON中对应字段的名字一致。

TangramDefaultDataSourceHelper会在ViewModel映射到View的过程中做字段保护，当数据类型与实际property类型不符合，会不进行赋值。为做保护，从JSON映射到的property仅支持NSString,NSArray,NSDictionary,NSNumebr,NSData类型。

在Tangram中，json映射到view的流程是： JSONObject --> ViewModel(TangramDefaultItemModel) --> View

## 注册

只有注册了组件，才可以使用TangramDefaultDatasourceHelper 快速实现从ViewModel到View的转化。

类似快速开始中Demo的写法

```objc
 [TangramDefaultItemModelFactory registElementType:@"1" className:@"TangramSingleImageElement"];
 [TangramDefaultItemModelFactory registElementType:@"2" className:@"TangramSimpleTextElement"];
```

## element实现

element是UIView的子类即可。需要实现`TangramEasyElementProtocol`和`TangramElementHeightProtocol`，这两个是必选的，同时推荐实现`TMMuiLazyScrollViewCellProtocol`

![](https://img.alicdn.com/tfs/TB1BWNgQpXXXXaoXVXXXXXXXXXX-739-619.png)

### [必选]TangramEasyElementProtocol

这个Protocol做一些绑定的动作，让element和model，layout，tangrambus做关联。

`TangramEasyElementProtocol`包含以下方法

```objc

@protocol TangramEasyElementProtocol <NSObject>

@required
//Get itemModel
- (TangramDefaultItemModel *)tangramItemModel;
//Set itemModel
- (void)setTangramItemModel: (TangramDefaultItemModel *)tangramItemModel;

@optional

// Bind layout
- (void)setAtLayout: (UIView<TangramLayoutProtocol> *)layout;
- (UIView<TangramLayoutProtocol> *)atLayout;

// Bind TangramBus
- (void)setTangramBus:(TangramBus *)tangramBus;

@end

```

其实这就是两个get&set方法加一个事件总线绑定，所以实际上是需要在element上声明两个变量tangramItemModel,atLayout，内部加一个weak的TangramBus

atLayout是绑定的layout,让组件可以找到layout

这些方法的调用时机是在element被init之后被调用，被调用的时候，element的frame还没有被赋值。

element需要的数据可以通过`itemModel`的`bizValueForkey`获取，拿到的就是服务端原始下发的json(除style)中的数据，style中的数据使用`styleValueForKey`方法获取

TangramBus用于组件之间，组件和Controller之前的事件通信，可以替代繁杂的Delegate，使用方法在后面进行说明

### [必选]实现TangramElementHeightProtocol

 element中需要实现一个静态方法`+ (CGFloat)heightByModel:(TangramDefaultItemModel *)itemModel;`
根据itemModel中的数据，返回对应该View的高度

在这里，可以使用`itemModel`的`bizValueForKey`或者`styleValueForKey`的方法在这里获取参数值，这里的itemModel已经获取了宽度

**不实现这个delegate的View将没有高度!**

### [强烈推荐]element复用/曝光/渲染时机

在MUILazyScrollView中，提供几个AOP方法

element可以实现`TMMuiLazyScrollViewCellProtocol`

```objc

@protocol  TMMuiLazyScrollViewCellProtocol<NSObject>

@optional
//准备复用的时候，做的动作 调用时机是在dequeueReusableItemWithIdentifier返回View之前
//调用dequeueReusableItemWithIdentifier一定会调用到这个方法
- (void)mui_prepareForReuse;
//当View进入视图范围内，调用这个方法，传入enter的次数，曝光埋点打在这里是精确的点
//注意：并非是在视图init的时候，视图init会受缓冲区的影响，生成View的范围比视图实际区域会大一些
//times 次数的意思,从0开始 0的意思是第一次，1第二次，以此类推
- (void)mui_didEnterWithTimes:(NSUInteger)times;
//提供一个渲染View的时机，生成View后执行
//在View从delegate中返回之后执行，推荐把布局视图等方法放在这个方法内
//如果是在Tangram的组件中使用，在这个方法执行的时候会带frame
- (void)mui_afterGetView;

@end
```

在渲染、复用前的准备这些时机需要插入代码时，强烈推荐使用Protocol的这些方法

请注意一定要在刷新的时候调用`TangramView`的`resetViewEnterTimes`方法，清空组件进入屏幕的次数

### 关于复用标记Protocol TangramElementReuseIdentifierProtocol

````objc
@protocol TangramElementReuseIdentifierProtocol <NSObject>

+ (NSString *)reuseIdByModel:(TangramDefaultItemModel *)itemModel;

````
如果没有实现这个protocol，Tangram会把组件的type号作为复用id，同类组件可以复用。

但是有些组件不能遵循这个规则，比如有些组件要求不被复用，有些组件的复用id不是type(比如动态组件)，那么实现这个protocol，返回对应的reuseIdentifier


