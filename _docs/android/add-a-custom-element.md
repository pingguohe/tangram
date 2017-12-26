---
title: "如何编写一个自定义基础控件"
permalink: /docs/android/add-a-custom-element
excerpt: "如何编写一个自定义基础控件"
modified: 2017-12-08T10:01:43-04:00
sidebar:
  title: "Android 使用指南"
  nav: android-docs
---

# Android

了解 VirtualView 里组件的编译、解析流程，有助于开发自定义基础控件，开发自定义基础控件分成两部分：解析器与控件实现，解析器是注册到编译工具里使用的，这一部分将来是可以优化省略掉，但目前还得手动写一个。控件实现需要按照规定的规范实现接口。下面是两个部分的步骤介绍，结合步骤和 Demo 里的源码可以更好的上手。

## 控件定义与编写解析器部分

![](https://gw.alicdn.com/tfs/TB1YYgleOqAXuNjy1XdXXaYcVXa-775-1125.jpg)

在处理组件属性的时候，需要将每一个属性都编译成整型，或者整型的资源索引。具体来讲在 `convertAttribute` 方法会有以下几个注意的地方：

1. 先调用父类的 `convertAttribute` 方法，让父类转换基础属性，父类能转换的会返回 `CONVERT_RESULT_OK`，这样自己就不用处理了。
2. 如果父类返回 `CONVERT_RESULT_FAILED` ，说明父类不认识当前属性，需要自定义控件自己转换，通过判断属性 id 是否是自定义属性，在自己的 `convertAttribute` 方法里一一转换，如果转换成功，也要返回 `CONVERT_RESULT_OK`，如果转换出错，返回 `CONVERT_RESULT_ERROR`，如果不认识，则直接返回 `CONVERT_RESULT_FAIL`，这样它的子类还能进一步处理，如果没有子类，编译工具会调用当前布局容器的方法来转换这些属性，对于 layout 开头的属性，都是在交给父节点的布局容器来转换的。
3. 从 XML 里读取的原始值都存储在 `AttrItem` 的 `mStrValue` 里，如果它应该是个尺寸数值，可以直接调用基类的 `parseNumber` 方法转换，更具体地也可以调用 `parseFloat` 或者 `parseInt` 方法转换；如果是颜色值、布尔值，常用的枚举值，基类里也有对应的方法可用。如果是自定义的新属性或者枚举类型，自行转换即可，转换结果分别按照类型调用 `AttrItem` 里的 `setIntValue`、`setFloatValue`、`setStr`等方法存储结果。

当然了，这样需要注意很多细节，对开发者不是很友好，努力优化中，让开发者不用写代码也能支持解析逻辑。


## 实现控件逻辑
![](https://gw.alicdn.com/tfs/TB1plm8hfDH8KJjy1XcXXcpdXXa-1130-1180.jpg)

也需要有几个地方说明：

1. setAttribute 系列方法对应了好几个重载方法，分别用来接收 int、float、string、object 类型的数据，需要根据自己的设计在不同的方法里接收不同属性的数据。
2. 同编译过程一样，也要先调用父类的 setAttribute 方法，父类能接收，会返回 true，自定义控件就不需要额外处理，否则返回 false，自定义控件里根据属性 id 来做进一步处理。自定义控件能处理也要返回 true，否则返回 false，这样它的子类能按照这个协议做进一步的处理。
3. string 类型的 setAttribute 方法，需要判断是否是数据表达式类型的数据，通过 `Utils.isEL`。如果是数据绑定表达式，先存储到 `ViewCache` 对象里，并指定类型，后续传入数据的时候，会根据类型来从原始数据里解析出真实值给属性，并调用对应类型的 setAttribute 方法。如果不是数据绑定表达式，则直接复制给属性。举个例子：

```
if (Utils.isEL(stringValue)) {
    mViewCache.put(this, StringBase.STR_ID_name, stringValue, Item.TYPE_STRING);
} else {
    mName = stringValue;
}
```