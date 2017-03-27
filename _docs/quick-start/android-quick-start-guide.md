---
title: "Android Quick-Start Guide"
permalink: /docs/quick-start-guide/android-quick-start-guide/
excerpt: "Android 快速开始指南."
modified: 2016-11-03T10:01:43-04:00
redirect_from:
  - /theme-setup/
---

## 1.引入依赖

```
// gradle
compile 'com.tmall.android:tangram:1.0.0@aar'
```

或者

```
// maven
<dependency>
  <groupId>com.tmall.android</groupId>
  <artifactId>tangram</artifactId>
  <version>1.0.0</version>
  <type>aar</type>
</dependency>
```

## 2.初始化 Tangram 环境

应用全局只需要初始化一次，提供一个通用的图片加载器，一个应用内通用的ImageView类型（通常情况下每个应用都有自定义的 ImageView，如果没有的话就提供系统的 ImageView 类）。

```
TangramBuilder.init(context, new IInnerImageSetter() {
	@Override
	public <IMAGE extends ImageView> void doLoadImageUrl(@NonNull IMAGE view,
                    @Nullable String url) {
		//假设你使用 Picasso 加载图片
		Picasso.with(context).load(url).into(view);
	}
}, ImageView.class);
```

## 3.初始化 ```TangramBuilder```

在 Activity 中初始化```TangramBuilder```，假设你的 Activity 是```TangramActivity```。

```
TangramBuilder.InnerBuilder builder = TangramBuilder.newInnerBuilder(TangramActivity.this);
```

这一步 builder 对象生成的时候，内部已经注册了框架所支持的所有组件和卡片，以及默认的```IAdapterBuilder```（它被用来创建 绑定到 RecyclerView 的Adapter）。

## 4.注册自定义的卡片和组件

一般情况下，内置卡片的类型已经满足大部分场景了，业务方主要是注册一下自定义组件。

```
builder.registerCell(1, TestView.class);
```

意思是类型为1的组件渲染时会被绑定到```TestView```的实例上。

更多组件信息请参考[组件开发](/docs/android/develop-component)。

## 5.生成```TangramEngine```实例

在上述基础上调用：

```
TangramEngine engine = builder.build();
```

## 6.绑定 recyclerView

```
setContentView(R.layout.main_activity);
RecyclerView recyclerView = (RecyclerView) findViewById(R.id.main_view);
...
engine.bindView(recyclerView);
```

# 7.加载数据并传递给 engine

数据一般是调用接口加载远程数据，这里演示的是 mock 加载本地的数据：

```
String json = new String(getAssertsFile(this, "data.json"));
        JSONArray data = null;
        try {
            data = new JSONArray(json);
            engine.setData(data);
        } catch (JSONException e) {
            e.printStackTrace();
        }
```

完整页面的数据结构可参考 [Demo]() 里。