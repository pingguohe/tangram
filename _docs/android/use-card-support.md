---
title: "使用布局辅助模块"
permalink: /docs/android/use-card-support
excerpt: "使用布局辅助模块"
modified: 2016-11-03T10:01:43-04:00
redirect_from:
  - /theme-setup/
---

```CardSupport```，布局辅助模块，主要处里布局背景加载的回调，让业务方有能力去控制相关逻辑，业务方需要继承它并注册到 Tangram 里：

+ ```public abstract void onBindBackgroundView(View layoutView, Card card)```，绑定布局背景图片，业务方可以做自定义的加载图片逻辑；
+ ```public void onUnbindBackgroundView(View layoutView, Card card)```，接触绑定背景图片，与前者匹配；
+ ```public FixAreaLayoutHelper.FixViewAnimatorHelper onGetFixViewAppearAnimator(Card card)```，提供固定布局里的组件出现、隐藏时的动画；