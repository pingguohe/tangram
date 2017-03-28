---
title: "使用定时器"
permalink: /docs/android/use-timer
excerpt: "使用定时器"
modified: 2016-11-03T10:01:43-04:00
sidebar:
  title: "Android 使用指南"
  nav: android-docs
---

组件的业务逻辑里难免有涉及到定数触发的逻辑，比如倒计时、定时滚动。Tangram 内置了定时器模块，可以全局复用，防止重复开发。以在组件里使用定时器为例：
在 bindView 或者 postBindView 方法里注册定时器：

```
if (cell.serviceManager != null) {
    TimerSupport timerSupport = cell.serviceManager.getService(TimerSupport.class);
    if (timerSupport != null && !timerSupport.isRegistered(this)) {
    	//第一个参数4是单位秒，第二个参数是接口回调，第三个参数是立即执行
        timerSupport.register(4, this, true);
    }
}
```

实现回调接口：

```
TimerSupport.OnTickListener
```

```
@Override
public void onTick() {
	//处理业务逻辑        
}
```

在 unbindView 或者 postUnbindView 的方法里要记得注销定时器：

```
if (cell.serviceManager != null) {
    TimerSupport timerSupport = cell.serviceManager.getService(TimerSupport.class);
    if (timerSupport != null) {
        timerSupport.unregister(this);
    }
}
```