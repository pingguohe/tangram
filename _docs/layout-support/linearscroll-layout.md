---
title: "横向滚动布局"
permalink: /docs/layout-support/linearscroll-layout
excerpt: "横向滚动布局"
modified: 2018-02-24T10:01:43-04:00
sidebar:
  title: "布局详细说明"
  nav: docs
---

适用于做线性的滚动，而不是Banner一页一页的滚动。

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg .tg-yw4l{vertical-align:top}
</style>
<table class="tg" style="undefined;table-layout: fixed; width: 770px">
<colgroup>
<col style="width: 140px">
<col style="width: 302px">
<col style="width: 170px">
<col style="width: 202px">
</colgroup>
  <tr>
    <th class="tg-yw4l">字段</th>
    <th class="tg-yw4l">意义</th>
    <th class="tg-yw4l">类型</th>
    <th class="tg-yw4l">样例</th>
  </tr>
  <tr>
    <td class="tg-yw4l">pageWidth</td>
    <td class="tg-yw4l">页面宽度，在iOS上配置了此参数，轮播布局的滚动会变为线性,不配置的话，就是一页一页的滚动，Android需要依赖此选型设置页面宽度</td>
    <td class="tg-yw4l">String/Number</td>
    <td class="tg-yw4l">"100"</td>
  </tr>
  <tr>
    <td class="tg-yw4l">pageHeight</td>
    <td class="tg-yw4l">页面高度</td>
    <td class="tg-yw4l">String/Number</td>
    <td class="tg-yw4l">"78"</td>
  </tr>
  <tr>
    <td class="tg-yw4l">hGap</td>
    <td class="tg-yw4l">横向每一帧之间的间距(Android不支持)</td>
    <td class="tg-yw4l">String/Number</td>
    <td class="tg-yw4l">"100"</td>
  </tr>
  <tr>
    <td class="tg-yw4l">scrollMarginLeft</td>
    <td class="tg-yw4l">最左边一帧距离布局左边的间距</td>
    <td class="tg-yw4l">String/Number</td>
    <td class="tg-yw4l">"100"</td>
  </tr>
  <tr>
    <td class="tg-yw4l">scrollMarginRight</td>
    <td class="tg-yw4l">最右边一帧距离布局右边的间距(iOS上不支持)</td>
    <td class="tg-yw4l">String/Number</td>
    <td class="tg-yw4l">"100"</td>
  </tr>
  <tr>
    <td class="tg-yw4l">hasIndicator</td>
    <td class="tg-yw4l">是否显示指示器</td>
    <td class="tg-yw4l">Boolean</td>
    <td class="tg-yw4l">false</td>
  </tr>
  <tr>
    <td class="tg-yw4l">indicatorColor</td>
    <td class="tg-yw4l">指示器选中颜色(iOS不支持)</td>
    <td class="tg-yw4l">Color</td>
    <td class="tg-yw4l">#FF0000 </td>
  </tr>
  <tr>
    <td class="tg-yw4l">defaultIndicatorColor</td>
    <td class="tg-yw4l">指示器默认颜色(iOS不支持)</td>
    <td class="tg-yw4l">Color</td>
    <td class="tg-yw4l">#666666 </td>
  </tr>
  
</table>
