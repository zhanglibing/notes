---
layout: post
title:  "javascript 模块"
tags:
categories:
---

## javascript 易错总结

```javascript
console.log(typeof undefined);              // undefined
console.log(typeof null)                   // object   （null是一个只有一个值的特殊类型。表示一个空对象引用）
console.log(typeof [1,2,3,4])              // 返回 object  （ 数组是一种特殊的对象类型）
console.log(typeof {name:'John', age:34})  // 返回 object

//用 constructor 属性来查看对象是否为数组 (包含字符串 "Array"):
function isArray(myArray) {
    return myArray.constructor.toString().indexOf("Array") > -1;
}
//同上
function isDate(myDate) {
    return myDate.constructor.toString().indexOf("Date") > -1;
}
```
`parseFloat()`解析一个字符串，并返回一个浮点数。  
`parseInt()` 解析一个字符串，并返回一个整数
```javascript
parseFloat('3.25') // 3.25
```

`Number`转化
```javascript
Number(false)     // 返回 0
Number(true)      // 返回 1
```


