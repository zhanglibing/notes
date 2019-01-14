---
layout: post
title:  "javascript 模块"
tags:
categories:
---

## javascript 易错总结

### typeof
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
### parse
`parseFloat()`解析一个字符串，并返回一个浮点数。  
`parseInt()` 解析一个字符串，并返回一个整数
```javascript
parseFloat('3.25') // 3.25
```
### Number
`Number`转化
```javascript
Number(false)     // 返回 0
Number(true)      // 返回 1
```

### Boolean
`Boolean`转化
```javascript
Boolean(0); //false
Boolean(1); //true
```

## 函数

###立即执行函数
- 一般用户初始化
- 自动执行，执行完成后立即释放
- IIFE (immediately invoked function expression)
- 闭包会导致原油作用域链不释放，造成内存泄漏
```javascript
(function(){})();
(function(){}()); //w3c建议
//()执行函数
//传参  有返回值
var num=(function(a,b){
    return a+b;
}(a,b))

//一定是表达式才会被执行符号执行
function test1(){
    console.log(1)
}(); //语法错误

var test=function test2(){
    console.log(1)
}(); //1


var num=(1,2) //2  返回，后面的
/*
*面试题
* */
function test(a){
    
}(6)  //不报错  （）里边有传参浏览器会解析成运算符  只是test函数没执行
function test(a){}() //报错


var a=10;
if(function b(){}){
    a+=typeof (b);
}
console.log(a) //10undefined  
//(function b(){}) 表达式里边忽略函数名  b已经不存在


//简单闭包
function b(){
    var sum=0;
    function a(){
        sum++;
        console.log(sum)
    }
    return a;
}
var test=b();
test() //1
test() //2

```

###  逗号‘,’

```javascript
//逗号运算 取后面值
var f=(function(){return '2'; },function(){return 1; })();
typeof f; //1 number

```

### 闭包图解
- 函数a执行完后函数a指向的AO被截断 释放 但是b函数的原型还指向a函数的AO
![Image-text](https://raw.githubusercontent.com/zhanglibing/img-folder/master/js/bibao.png)
```javascript
function a(){
    function b(){
        var bbb=234;
        console.log(aaa)
    }
    var aaa=123;
    return b;
}
var glob=100;
var demo=a()
demo() //123


//经典闭包
function test(){
    var arr=[];
    for(var i=0;i<10;i++){
        (function(j){
            arr[j]=function(){
                console.log(j)
            }
        }(i))
    }
    return arr;
}

var myarr=test();
for(var j=0;j<10;j++){
    myarr[j]() //0 1 2 3 4 5 ...9
}

```

### date日期
-  构造函数的不同形式
```javascript
 new Date() ;
 new Date(milliseconds) ; 
 new Date(datestring) ; //'2018 2 20'  、'2018/2/20' 、'2018-2-20' ...任何时间字符串 
 new Date(year,month,date[,hour,minute,second,millisecond]);
```

