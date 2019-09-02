---
title: JS面试
date: 2019-08-31 15:34:50
categories: 
  - js
  - 面试
tags: 
  - js
  - 面试
---
## 1. new Boolean(false)是真还是假？
JavaScript的其他数据类型都可以转化成Boolean类型，但是注意
 <!--more-->
###### 只有这几种类型会转化成false：
* undefined
* null
* 0
* -0
* NaN
* ''(空字符串)

###### 其他的都会转化成true。空对象{}，空数组[]，负数，false的对象包装（值为 false 的 Boolean 对象）等等。
```
if(-1){
  console.log('-1转换为true');
}
if({}){
  console.log('{}转换为true');
}
if([]){
  console.log('[]转换为true');
}
if(new Boolean(false)){
  console.log('new Boolean(false)转换为true');
}

```
new Boolean(false)是布尔值的包装对象    typeof (new Boolean(false))  // 'object'  ,所以 转换为boolean是true，而不是false

new Boolean()出来的一定是个对象，但是Boolean(value)的值是要看所传递的参数value是不是0,-0,null,undefined,'',NaN这六个，只要不是这六个就是true。

## 2. javaScript有哪几种方法定义函数：
1. 函数声明表达式；
```
function calcRectArea(width, height) {
  return width * height;
}

console.log(calcRectArea(5, 6));
```
2. function操作符
```
var getRectArea = function(width, height) {
    return width * height;
}

console.log(getRectArea(3,4));
```
3. Function构造函数：
```
var sum = new Function('a', 'b', 'return a + b');

console.log(sum(2, 6));
```
4. ES6: arrow function:
```
var materials = [
  'Hydrogen',
  'Helium',
  'Lithium',
  'Beryllium'
];

console.log(materials.map(material => material.length));
```

## 3. javascript 有哪些方法定义对象
1. 对象字面量： var obj = {};
2. 构造函数： var obj = new Object();
3. Object.create(): var obj = Object.create(Object.prototype);
