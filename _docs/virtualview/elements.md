---
title: "基础组件模型"
permalink: /docs/virtualview/elements
excerpt: "基础组件模型"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

### `公共属性`

组件模型的通用属性定义。

### 虚拟view与实体view

虚拟view，即view的内容直接通过宿主的canvas绘制出来，它本身并不需要一个实体的view存在，在真实的view tree中，是看不到这个实例，只能看到其宿主的存在。它包括：原子虚拟view组件比如文本、图片、线条，布局虚拟view组件比如线性布局、帧布局等。虚拟view也遵循Android绘制view的逻辑，需要响应measure、layout、draw的过程才能显示。

实体view，即原生的native view。

### 属性

|名称|类型|默认值|描述|是否支持表达式|
|---|---|---|---|---|
|id|int|0|组件id|否|
|layoutWidth|int<br>float<br>enum:<br>    match_parent<br/>    wrap_content|0|组件的布局宽度，与Android里的概念类似，写绝对值的时候表示绝对宽高，match_parent表示尽可能撑满父容器提供的宽高，wrap_content表示根据自身内容的宽高来布局|是|
|layoutHeight|int<br>float<br>enum:<br/>    match_parent<br/>    wrap_content|0|组件的布局宽度，与Android里的概念类似，写绝对值的时候表示绝对宽高，match_parent表示尽可能撑满父容器提供的宽高，wrap_content表示根据自身内容的宽高来布局|是|
|layoutGravity|enum:<br>    left<br/>    right<br/>    top<br/>    bottom<br/>    v_center<br/>    h_center|left\|top|描述组件在容器中的对齐方式，left：靠左，right：靠右，top：靠上，bottom：靠底，v_center：垂直方向居中，h_center：水平方向居中，可用`或`组合描述|否|
|autoDimX|int<br/>float|1|组件宽高比计算的横向值|是|
|autoDimY|int<br/>float|1|组件宽高比计算的竖向值|是|
|autoDimDirection|enum:<br/>    X<br/>    Y<br/>    NONE|NONE|组件在布局中的基准方向，用于计算组件的宽高比，与autoDimX、autoDimY配合使用，设置了这三个属性时，在计算组件尺寸时具有更高的优先级。当autoDimDirection=X时，组件的宽度由layoutWidth和父容器决策决定，但高度 = width * (autoDimY / autoDimX)，当autoDimDirection=Y时，组件的高度由layoutHeight和父容器决策决定，但宽度 = height * (autoDimX / autoDimY)|否|
|minWidth(iOS暂未支持)|int<br/>float|0|最小宽度|否|
|minHeight(iOS暂未支持)|int<br/>float|0|最小高度|否|
|padding|int<br/>float|0|同时设置 4 个内边距|是|
|paddingLeft|int<br/>float|0|左内边距，优先级高于 padding|是|
|paddingRight|int<br/>float|0|右内边距，优先级高于 padding|是|
|paddingTop|int<br/>float|0|上内边距，优先级高于 padding|是|
|paddingBottom|int/float|0|下内边距，优先级高于 padding|是|
|layoutMargin|int/float|0|同时设置 4 个外边距|是|
|layoutMarginLeft|int/float|0|左外边距，优先级高于 layoutMargin|是|
|layoutMarginRight|int/float|0|右外边距，优先级高于 layoutMargin|是|
|layoutMarginTop|int/float|0|上外边距，优先级高于 layoutMargin|是|
|layoutMarginBottom|int/float|0|下外边距，优先级高于 layoutMargin|是|
|background|int|0|背景色|是|
|borderWidth|int<br/>float|0|边框宽度|是|
|borderColor|int|0|边框颜色|是|
|borderRadius|int<br/>float|0|边框四个角的圆角半径，与 borderWidth 配合使用，支持NText、VText、VHLayout、VH2Layout、FrameLayout、GridLayout|是|
|borderTopLeftRadius|int<br/>float|0|单独设置左上角圆角半径，使用同上(iOS仅Layout支持单独设置)，优先级高于 borderRadius|是|
|borderTopRightRadius|int<br/>float|0|单独设置右上角圆角半径，使用同上(iOS仅Layout支持单独设置)，优先级高于 borderRadius|是|
|borderBottomLeftRadius|int<br/>float|0|单独设置左下角圆角半径，使用同上(iOS仅Layout支持单独设置)，优先级高于 borderRadius|是|
|borderBottomRightRadius|int<br/>float|0|单独设置右下角圆角半径，使用同上(iOS仅Layout支持单独设置)，优先级高于 borderRadius|是|
|visibility|enum:<br/>    visible<br/>    invisible<br/>    gone|visible|可见性，与Android里的概念类似，visible：可见，invisible：不可见，但占位，gone：不可见也不占位|是|
|dataTag|string|组件数据标识||是|
|flag|enum:<br/>    flag_software<br>    flag_exposure<br>    flag_clickable<br>     flag_longclickable<br>    flag_touchable|组件行为定义|flag_software：关闭view的硬件加速，flag_exposure：需要触发曝光事件，flag_clickable：需要响应点击事件，flag_longclickable：需要响应长按事件，flag_touchable：需要响应触摸事件|否|
|action|string|null|（表示点击事件触发之后跳转到数据中action字段定义的页面）|是|
|class|string|null|跟组件绑定的逻辑处理对象名称|是|

### 数值单位

在组件属性里，跟尺寸相关的属性，其值的单位默认是dp，比如layoutWidth=10，表示宽度是10dp；实际值 = dp * density；

为了更精准地适配视觉，支持rp单位，表示适配屏幕大小的值，比如layoutWidth=10rp，实际值 = 10 * 屏幕宽度 / 750；这里 750 指的是设计稿里的屏幕宽度，默认值是 750，可以通过 `com.libra.Utils.setUedScreenWidth()` 方法修改默认值。

### 颜色值

在组件颜色相关属性里，支持16进制数表示，#RRGGBB、#AARRGGBB，也支持以下几个颜色文本：

+ black
+ blue
+ cyan
+ dkgray
+ gray
+ green
+ ltgray
+ magenta
+ red
+ transparent
+ yellow

