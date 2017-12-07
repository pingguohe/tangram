---
title: "动态化组件——VirtualView"
permalink: /docs/virtualview/about-virtualview
excerpt: "About VirtualView."
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "动态化组件介绍"
  nav: virtualview-docs
---

VirtualView 是 Tangram 升级过程中引入的新的组件开发技术，它开创了一种虚拟化开发基础组件的技术，使用方只要按照指定协议实现一个基础组件的尺寸计算、绘制逻辑、布局逻辑，即能实现在宿主容器的 canvas 里实现直接绘制 UI 内容的，让最终渲染出来的视图结构呈现扁平化，提升组件渲染性能。同时为了解决虚拟化 View 带来的原生 View 的能力损失的问题，它支持加载和渲染原生基础组件，两者组合产生合力，既能最大限度发挥性能提升，又能满足特殊场景下的业务需求。
VirtualView 内置实现了一系列基础组件，可以让使用方直接上手尝试；而搭建业务组件的方式采用 XML 模板来编写，这使得业务组件动态更新成为了可能。XML 模板里还支持写数据绑定的表达式，在样式动态化、数据动态化的场景下能非常方便地实现业务需求。

要学习使用 VirtualView，需要从四个方面入手：
+ 了解 VirtualView 模板数据的格式；
+ 了解 VirtualView 的基本原理，包括从模板编译、解析、绑定数据几个主要流程；
+ 了解 VirtualView 的基本接入方式，初始化、添加自定义基础组件、添加与外部的逻辑交互等；
+ 了解 VirtualView 内置基础组件的特性，避免重复开发；