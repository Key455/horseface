---
title: 关于运算符中null和undefined
date: 2020-07-03 18:05:03
tags:
banner_img: /img/null.jpg
index_img: /img/null.jpg
---

# 关于运算符中null和undefined

最近复习了一下关于JS中运算符的东西，总结一下null和undefined的比较:

- **null与undefind的比较**

```js
    //关系运算符中null、undefined与0的结果
    null >= 0  //true
    undefined >= 0  //false

    //相等运算符中null、undefined与0的结果
    null == 0  //false
    undefined == 0  //false
```

我们来整理一下：

```js
    Number(null); //0
    null >= 0;  // ture
    Number(undefined); //NaN
    undefined >= 0; //flase
    null >= null; //Number(null) >= Number(null)，0 >= 0，结果为ture
    undefined >= undefined; //Number(undefined) >= Number(undefined)，NaN >= NaN，结果为flase

    null == undefined; // ture
    null == null; // ture
    undefined == undefined; // true

    null == 0; //flase
    undefined == 0; //flase
```

总结结果如下，也是以后需要注意的点:

- 在关系运算符中，null、undefined**会被强制转换为Number类型**再进行比较

- 在相等运算符中，null、undefiner则**不会被强制转换为Number**，处于**原型状态**，与自身对于或者与null、undefiner结果都为ture
