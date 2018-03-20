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

### 从 XML 到运行时实例

举个简单的例子，在 XML 模板里，可能会有这么一块控件的使用：

```
<NText
    id="1"
    text="title"
    textSize="12"
    textColor="#333333"
    layoutWidth="wrap_content"
    layoutHeight="wrap_content"
    lineSpaceMultiplier="1.1"
    lines="2"
    flag="flag_event|flag_exposure|flag_clickable"
 />
```

这在 VirtualView 表示引用一个文本控件，在 VirtualView 方案里会将 XML 里的字符串等编译成整型数值或者索引来降低解析成本。因此从在 XML 里使用一个控件到运行时渲染它，就要经过一系列的转换过程，其中有一半的过程是事先离线执行的，另一半的过程才是在客户端里运行时执行。以下这张图概括了整个流程：

![](https://user-gold-cdn.xitu.io/2018/3/20/16242d5e39f24266?w=1024&h=768&f=jpeg&s=27308)

说明一下每个步骤：

+ 先编写 XML 文件，如图所示引用了一个 NText 控件；
+ 在 Config 文件里配置 NText 的标签名及属性的映射关系；像这种内置控件都已经配置好了，如果是自定义属性和控件，才要操作自己添加配置；配置文件含义参考这篇文章：[VirtualView 工具大更新啦](http://pingguohe.net/2018/01/23/virtualview-tool.html)。在这个示例中，NText 标签被编译工具编译成数字 7，属性名 id、text、textSize、textColor 都被编译成一个 hashcode 索引，真正的字符串值会存储到字符串资源区；属性值 title 也是被编译成一个 hashcode 索引，真正的字符串值会存储到字符串资源区；属性值 12 被直接编译成数字 12； 属性值 #333333 被编译成颜色值 -13421773；
+ 编译工具根据 XML 文件和 Config 文件编译出一份二进制文件，交给客户端使用；
+ 客户端初始化框架的时候会根据 id 注册控件，在这个示例中 7 代表了 NativeText 类控件，它就用来实例化 XML 里的 NText 标签；
+ 最后将 XML 里 NText 下的属性传给 NativeText 实例进一步用于渲染；

### 创建控件实例的过程

以创建一个 PicassoImage 为例（虽然内置了 VImage 和 NImage 两个控件，但在实际业务场景中，还是使用一个自定义的图片控件比较合适，这样可以更好利用起结合图片库的内存管理、性能优化等 feature）。

#### 目标

+ 实现一个原生 Image 控件，使用 Picasso 加载图片
+ 支持绑定 url 属性用来加载网络图片
+ 支持绑定 degree 属性用来旋转图片

#### 1. 定义标签名及其 id，属性名及类型

在编译工具里配置文件里定义：

+ `VIEW_ID_PicassoImage=1014`，其中 `PicassoImage` 就是 XML 里的标签名，id 值为 1014，这个是自定义的，**建议从 1001开始，前 1000 保留给系统使用**；
+ `degree=Float`，表示属性名是 degree ，属性值按 Float 类型编译解析；
+ `url=String`，表示属性名是 url，属性值按 String 类型编译，不过未在配置文件里声明的属性都是按 String 类型编译的，所以可以**省略**；

#### 2. 定义控件的载体 View

取名 `PicassoImageView`，继承 `ImageView`，实现 `IView` 接口，因为 demo 比较简单，除此之外不做其他逻辑，主要实现 `IView` 的接口调用对应的系统 measure、layout 方法，因为这些方法是不能在外部调用的，只能通过 `IView` 的接口封装一下暴露出去。

详细代码：[PicassoImageView.java](https://github.com/alibaba/Virtualview-Android/blob/master/app/src/main/java/com/tmall/wireless/virtualviewdemo/custom/PicassoImageView.java)

#### 3. 定义控件 model 

取名 `PicassoImage`，继承 `ViewBase`，在构造函数里实例化 `PicassoImageView`，并获取自定义属性的 id；

```
public PicassoImage(VafContext context,
        ViewCache viewCache) {
        super(context, viewCache);
        mPicassoImageView = new PicassoImageView(context.getContext());
        StringSupport mStringSupport = context.getStringLoader();
        // 这里会取加载的模板数据里取获取对应的 id，第一个参数是属性名，第二个参数应当为 false；
        urlId = mStringSupport.getStringId("url", false);
        degreeId = mStringSupport.getStringId("degree", false);
    }
```

由于 `ViewBase` 本身也是实现 `IView` 接口的，所以复写几个 `IView` 的 measure、layout 接口，去调用对应的 `PicassoImageView` 里的接口。在 VirtualView 体系内部，都是通过 `ViewBase` 对象来驱动布局计算的，因此必须通过 `IView` 接口调用系统 `View` 真正的计算接口。

```
@Override
public void onComMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    mPicassoImageView.onComMeasure(widthMeasureSpec, heightMeasureSpec);
}

@Override
public void onComLayout(boolean changed, int l, int t, int r, int b) {
    mPicassoImageView.onComLayout(changed, l, t, r, b);
}

@Override
public void comLayout(int l, int t, int r, int b) {
    super.comLayout(l, t, r, b);
    //这一步很关键，否则 view 不显示。
    mPicassoImageView.comLayout(l, t, r, b);
}
```

剩下的主要逻辑是处理自定义属性，有几个 `setAttribute`，`setRPAttribute` 重载的方法，它们用于接收不同类型的属性值：

+ `boolean setAttribute(int key, int value)` 处理编译成整数类型的属性；
+ `boolean setAttribute(int key, float value)` 处理编译成浮点数类型的属性；
+ `boolean setAttribute(int key, String stringValue)` 处理编译成字符串类型的属性，包括那些本该编译成整数或者浮点数但因为写了表达式被编译成字符串类型的；
+ `boolean setRPAttribute(int key, int value)` 处理编译成整数类型的尺寸属性，单位是 rp([介绍在此](http://tangram.pingguohe.net/docs/virtualview/elements))；
+ `boolean setRPAttribute(int key, float value)` 处理编译成浮点数类型的尺寸属性，单位是 rp；

基础 `ViewBase` 里解析处理了大量基础属性，所以自定义控件只要处理新增的自定义属性就行了。以上这些重载方法都有一个 Boolean 返回值，它遵循冒泡逻辑，当你返回 true 的时候，当前层级处理了这个属性，否则表示当前层级处理不了这个属性，需要进一步交给子类解析；在本文的示例里，是这么处理的：

```
@Override
protected boolean setAttribute(int key, float value) {
    boolean ret = true;
    if (key == degreeId) {
    	//从模板里直接获取到旋转角度属性值
        degrees = value;
    } else {
        ret = super.setAttribute(key, value);
    }
    return ret;
}

@Override
protected boolean setAttribute(int key, String stringValue) {
    boolean ret = true;
    if (key == degreeId) {
    	//从模板里直接获取到旋转角度属性值是一个表达式，暂存到 viewCache 里，等传入数据的时候再次解析，然后回调到上述 setAttribute(int key, float value) 方法里获取最终值
        if (Utils.isEL(stringValue)) {
            mViewCache.put(this, degreeId, stringValue, Item.TYPE_FLOAT);
        }
    } else if (key == urlId) {
	    //从模板里直接获取到url属性值可能是一个表达式，也可能是个直接的 url，如果是表达式，暂存到 viewCache 里，等传入数据的时候再次解析，然后回调本方法里获取最终值
        if (Utils.isEL(stringValue)) {
            mViewCache.put(this, urlId, stringValue, Item.TYPE_STRING);
        } else {
            url = stringValue;
        }
    } else {
        ret = super.setAttribute(key, stringValue);
    }
    return ret;
}
```

最后就是使用这些属性值，在 `onParseValueFinised()` 里一次性应用属性：

```
@Override
public void onParseValueFinished() {
    super.onParseValueFinished();
    Picasso.with(mContext.getContext()).load(url).rotate(degrees).into(mPicassoImageView);
}
```

详细代码：[PicassoImage.java](https://github.com/alibaba/Virtualview-Android/blob/master/app/src/main/java/com/tmall/wireless/virtualviewdemo/custom/PicassoImage.java)

#### 4. 注册控件

通过 `ViewManager` 里的 `ViewFactory` 注册，如下：

```
sViewManager.getViewFactory().registerBuilder(1014,new PicassoImage.Builder());
```

#### 5. 使用与运行效果

XML 里这么写：

```
<VHLayout
    flag="flag_exposure|flag_clickable"
    orientation="V"
    layoutWidth="match_parent"
    layoutHeight="match_parent"
>
<VText
        text="Title: Loading Image with Picasso"
        textSize="12"
        textColor="#333333"
        background="#008899"
        layoutWidth="match_parent"
        layoutHeight="20" />

<PicassoImage
        url="${url}"
        degree="90"
        layoutWidth="match_parent"
        layoutHeight="300" />
</VHLayout>
```

绑定的数据：

```
{
  "url": "https://user-gold-cdn.xitu.io/2018/3/20/16242d5e3adadb1f?w=200&h=200&f=png&s=16211"
}
```

运行的结果：

![](https://user-gold-cdn.xitu.io/2018/3/20/16242d5e39fdd018?w=270&h=480&f=png&s=19859)

图片原图是这样的：

![](https://user-gold-cdn.xitu.io/2018/3/20/16242d5e3adadb1f?w=200&h=200&f=png&s=16211)

可以看到，通过添加自定义的 degree 属性，并调用 Picasso 的 ratate 方法，最终加载了图片，也旋转了图片，可以根据此思路继续为 `PicassImage` 添加更多 Picasso 支持的属性。

本文里用到的例子也上传到了 demo 里，从上午的源码链接里可以获取到完整的 demo。