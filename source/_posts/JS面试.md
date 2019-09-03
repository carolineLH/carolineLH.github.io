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

## 4. ===运算符判断相等的流程是怎样的
(1)不同类型值
如果两个值的类型不同，直接返回false。

(2)同一类的原始类型值
同一类型的原始类型的值（数值、字符串、布尔值）比较时，值相同就返回true，值不同就返回false。-0 等于 0

(3)同一类的复合类型值
两个复合类型（对象、数组、函数）的数据比较时，不是比较它们的值是否相等，而是比较它们是否指向同一个对象。

(4)undefined和null
undefined 和 null 与自身严格相等。
```
null === null  //true
undefined === undefined  //true

```

(5) 如果其中有一个是NaN，它们不相等

## 5. ==运算符判断相等的流程是怎样的
1. 如果两个值类型相同，按照===比较方法进行比较
2. 如果类型不同，使用如下规则进行比较
3. 如果其中一个值是 null，另一个是 undefined，它们相等
4. 如果一个值是数字另一个是字符串，将字符串转换为数字进行比较
5. 如果有布尔类型，将true 转换为 1，false 转换为 0，然后用==规则继续比较
6. 如果一个值是对象，另一个是数字或字符串，将对象转换为原始值然后用==规则继续比较
7. 其他所有情况都认为不相等