---
title: "接入Tangram代码"
permalink: /docs/ios/access-tangram
excerpt: "接入Tangram代码"
modified: 2016-11-03T10:01:43-04:00
redirect_from:
  - /theme-setup/
---

Tangram要求iOS系统版本在 6.0及以上即可

## 使用Cocoapods

在Podfile里面增加Tangram依赖即可

````
pod 'Tangram
````


## 直接使用代码依赖

直接把Tangram中Source和Resource文件夹加入到工程中即可。

需要注意的是，Tangram本身也有依赖，需要把依赖加到Podfile中或者自行依赖下面的这些模块

Tangram本身需要的依赖有：

```
 pod 'LazyScroll', ~>'0.0.2'
 pod 'RegexKitLite', ~>'4.0'
 pod 'JSONKit', ~>'2.1.0'
 pod 'SDWebImage', ~>'3.7.3.24'
```

