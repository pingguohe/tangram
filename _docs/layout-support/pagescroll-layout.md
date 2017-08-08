---
title: "轮播/横向滚动布局"
permalink: /docs/layout-support/pagescroll-layout
excerpt: "轮播/横向滚动布局"
modified: 2016-11-03T10:01:43-04:00
sidebar:
  title: "布局详细说明"
  nav: docs
---

适用于Banner的场景，可自动滚动，循环滚动，也可以做线性的滚动，而不是Banner一页一页的滚动。

## 示意图

<img src="https://gw.alicdn.com/tfs/TB1vP0fQXXXXXaHapXXXXXXXXXX-720-1280.gif" width = "360" height = "640"/>

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg .tg-yw4l{vertical-align:top}
</style>
<table class="tg" style="undefined;table-layout: fixed; width: 770px">
<colgroup>
<col style="width: 120px">
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
    <td class="tg-yw4l">margin</td>
    <td class="tg-yw4l">卡片的外间距，顺序是上右下左</td>
    <td class="tg-yw4l">Array(内部是Number或者String)/String</td>
    <td class="tg-yw4l">[9,9,9,9],"[9,9,9,9]",["9","9","9","9"]均可，建议使用第一种</td>
  </tr>
  <tr>
    <td class="tg-yw4l">padding</td>
    <td class="tg-yw4l">卡片的内间距，顺序是上右下左</td>
    <td class="tg-yw4l">Array(内部是Number或者String)/String</td>
    <td class="tg-yw4l">[9,9,9,9],"[9,9,9,9]",["9","9","9","9"]均可，建议使用第一种</td>
  </tr>
  <tr>
    <td class="tg-yw4l">autoScroll</td>
    <td class="tg-yw4l">自动滚动的间隔，单位毫秒，填写数字大于0就开始自动滚动，默认值</td>
    <td class="tg-yw4l">String/Number</td>
    <td class="tg-yw4l">"3000",3000</td>
  </tr>
  <tr>
    <td class="tg-yw4l">specialInterval</td>
    <td class="tg-yw4l">单独指定每一帧的自动滚动的间隔，单位毫秒，key从0开始计数；与autoScroll配合使用，当未在此声明某一帧的停留时间的时候，使用autoScroll指定的间隔，否则使用此处声明的间隔时间</td>
    <td class="tg-yw4l">Map</td>
    <td class="tg-yw4l">{"1": "10000", "2": "5000"}</td>
  </tr>
  <tr>
    <td class="tg-yw4l">infinite</td>
    <td class="tg-yw4l">是否无限滚动</td>
    <td class="tg-yw4l">String/Bool</td>
    <td class="tg-yw4l">"true","false"</td>
  </tr>
  <tr>
    <td class="tg-yw4l">indicatorImg1</td>
    <td class="tg-yw4l">指示器选中状态的图片，必须带图片宽高比后缀</td>
    <td class="tg-yw4l">String</td>
    <td class="tg-yw4l">https://img.alicdn.com/tps/TB16i4qNXXXXXbBXFXXXXXXXXXX-32-4.png</td>
  </tr>
  <tr>
    <td class="tg-yw4l">indicatorImg2</td>
    <td class="tg-yw4l">指示器未被选中状态的图片，必须带图片宽高比后缀</td>
    <td class="tg-yw4l">String</td>
    <td class="tg-yw4l">https://img.alicdn.com/tps/TB1XRNFNXXXXXXKXXXXXXXXXXXX-32-4.png</td>
  </tr>
  <tr>
    <td class="tg-yw4l">indicatorGravity</td>
    <td class="tg-yw4l">指示器位置，居中居左还是居右</td>
    <td class="tg-yw4l">String</td>
    <td class="tg-yw4l">"left"/"right"/"center"</td>
  </tr>
  <tr>
    <td class="tg-yw4l">indicatorPosition</td>
    <td class="tg-yw4l">指示器位置，在内部还是在外部</td>
    <td class="tg-yw4l">String</td>
    <td class="tg-yw4l">"inside"/"outside"</td>
  </tr>
  <tr>
    <td class="tg-yw4l">indicatorGap</td>
    <td class="tg-yw4l">每个之间的指示器间距</td>
    <td class="tg-yw4l">String/Number</td>
    <td class="tg-yw4l">"9"</td>
  </tr>
  <tr>
    <td class="tg-yw4l">indicatorMargin</td>
    <td class="tg-yw4l">指示器相对于布局底端的间距</td>
    <td class="tg-yw4l">String/Number</td>
    <td class="tg-yw4l">"9"</td>
  </tr>
  <tr>
    <td class="tg-yw4l">indicatorHeight</td>
    <td class="tg-yw4l">指示器高度</td>
    <td class="tg-yw4l">String/Number</td>
    <td class="tg-yw4l">"9"</td>
  </tr>
  <tr>
    <td class="tg-yw4l">pageWidth</td>
    <td class="tg-yw4l">页面宽度，配置了此参数，轮播布局的滚动会变为线性,不配置的话，就是一页一页的滚动</td>
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
    <td class="tg-yw4l">最右边一帧距离布局右边的间距</td>
    <td class="tg-yw4l">String/Number</td>
    <td class="tg-yw4l">"100"</td>
  </tr>
  <tr>
    <td class="tg-yw4l">hGap</td>
    <td class="tg-yw4l">横向每一帧之间的间距</td>
    <td class="tg-yw4l">String/Number</td>
    <td class="tg-yw4l">"100"</td>
  </tr>
</table>

