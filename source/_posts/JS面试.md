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

## 2. 类的优先级：
* 内联样式，优先级1000

* id选择器，优先级100

* 类和伪类，优先级10

* 元素选择器，优先级1

* 通配选择器，优先级0

* 继承的样式没有优先级

* 当选择器包含多种选择器时，需要将多种选择器的优先级相加然后进行比较。但是注意，选择器的优先级计算不会超过他的最大数量级，如果选择器的优先级一样，则使用靠后的样式。

* 并集选择器的优先级时单独计算。

* 可以在样式的最后添加一个!important，则此时该样式会获得一个最高的一个优先级，将会超过所有的样式甚至超过内联样式，所以在开发中尽量避免使用。

