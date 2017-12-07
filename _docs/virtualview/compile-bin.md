---
title: "解析编译后的模板数据"
permalink: /docs/virtualview/parse-bin
excerpt: "解析编译后的模板数据"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

编译阶段是独立于客户端运行时的一个过程，客户端渲染组件，是从解析编译好的二进制数据开始，解析起始就是编译过程的逆过程，但解析流程只负责把原始数据提取出来，组织好格式，并没有直接构建出组件对象。理解了二进制数据的协议及编译过程，再看解析过程就比较清楚了。详细说明每一个步骤：

![](https://gw.alicdn.com/tfs/TB1uxWIhf6H8KJjy0FjXXaXepXa-720-1710.jpg)
