---
title: "绑定数据"
permalink: /docs/virtualview/simple-expr
excerpt: "绑定数据"
modified: 2017-12-07T20:05:12-04:00
sidebar:
  title: "VirtualView基本原理"
  nav: virtualview-docs
---

开发业务组件的时候，UI 元素的基础属性或者样式往往不能在模板里直接写死，而是需要从数据里获取，所以借鉴其他模板化方案引入了用户数据绑定的表达式，包括两括对象属性的访问和三元操作符。

## 访问数据属性的 EL 表达式

基于标准 Java bean 规范，通过 EL 访问对象的属性，语法上以 ${ 开头，以 } 结束。对于 POJO 以及 Map，通过 . 操作符进行访问，对于 Array 或者 List 通过 [] 操作符进行访问。

示例

```
class Person { String name; public Person(){ name="jack"; } }

List<Object> list = new ArrayList<Object>();
list.add("person", new Person());
list.add("url2");

Map<String, Object> data = new HashMap<String, Object>();
data.put("list", list);
data.put("logo", "https://gw.alicdn.com/tps/TB1wwiKKV.png");
```

+ ${logo}: https://gw.alicdn.com/tps/TB1wwiKKV.png
+ ${list[0].name}: jack

```
{
  "ID": null,
  "name": "Doe",
  "first-name": "John",
  "age": 25,
  "hobbies": [
    "reading",
    "cinema",
    {
      "sports": [
        "volley-ball",
        "badminton"
      ]
    }
  ],
  "address": {}
}
```

+ ${first-name}: John
+ ${hobbies[0]}: reading
+ ${hobbies[2].sports[0]: volley-ball

## 三元操作符

三元操作符用来给那些需要根据数据中某个字段来设置值的属性，比如当存在 titleColor 字段的时候，设置文本的颜色为 titleColor 指定的值，否则用默认的黑色。语法上以 @{ 开头，以 } 结束，中间部分为表达式的具体内容。

`条件表达式 ? 结果表达式[1] : 结果表达式[2]`

当`条件表达式`成立的时候，使用`结果表达式[1]`，否则使用`结果表达式[2]`。三个表达式均支持 EL 解析。

安卓 SDK 中以下场景均为 false：
+ 布尔类型值为 false
+ 字符串为 null 或者 "" 或者 "null"
+ 字符串 "false" 或者 "FALSE"
+ JSONObject 为空或 JSONObject.NULL
+ JSONArray 长度为 0
+ 字段不存在

iOS SDK 中以下场景均为 false:
- key 对应的值为字串，且字串为 "false"
- key 对应的值不存在

示例：

`@{${logoUrl} ? visible : invisible }`
`@{${titleColor} ? ${titleColor} : black }` 

## 一个实际的例子

以下 `NImage` 和 `VText` 分别使用了 EL 表达式和三元操作符：

```
<VHLayout
        flag="flag_exposure|flag_clickable"
        action="action"
        orientation="V"
        gravity="v_center"
        layoutWidth="match_parent"
        layoutHeight="match_parent"
>
    <NImage
            id="1"
            layoutGravity="h_center"
            src="${imgUrl}"
            scaleType="center_crop"
            layoutMarginBottom="6"
            layoutWidth="96rp"
            layoutHeight="96rp"/>
    <VText
            id="2"
            layoutGravity="h_center"
            text="${title}"
            textSize="12"
            textColor="@{${titleColor} ? ${titleColor} : #333333 }"
            layoutWidth="wrap_content"
            layoutHeight="wrap_content"/>
</VHLayout>
```