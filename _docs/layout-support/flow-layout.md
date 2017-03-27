---
title: "流式(网格)布局"
permalink: /docs/layout-support/flow-layout
excerpt: "流式(网格)布局"
modified: 2016-11-03T10:01:43-04:00
redirect_from:
  - /theme-setup/
---

支持属性列表\(单列、双列...到五列，支持的属性都是一样的\)

## 示意图

![](https://gw.alicdn.com/tfs/TB1DE0xQXXXXXatXpXXXXXXXXXX-520-401.png)
![](https://gw.alicdn.com/tfs/TB1gDkPPVXXXXbiaFXXXXXXXXXX-598-399.png)
![](https://gw.alicdn.com/tfs/TB1keXpQXXXXXXCXFXXXXXXXXXX-468-400.png)


<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg .tg-yw4l{vertical-align:top}
</style>
<table class="tg" style="undefined;table-layout: fixed; width: 723px">
<colgroup>
<col style="width: 85px">
<col style="width: 282px">
<col style="width: 154px">
<col style="width: 202px">
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
    <td class="tg-yw4l">Array(内部是Number或者String)String</td>
    <td class="tg-yw4l">[9,9,9,9],"[9,9,9,9]"，["9","9","9","9"]均可，建议使用第一种</td>
  </tr>
  <tr>
    <td class="tg-yw4l">padding</td>
    <td class="tg-yw4l">卡片的内间距，顺序是上右下左</td>
    <td class="tg-yw4l">Array(内部是Number或者String)String</td>
    <td class="tg-yw4l">[9,9,9,9],"[9,9,9,9]",["9","9","9","9"]均可，建议使用第一种</td>
  </tr>
  <tr>
    <td class="tg-yw4l">aspectRatio</td>
    <td class="tg-yw4l">每一行的宽高比</td>
    <td class="tg-yw4l">String</td>
    <td class="tg-yw4l">"9"</td>
  </tr>
  <tr>
    <td class="tg-yw4l">vGap</td>
    <td class="tg-yw4l">垂直方向上，每个组件的间距</td>
    <td class="tg-yw4l">String\/Number</td>
    <td class="tg-yw4l">"9"</td>
  </tr>
  <tr>
    <td class="tg-yw4l">hGap</td>
    <td class="tg-yw4l">水平方向上，每个组件的间距</td>
    <td class="tg-yw4l">String\/Number</td>
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
    <td class="tg-yw4l">每列的百分比，如果是N列，可以只写Array中只写N-1项，最后一项会自动填充，如果加一起大于100，就按照填写的来算</td>
    <td class="tg-yw4l">Array(String/Number)</td>
    <td class="tg-yw4l">["30","30"] 或  [30",30]</td>
  </tr>
</table>