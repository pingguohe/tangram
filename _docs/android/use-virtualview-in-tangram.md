---
title: "在 Tangram 中使用 VirtualView"
permalink: /docs/android/use-virtualview-in-tangram
excerpt: "在 Tangram 中使用 VirtualView"
modified: 2017-12-08T10:01:43-04:00
sidebar:
  title: "Android 使用指南"
  nav: android-docs
---

在 Tangram 里使用 VirtualView 的时候，大致流程如上述所示，只不过很多步骤已经内置到 Tangram 的初始化里了，外部只需要注册业务组件类型、加载模板数据、提供事件处理器。

注册 VirtualView 版本的 Tangram 组件，只需要提供组件类型名称即可，

```
Tangram.Builder builder = Tangram.newBuilder(activity);
builder.registerVirtualView("tmallcomponent1");
builder.registerVirtualView("tmallcomponent2");
```

在 TangramEngine 构建出来之后加载模板数据，

```
tangramEngine.setVirtualViewTemplate(TMALLCOMPONENT1.BIN);
tangramEngine.setVirtualViewTemplate(TMALLCOMPONENT2.BIN);
...
```

同样的有必要的话需要注册自定义基础组件的构造器，

```
ViewManager viewManager = tangramEngine.getService(ViewManager.class);
viewManager.getViewFactory().registerBuilder(BizCommon.TM_PRICE_TEXTVIEW, new TMPriceView.Builder());
...
```

注册事件处理器，

```
VafContext vafContext = tangramEngine.getService(VafContext.class);
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

更多关于在 Tangram 中使用 VirtualView 的信息，参考文档。TODO 