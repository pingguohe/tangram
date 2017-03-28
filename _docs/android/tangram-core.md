---
title: "TangramView核心方法"
permalink: /docs/android/tangram-core
excerpt: "TangramView核心方法"
modified: 2016-11-03T10:01:43-04:00
sidebar:
  title: "Android 使用指南"
  nav: android-docs
---

## TangramBuilder 核心方法

```TangramBuilder```是初始化 Tangram 运行环境，初始化```TangramEngine```的模块，它有一些核心方法，必须重点介绍。

---

```
public static void init(@NonNull final Context context, IInnerImageSetter innerImageSetter, Class<? extends ImageView> imageClazz)
```

初始化 Tangram，初始化在应用全局只需要调用一次即可。主要提供一个[```InnerImageSetter```]()，它是加载图片的通用接口，用来在框架内部加载图片时调用。还要提供一个图片基类，通常一个应用会自定义一个 ImageView，为了让 Tangram 内部也能使用业务方自定义的 ```ImageView```，在初始化的时侯也将该类传给 Tangram。

---

```
public static InnerBuilder newInnerBuilder(@NonNull final Context context)
```

构造真正的 builder 对象，返回的```InnerBuilder```对象里已经注册了 Tangram 内置的卡片，业务方拿到这个 buidler 对象主要是去注册自定义的卡片和组件。最后调用```public TangramEngine build()```生成```TangramEngine```。

---

## TangramEngine 核心方法

```TangramEngine```是绑定 Tangram 数据，绑定 RecyclerView 的核心模块，也是操作页面数据的核心模块，它的一些核心方法，也必须重点介绍。

---

```
public void bindView(@NonNull final RecyclerView view)
```

绑定 recyclerView 对象。

```
public void unbindView()
```

清空所绑定的 recyclerView 对象、adapter对象等。

```
public RecyclerView getContentView()
```

获取所绑定的 recyclerView 对象，这一步应当在调用 bindView 之后调用。

---

```
public void destroy()
```

页面退出时调用，释放内部资源。

---

```
public List<C> parseData(@Nullable T data)
```
解析卡片json数据

```
public List<L> parseComponent(@Nullable T data)
```
解析组件json数据

---

```
public void setData(@Nullable T data)
```
绑定整个页面的数据

```
public void appendData(@Nullable T data)
```
添加卡片数据

```
public void insertData(int position, @Nullable T data)
```
插入卡片数据

```
public void replaceData(int position, @Nullable T data)
```
替换卡片数据

上述几个方法都有原始json 数据类型和解析过的卡片 model 数据类型的版本。

---

```
public <S> void register(@NonNull Class<S> type, @NonNull S service)
```

注册自定义的辅助模块

```
public <S> S getService(@NonNull Class<S> type)
```
获取注册的辅助模块

---

```
public void refresh()
```
刷新页面

```
public void onScrolled()
```
触发页面懒加载数据的逻辑