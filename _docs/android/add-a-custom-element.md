---
title: "如何编写一个自定义基础组件"
permalink: /docs/android/add-a-custom-element
excerpt: "如何编写一个自定义基础组件"
modified: 2017-12-08T10:01:43-04:00
sidebar:
  title: "Android 使用指南"
  nav: android-docs
---

# Android

了解 VirtualView 里组件的编译、解析流程，有助于开发自定义基础组件，开发自定义基础组件分成两部分：解析器与组件实现，解析器是注册到编译工具里使用的，这一部分将来是可以优化省略掉，但目前还得手动写一个。组件实现需要按照规定的规范实现接口。下面是两个部分的步骤介绍，结合步骤和 Demo 里的源码可以更好的上手。

## 组件定义与编写解析器部分

![](https://gw.alicdn.com/tfs/TB1YYgleOqAXuNjy1XdXXaYcVXa-775-1125.jpg)

## 实现组件逻辑
![](https://gw.alicdn.com/tfs/TB1plm8hfDH8KJjy1XcXXcpdXXa-1130-1180.jpg)