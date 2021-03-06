---
title: "二进制文件格式"
permalink: /docs/virtualview/bin-format
excerpt: "二进制文件格式"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### 二进制文件的格式

通过 XML 编写的业务组件，并不直接在客户端里运行使用，而是先进行一次二进制序列化操作，原始的 XML 模板文件保存成文件的时候，就是以纯文本的形式存在，会包含很多冗余信息，比如空格、换行、还有重复出现的字符串等，文件体积比较大，以xml解析器去解析的时候，也会需要大量字符串操作，效率和性能不能达到最优。而将它编译成二进制格式，会避免这些问题，比如文件重复出现的字符串只保留一份，通过字符串索引去引用它，所有的组件类型也都会被转换成一个数字索引，在客户端内通过数字索引反过来找到对应的类实例化。这样文件格式会非常紧凑，体积更小。整个设计也借鉴了 Android 系统编译模板文件的思路。它的具体格式说明如下：

![](https://gw.alicdn.com/tfs/TB1H9.tg8fH8KJjy1XbXXbLdXXa-1270-300.jpg)

按照图中从左往右、从上往下的顺序分别说明每个段的作用：

+ 开始5个字节固定为 ALIVV；相当于我们的文件格式的一个标记。
+ 版本号分三个，分别为主版本号，次版本号和修订版本号，均为 2 个字节；在无重大重构更新时，前两位一般不变，第三位用于组件的业务级别变更升级；
+ 组件区的起始位置和长度，均为 4 个字节；表示这份文件里组件区数据从第几个字节开始，它总共有多少个字节，这样解析这份数据的时候能直接将文件指针定位到特定位置来读取数据。
+ 字符串区的起始位置和长度，均为 4 个字节；表示这份文件里字符串数据从第几个字节开始，它总共有多少个字节。
+ 表达式区的起始位置和长度，均为 4 个字节；表示这份文件里字符串数据从第几个字节开始，它总共有多少个字节。
+ 数据区的起始位置和长度，均为 4 个字节；表示这份文件里附加数据从第几个字节开始，它总共有多少个字节。目前这一区块是作为一种保留区，实际还未使用到。
+ 当前文件所属页编码，2 个字节，唯一标识一个页（保留使用）
+ 当前文件依赖页的个数为 2 个字节，后面为依赖页的 Id，依赖页个数大于 0 表示该页用到了其他页的资源或者代码，在该页加载之前需要确保依赖页必须已经加载；（保留使用）
+ 组件区开始，前 4 个字节表示文件里业务组件个数，目前一个 XML 模板编译成一个二进制文件，故其值固定为 1。每个业务组件前 2 个字节表示业务组件名称字符串的长度，后面为指定长度的字符串字节数据；紧接着是 2 个字节的编译后组件二进制流长度，后面为二进制代码；
+ 字符串区开始，前4个字节表示字符串个数，在我们的框架里，会内置一些系统级别的字符串资源，比如上文5.2开端表格里提到的那些属性名，这些字符串不用序列化到二进制文件里，而模板文件里出现的非系统字符串才会作为资源序列化到二进制文件。每个字符串资源前 4 个字节字符串索引 Id 即它的 hashCode，后面 2 个自己为字符串的长度，再后面为对应的字符串；
+ 逻辑表达式代码表。前 4 个字节表示逻辑表达式资源个数，每个表达式资源前4个自己表示表达式的索引，它是表达式原始字符串的hashCode，后面两个2 个字节表示表达式的长度，后面为对应的表达式内容，它是表达式按照关键字切割后的字符串结构；
+ 扩展数据段是保留为第三方扩展使用；