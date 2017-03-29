---
title: "接入Tangram代码"
permalink: /docs/ios/access-tangram
excerpt: "接入Tangram代码"
modified: 2016-11-03T10:01:43-04:00
sidebar:
  title: "iOS 使用指南"
  nav: ios-docs
---


## 1. 增加Tangram依赖

Tangram要求iOS系统版本在 6.0及以上即可

### 使用Cocoapods

在Podfile里面增加Tangram依赖

````
pod 'Tangram'
````

### 直接使用代码依赖

直接把Tangram中Source和Resource文件夹加入到工程中即可。

Tangram本身也有依赖，需要把依赖加到Podfile中或者自行依赖下面的这些模块

```
 pod 'LazyScroll', ~>'0.0.2'
 pod 'SDWebImage', ~>'3.7'
```

## 2. 注册Tangram组件

类似UITableView注册cell一样，使用Tangram组件也是需要预先注册的。

下面是一个样例，注册了type = 1的组件 是 TangramSingleImageElement 这个类

type = 2的组件是 TangramSimpleTextElement 这个类

```objc
  [TangramDefaultItemModelFactory registElementType:@"1" className:@"TangramSingleImageElement"];
  [TangramDefaultItemModelFactory registElementType:@"2" className:@"TangramSimpleTextElement"];
```

## 3. 读取数据，使用Helper解析成layout实例

```objc
 //获取数据
 NSString *mockDataPath = [NSString stringWithContentsOfFile:[[NSBundle mainBundle] pathForResource:@"TangramMock" ofType:@"json"] encoding:NSUTF8StringEncoding error:nil];
 NSDictionary *dict = [mockDataPath objectFromJSONString];
 self.layoutModelArray = [[dict objectForKey:@"data"] objectForKey:@"cards"];
 //使用helper解析成layout实例
 self.layoutArray = [TangramDefaultDataSourceHelper layoutsWithArray:self.layoutModelArray];
```

## 4. 实现TangramDataSource Delegate

```objc
//返回layout个数
- (NSUInteger)numberOfLayoutsInTangramView:(TangramView *)view
{
    return self.layoutModelArray.count;
}
//返回layout的实例
- (UIView<TangramLayoutProtocol> *)layoutInTangramView:(TangramView *)view atIndex:(NSUInteger)index
{
    return [self.layoutArray objectAtIndex:index];
}
//返回某一个layout中itemModel的个数
- (NSUInteger)numberOfItemsInTangramView:(TangramView *)view forLayout:(UIView<TangramLayoutProtocol> *)layout
{
    return layout.itemModels.count;
}
//返回layout中指定index的itemModel实例
- (NSObject<TangramItemModelProtocol> *)itemModelInTangramView:(TangramView *)view forLayout:(UIView<TangramLayoutProtocol> *)layout atIndex:(NSUInteger)index
{
    return [layout.itemModels objectAtIndex:index];;
}
//根据Model生成View
//以上的方法在调用Tangram的reload方法后就会执行，而这个方法是按需加载
- (UIView *)itemInTangramView:(TangramView *)view withModel:(NSObject<TangramItemModelProtocol> *)model forLayout:(UIView<TangramLayoutProtocol> *)layout atIndex:(NSUInteger)index
{
    //先尝试找可以复用的View，有的话就赋值，没有的话就生成一个
    UIView *reuseableView = [view dequeueReusableItemWithIdentifier:model.reuseIdentifier];
    
    if (reuseableView) {
        reuseableView =  [TangramDefaultDataSourceHelper refreshElement:reuseableView byModel:model];
    }
    else
    {
        reuseableView =  [TangramDefaultDataSourceHelper elementByModel:model];
    }
    return reuseableView;
}
```

## 5. 创建Tangram实例

```objc
-(TangramView *)tangramView
{
    if (nil == _tangramView) {
        _tangramView = [[TangramView alloc]init];
        _tangramView.frame = self.view.bounds;
        //要设置datasouce delegate
        [_tangramView setDataSource:self];
        _tangramView.backgroundColor = [UIColor whiteColor];
        [self.view addSubview:_tangramView];
    }
    return _tangramView;
}
```

## 6. 刷新一下，视图即可展现

```objc
    [self.tangramView reloadData];
```

完整代码可看TangramDemo中的`` MockViewController.m``






