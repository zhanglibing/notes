---
layout: post
title:  "javascript 模块"
tags:
categories:
---

## javascript 易错总结

### console.log([]==false) true[]
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

###  instanceof
- 检测是否为数组
`[] instanceof Array`  返回true


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

###  数组常用方法
- join()
- push() 返回修改后数组的长度
- pop()  数组末尾移除最后一项，减少数组的 length 值，然后返回移除的项
- shift() 删除原数组第一项，并返回删除元素的值；如果数组为空则返回undefined 
- unshift() 将参数添加到原数组开头，并返回数组的长度
- sort()  sort()方法会调用每个数组项的 toString()转型方法，然后比较得到的字符串，以确定如何排序。即使数组中的每一项都是数值， sort()方法比较的也是字符串
- reverse()
- concat()
- slice()
- splice()
- indexOf()和 lastIndexOf() （ES5新增）
- forEach() （ES5新增）
- map() （ES5新增）
- filter() （ES5新增）
- every() （ES5新增）
- some() （ES5新增）
- reduce()和 reduceRight() （ES5新增）
```javascript
function compare(value1, value2) {
    if (value1 < value2) {
        return -1;
    } else if (value1 > value2) {
        return 1;
    } else {
        return 0;
    }
}
arr2 = [13, 24, 51, 3];
console.log(arr2.sort(compare)); // [3, 13, 24, 51]

```

### 数组深拷贝的方法
- 仅适用于对不包含引用对象的一维数组的深拷贝。对于数组内部存在对象和数组，
当改变对象属性和内部数组的元素后，深拷贝的数组同样也发生了改变。
- `当数组里边有对象时，修改拷贝的数组对象值原数组也会被改变`
- `JSON.parse(JSON.stringify(obj|arr))`; //这个可以实现深拷贝数组对象（暴力深拷贝） IE6、IE7不支持

```javascript
//1循环实现数组的深拷贝
//2.slice
var arr = [1,2,3,4,5]
var newArr = arr.slice(0);

//3.concat
var arr = [1,2,3,4,5]
var newArr = arr.concat()

//4.数组解构
var arr = [1,2,3,4,5]
var newArr = [...arr]

//5.
JSON.parse(JSON.stringify(obj|arr))

/*注意*/
var a=[{a:456}];
var [...b]=a;
b[0].a=999;
console.log(a)  //[{a:999}]

```

### 数组去重方法
- for循环
- `Array.from(new Set([1, 1, 1, 2, 3, 2, 4]))`
- ES6中新增了Set数据结构，类似于数组，但是 它的成员都是唯一的
- Array.from可以把类似数组的对象转换为数组

### 对象的深拷贝

```javascript
// for循环
var obj = {
  name: 'FungLeo',
  sex: 'man',
  old: '18'
}
var obj2 = copyObj(obj)
function copyObj(obj) {
  let res = {}
  for (var key in obj) {
    res[key] = obj[key]
  }
  return res
}


//2.
var obj = {
  name: 'FungLeo',
  sex: 'man',
  old: '18'
}
var obj2 = JSON.parse(JSON.stringify(obj))

//3.
var obj = {
  name: 'FungLeo',
  sex: 'man',
  old: '18'
}
var { ...obj2 } = obj;

```

