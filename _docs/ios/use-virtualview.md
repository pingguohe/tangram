---
title: "使用VirtualView"
permalink: /docs/ios/use-virtualview
excerpt: "使用VirtualView"
modified: 2018-02-23T10:01:43-04:00
sidebar:
  title: "iOS 使用指南"
  nav: ios-docs
---

VirtualView 的新版本都会发布到 CocoaPods 上，近期版本已经较为稳定，可以直接用以下指令引用：

```
pod 'VirtualView'
```

如果你想要配合 Tangram 使用，那么直接依赖 Tangram 就够了：

```
pod 'Tangram'
```

VirtualView 作为 Tangram 2.0 的一部分也会被正确的安装。

如果你想要直接下载源代码并添加到工程中，那么可以访问 [Releases 页面](https://github.com/alibaba/VirtualView-iOS/releases)，把最新版下载下来放到你的工程里。

## 1. 加载 .out 文件读取模板

```
if (![[VVTemplateManager sharedManager].loadedTypes containsObject:@"icon_type"]) {
    NSString *path = [[NSBundle mainBundle] pathForResource:@"icon_file" ofType:@"out"];
    [[VVTemplateManager sharedManager] loadTemplateFile:path forType:@"type_alias"];
}
```

先判断组件是否已经加载过了，没有的话则找到对应文件进行加载。

## 2. 创建模板对应的组件

```
self.viewContainer = [VVViewContainer viewContainerWithTemplateType:@"icon_type"];
[self.view addSubview:self.viewContainer];
```

创建出的组件是一个 UIView，直接像使用正常 UIView 一样把它添加进视图树即可。

## 3. 绑定数据并计算布局（适用于已经确定尺寸的组件）

```
self.viewContainer.frame = CGRectMake(0, 0, SCREEN_WIDTH, 1000);
[self.viewContainer update:@{
    @"type" : @"icon-type",
    @"imgUrl" : @"https://test.com/test.png"
}];
```

先设定好尺寸，然后调用 `update` 方法进行更新，此方法会更新数据也会更新布局。

## 4. 如果你想计算得出组件的尺寸

```
[self.viewContainer updateData:@{
    @"type" : @"icon-type",
    @"imgUrl" : @"https://test.com/test.png"
}];
CGSize size = CGSizeMake(MAX_WIDTH, MAX_HEIGHT);
size = [self.viewContainer estimatedSize:size];
self.viewContainer.frame = CGRectMake(0, 0, size.width, size.height);
[self.viewContainer updateLayout];
```

计算尺寸的话，更新数据和更新布局要分步处理：

1. 首先调用 `updateData` 进行数据的更新
2. 准备好最大约束尺寸，用 `estimatedSize` 方法计算预估的组件大小
3. 设定好组件的正确尺寸，最后用 `updateLayout` 更新组件内部元素布局

更多的详细使用细节，可以在 demo 工程里面找到，包含了各种基础元素的使用示例。demo 工程还里演示了怎么用脚本自动编译 XML 新的修改并同步 .out 文件。