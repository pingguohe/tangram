---
title: "增加自定义逻辑"
permalink: /docs/android/add-custom-logic
excerpt: "增加自定义逻辑"
modified: 2017-12-08T10:01:43-04:00
sidebar:
  title: "Android 使用指南"
  nav: android-docs
---

在开发基础组件的时候，难免碰到一些特殊需求，或是与其他基础组件存在通信、或是与外部存在通信，不能简单的通过定义组件来完成。这里有两种方式可以处理：

## 注册 Service 到 VirtualView 里

注册服务，比如：

```
mVafContext.registerService(Animation.Class, animationBuilder);
```

在自定义组件的 model 里获取服务并调用：

```
mContext.getService(Animation.Class);
```

## 注册 IBean 到 VirtualView 里，并在模板里配置 class 属性

配置模板，比如：

```
<TMNImage
  flag="flag_clickable"
  class="TMImageBean"
  src="${bgImgUrl}"
  layoutWidth="match_parent"
  layoutHeight="match_parent"
  visibility="@{${bgImgUrl} ? visible : gone }"/>
```

组件创建的时候会读取到 class 属性里的值，并在最好创建出实例来。

实现一个 `TMImageBean`，实现 `IBean` 接口，主要实现以下几个方法：

```
//初始化
void init(Context context, ViewBase view);
//销毁释放资源
void uninit();
//处理消息，这里的自定义消息，和之前提到的 VirtualView 内置事件类型无关
void doEvent(int type, int param, Object objParam);
```

然后在自定义基础组件的合适时机里调用，

```
IBean bean = mVirtualView.getBean();
if (null != bmp && null != bean) {
	bean.doEvent(EVENT_TYPE_IMAGE_LOADED, 0, bmp);
}
```

模板里写的是名称，而构造实例需要依赖类，所以还需要一个映射关系，注册方式是：

```
mVafContext.getBeanManager().register("TMImageBean", TMImageBean.class);
```

像这里的代码示例里，实现的是将图片组件父容器的边框绘制成 bitmap 里第一个像素的颜色的这么个逻辑。`TMImageBean` 里的 `doEvent` 逻辑会这么写：

```
public void doEvent(int type, int param, Object objParam) {
    switch (type) {
        case TMNativeImage.EVENT_TYPE_IMAGE_LOADED:
        {
            if (null != objParam) {
                Bitmap bmp = (Bitmap)objParam;
                int borderColor = bmp.getPixel(0, 0);
                ViewBase p = mSelf.getParent().getParent();
                if (null != p) {
                    p.setBorderColor(borderColor);
                }
            }
            break;
        }
    }
}
```

## 对比
第一种方式比较简单粗暴，第二种方式用起来繁琐一些，但是可以配置化的方式让基础组件开启或者不开启某种特殊逻辑。