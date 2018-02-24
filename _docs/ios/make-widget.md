---
title: "创建自定义widget"
permalink: /docs/ios/make-widget
excerpt: "创建自定义widget"
modified: 2018-02-24T10:01:43-04:00
sidebar:
  title: "iOS 使用指南"
  nav: ios-docs
---

Demo 工程里有新增自定义 widget 的完整示例，可以参照这个 commit：[c7c6909](https://github.com/alibaba/VirtualView-iOS/commit/c7c6909fa163971310436d07f77e43674cc0f986)

所谓 widget，就是指在 XML 模板中可以直接使用的原子组件，类似 NText 或 NImage 之类。

## 1. 修改编译工具新增类型和属性

首先要在编译工具里新增新 widget 对应的 ID，具体可以参考[编译工具文档](http://tangram.pingguohe.net/docs/virtualview/tools)里的 `config.properties` 段落。新增的 widget 如果有新属性的话，也是在编译工具的配置里添加。

以 demo 工程里新增一个 Dot9Image 类型为例，我们需要在 `config.properties` 里增加以下内容：

```
VIEW_ID_Dot9Image=1001
dot9Left=Number
dot9Right=Number
dot9Top=Number
dot9Bottom=Number
dot9Scale=Int
```

我们建议新增的自定义 widget 从一个较大的数字开始，以免和我们以后新增的内部 widget 冲突，所以这里定义了一个新的 widget ID 为 `1001`，然后在 XML 中对应的类型为 `Dot9Image`。

然后 .9 图需要四周边界的属性，所以我们新增了四个对应的属性。因为网络下载的图片 URL 是不可控制的，所以我们无法知道它是 2x 图还是 3x 图之类，所以为网络图片额外增加了 scale 属性。

## 2. 在 Xcode 工程里注册对应的类型和属性

使用以下方法注册新的原子组件类型：

```
[VVNodeClassMapper registerClassName:@"Dot9ImageView" forID:1001];
```

这里的 `Dot9ImageView` 是对应 class 的名称。

然后可以用以下方法注册对应属性名的字串到 VV 里：

```
[VVBinaryStringMapper registerString:@"dot9Left"];
[VVBinaryStringMapper registerString:@"dot9Top"];
[VVBinaryStringMapper registerString:@"dot9Right"];
[VVBinaryStringMapper registerString:@"dot9Bottom"];
[VVBinaryStringMapper registerString:@"dot9Scale"];
```

注意目前来说这一步是可选的，不过以后这一步会是必选的一步，而且目前来说也会对调试带来一定的便利，所以建议进行对应的注册好一些。

这些注册方法要在使用 VVTemplateManager 加载模板之前进行，请自行安排到合适的地方去调用。

## 3. 完成对应的组件代码

这里我们暂且不讨论复杂的自定义布局，只讨论自定义 widget。

首先需要继承一个现存的 widget 或者最基础的 VVBaseNode。

### 3-1. 属性的设置

```
- (BOOL)setIntValue:(int)value forKey:(int)key;
- (BOOL)setFloatValue:(float)value forKey:(int)key;
- (BOOL)setStringValue:(NSString *)value forKey:(int)key;
- (BOOL)setStringData:(NSString *)data forKey:(int)key;
- (BOOL)setDataObj:(NSObject *)obj forKey:(int)key;
```

我们提供了这么一套方法来进行属性的设置，靠上面的三个方法比较直观，是从 XML 模板读取固定数据时用到的，分别对应 int / float / string 类型的数据。这里注意一下枚举和布尔值也是使用 int 类型的。

记得使用这些方法的时候要调用 super 类对应的方法，如果父类没有处理过对应属性，再继续进行处理。另外数据如果可以被处理，请返回 YES 来进行标记。代码的结构大概类似这样：

```
- (BOOL)setIntValue:(int)value forKey:(int)key
{
    BOOL ret = [super setIntValue:value forKey:key];
    if (!ret) {
        if (key == [VVBinaryStringMapper hashOfString:@"dot9Scale"]) {
            self.dot9Scale = value;
            ret = YES;
        }
    }
    return ret;
}
```

未被处理的属性，在 VV_DEBUG 宏开关打开时，会进入断言错误，方便排查问题。

属性建议都是先存到对应的 widget 属性里，比如上面的 `dot9Scale` 就是一个 widget 的单独属性：

```
@property (nonatomic, assign) int dot9Scale;
```

如果 widget 属性需要绑定到一些别的实例上去时，重载对应属性的 set 方法就好。比如 `backgroud` 是 VV 底层就有的一个基础属性，但是我想把它的值绑定到 UIImageView 的 `backgroundColor` 上去，那么：

```
- (void)setBackgroundColor:(UIColor *)backgroundColor
{
    [super setBackgroundColor:backgroundColor];
    self.cocoaView.backgroundColor = backgroundColor;
}
```

如果是你的 widget 新增的属性，那么：

```
- (void)setDot9Scale:(int)dot9Scale
{
    _dot9Scale = dot9Scale;
    // 做对应的绑定
}
```

总体来说差不多一个意思。

### 3-2. 属性支持数据绑定

VV 底层的所有属性大部分都是支持数据绑定的，只有极个别的比如 `orientation` 是无法支持数据绑定的。

上文中的 `setStringData:forKey:` 是为了支持需要转换的数据的。比如颜色值在 .out 文件中会是 int 类型的值，但是数据绑定时肯定是个 string，所以需要在这个方法里进行额外的转换。

其它的 VV 底层自带的属性是通过标记好类型，进行对应类型的 switch case 调用来分别处理，具体代码可以参考 VVPropertyExpressionSetter 类的代码。

如果是用户新增的自定义属性，那么在底层一定是没有办法判断类型的，所以数据类型都会被标记成 TYPE_OBJECT，会使用 `setDataObj:forKey:` 进行属性设值。

所以总结下来就是，新增的自定义属性如果需要支持数据绑定，需要用类似这样的方法来支持：

```
- (BOOL)setDataObj:(NSObject *)obj forKey:(int)key
{
    BOOL ret  = [super setDataObj:obj forKey:key];
    if (!ret) {
        if (key == [VVBinaryStringMapper hashOfString:@"dot9Scale"]) {
            if (obj && [obj isKindOfClass:[NSString class]]) {
                self.dot9Scale = [(NSString *)obj intValue];
                ret = YES;
            }
        }
    }
    return ret;
}
```

那么为什么 `src` 属性就不需要在 setDataObj:forKey: 里写一遍呢？因为它是 VV 底层有的一个属性啊。

### 3-3. Native 元素的添加

实现的自定义组件一般都是用 iOS 的 Native 元素来实现的，你需要加上以下一段代码，来保证 VV 会正确的把你的 Native 元素加到视图树中：

```
- (UIView *)cocoaView
{
	// 这里是为了正确的指定这个 VVNode 对应的根 native 元素，不是必须的
	// 但是如果这里没有重载的话，下面的方法对应的要修改
    return _imageView;
}

- (void)setRootCocoaView:(UIView *)rootCocoaView
{
    if (self.cocoaView.superview != rootCocoaView) {
        if (self.cocoaView.superview) {
            [self.cocoaView removeFromSuperview];
        }
        // 将 native 元素添加到 VV 组件的视图树中
        [rootCocoaView addSubview:self.cocoaView];
    }
    [super setRootCocoaView:rootCocoaView];
}
```

### 3-4. 数据绑定时的回调

有以下两个回调：

```
- (void)reset;       // widget 即将绑定新的数据时触发
- (void)didUpdated;  // widget 绑定完新的数据后触发
```

根据具体需要来进行重载并添加逻辑。

比如 demo 里我们书写的是 image widget，所以在重新绑定数据时一定要把现在的图片先清空了，防止异步加载新图片时老的图片还残留着在：

```
- (void)reset
{
    self.imageView.image = nil;
    [self.imageView sd_setImageWithURL:nil];
    self.needReload = YES;
}
```

### 3-5. 布局事件的重载

主要的布局事件有这么几个：

```
- (void)updateHidden NS_REQUIRES_SUPER;
- (void)updateFrame NS_REQUIRES_SUPER;
- (void)layoutSubNodes;
- (CGSize)calculateSize:(CGSize)maxSize;
```

其中 `layoutSubNodes` 是进行布局的代码，`calculateSize:` 是计算组件尺寸的代码。

`layoutSubNodes` 是自定义布局才需要重载的东西，这里不做讨论。

`calculateSize:` 仅在你的组件需要支持 WARP_CONTENT 模式式才需要重载，大部分情况下不需要自己重载添加逻辑，如果需要的话具体可以参考 NVTextView 内部实现。

`updateHidden` 是 VV 底层递归 hidden 属性（父 node 隐藏时需要隐藏其子 nodes）用到的，一般不需要重载。

而 `updateFrame` 应该是重载概率最高的了，因为在 `updateFrame` 执行完之后，widget 的尺寸才是最终的正确尺寸，图片加载之类的操作应该重载它，在 super 调用结束后进行：

```
- (void)updateFrame
{
    [super updateFrame];
    if (self.needReload) {
        if (self.src && self.src.length > 0) {
            if ([self.src containsString:@"//"]) {
                [self.imageView sd_setImageWithURL:[NSURL URLWithString:self.src]];
            } else {
                self.imageView.image = [UIImage imageNamed:self.src];
            }
        }
        self.needReload = NO;
    }
}
```

## 4. 结语

以上书写自定义 widget 常用的方法都已经介绍完毕。更多的不常用方法，后续会更新更多的技术文档来讲述细节。