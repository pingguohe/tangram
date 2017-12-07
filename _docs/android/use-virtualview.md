---
title: "单独使用VirtualView"
permalink: /docs/android/use-virtualview
excerpt: "单独使用VirtualView"
modified: 2017-12-08T10:01:43-04:00
sidebar:
  title: "Android 使用指南"
  nav: android-docs
---

到 Github 上查找最新版本，最新的 aar 都会发布到 jcenter 和 MavenCentral 上，确保配置了这两个仓库源，然后引入aar依赖：

``` gradle
compile ('com.alibaba.android:virtualview:1.0.0@aar') {
	transitive = true
}
```

或者maven:
pom.XML
``` XML
<dependency>
  <groupId>com.alibaba.android</groupId>
  <artifactId>virtualview</artifactId>
  <version>1.0.0</version>
  <type>aar</type>
</dependency>
```

构建一个 `VafContext` 对象，

```
VafContext vafContext = new VafContext(mContext.getApplicationContext());
```
初始化一下图片加载器，如果只是运行 Demo 程序，使用内置的基础图片组件 NImage 和 VImage，那么需要初始化一下图片加载器，在真实的产品里，一般不需要这一步，因为往往需要使用方注册使用自己的图片组件；

```
VafContext.loadImageLoader(mContext.getApplicationContext());
```

初始化 `ViewManager` 对象，

```
ViewManager viewManager = vafContext.getViewManager();
viewManager.init(mContext.getApplicationContext());
```

加载模板数据，利用 VirtualView Tools 编译出二进制文件，在初始化的时候加载，有两张方式，一种是直接加载二进制字节数组（推荐）：

```
viewManager.loadBinBufferSync(TMALLCOMPONENT1.BIN);
viewManager.loadBinBufferSync(TMALLCOMPONENT2.BIN);
......
```

另一种是通过二进制文件路径加载：

```
viewManager.loadBinFileSync(TMALLCOMPONENT1_PATH);
viewManager.loadBinFileSync(TMALLCOMPONENT2_PATH);
......
```

如果开发了自定义的基础组件，注册自定义组件的构造器：(开发自定义组件的说明参考：TODO)

```
viewManager.getViewFactory().registerBuilder(BizCommon.TM_PRICE_TEXTVIEW, new TMPriceView.Builder());
viewManager.getViewFactory().registerBuilder(BizCommon.TM_TOTAL_CONTAINER, new TotalContainer.Builder());
```

注册事件处理器，比如常用的点击、曝光处理：(更多事件处理信息的说明参考：TODO）

```
vafContext.getEventManager().register(EventManager.TYPE_Click, new IEventProcessor() {

    @Override
    public boolean process(EventData data) {
        //handle here
        return true;
    }
});
vafContext.getEventManager().register(EventManager.TYPE_Exposure, new IEventProcessor() {

    @Override
    public boolean process(EventData data) {
        //handle here
        return true;
    }
});
```

通过组件名参数 name 生成组件实例，

```
View container = vafContext.getContainerService().getContainer(name, true);
mLinearLayout.addView(container);
```

如果组件模板里写了数据绑定的表达式，那么需要给组件绑定真实的数据，

```
IContainer iContainer = (IContainer)container;
JSONObject json = getJSONDataFromAsset(data);
if (json != null) {
    iContainer.getVirtualView().setVData(json);
}
``` 