---
title: "一拖N布局"
permalink: /docs/layout-support/oneplusn-layout
excerpt: "一拖N布局"
modified: 2016-11-03T10:01:43-04:00
redirect_from:
  - /theme-setup/
---

## 示意图

* 二个组件／三个组件

![](https://gw.alicdn.com/tfs/TB1V_QSPVXXXXatapXXXXXXXXXX-441-401.png)

* 四个组件(通过rows属性调整了上下比例)／五个组件

![](https://gw.alicdn.com/tfs/TB1njpvQXXXXXarXpXXXXXXXXXX-440-399.png)


<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg .tg-yw4l{vertical-align:top}
</style>
<table class="tg" style="undefined;table-layout: fixed; width: 766px">
<colgroup>
<col style="width: 93px">
<col style="width: 321px">
<col style="width: 167px">
<col style="width: 185px">
</colgroup>
  <tr>
    <th class="tg-yw4l">字段</th>
    <th class="tg-yw4l">意义</th>
    <th class="tg-yw4l">类型</th>
    <th class="tg-yw4l">样例</th>
  </tr>
  <tr>
    <td class="tg-yw4l">margin</td>
    <td class="tg-yw4l">卡片的外间距，顺序是上右下左</td>
    <td class="tg-yw4l">Array(内部是Number或者String)/String</td>
    <td class="tg-yw4l">[9,9,9,9],"[9,9,9,9]",["9","9","9","9"]均可，建议使用第一种</td>
  </tr>
  <tr>
    <td class="tg-yw4l">aspectRatio</td>
    <td class="tg-yw4l">整体宽高比</td>
    <td class="tg-yw4l">String</td>
    <td class="tg-yw4l">"9"</td>
  </tr>
  <tr>
    <td class="tg-yw4l">bgImgUrl</td>
    <td class="tg-yw4l">背景图</td>
    <td class="tg-yw4l">String</td>
    <td class="tg-yw4l">https://gw.alicdn.com/tps/TB1dkjfOXXXXXb0aXXXXXXXXXXX-1125-220.png</td>
  </tr>
  <tr>
    <td class="tg-yw4l">bgColor</td>
    <td class="tg-yw4l">背景色</td>
    <td class="tg-yw4l">String</td>
    <td class="tg-yw4l">"#FFFFFF"</td>
  </tr>
  <tr>
    <td class="tg-yw4l">cols</td>
    <td class="tg-yw4l">设定宽度比例，内容是百分比的数字。下面的1,2,3,4,5 指代第N个组件，具体可看样例图组件上的数字。三个组件时：1:2；四个组件：1:2:3:4;五个组件时:1:2:3:4:5</td>
    <td class="tg-yw4l">Array(String/Number)</td>
    <td class="tg-yw4l">["30","30"] 或  [30,30]</td>
  </tr>
  <tr>
    <td class="tg-yw4l">rows</td>
    <td class="tg-yw4l">设定上下比例，只能有两项</td>
    <td class="tg-yw4l">Array(String/Number)</td>
    <td class="tg-yw4l">["30","30"] 或  [30,30]</td>
  </tr>
</table>
