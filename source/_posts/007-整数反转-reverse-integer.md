---
title: 007-整数反转(reverse-integer)
date: 2019-12-05 15:34:39
categories: 
  - leetcode
tags: 
  - leetcode
---
## 题目描述

#### 难度：简单
#### 涉及知识：数组、字符串
#### 题目地址：https://leetcode-cn.com/problems/reverse-integer/
 <!--more-->
#### 题目描述：
```
给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

示例 1:

输入: 123
输出: 321
 示例 2:

输入: -123
输出: -321
示例 3:

输入: 120
输出: 21

注意：假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。
```

## 解题：

#### 解法一：转字符串

* 解题代码：
```js
/**
 * @param {number} x
 * @return {number}
 */
var reverse = function(x) {
    // 转数组
    let toStringArr = String(Math.abs(x)).split('');
    // 转字符串
    let result = '';
    for(let i=0; i<toStringArr.length;) {
        result += toStringArr.pop();
    }
    // 转为数字
    result = x > 0 ? Number(result) : - Number(result);
    // 超 [-Math.pow(2, 31), Math.pow(2, 31) - 1] 判断
    if (result > Math.pow(2, 31) - 1
    || result < - Math.pow(2, 31)) {
        result = 0;
    }
    
    return result;
};
```
或者：
```js
/**
 * @param {number} x
 * @return {number}
 */
var reverse = function(x) {
    // 转数组
    let toStringArr = String(Math.abs(x)).split('');
    let arr = []
    for(var i=0; i<toStringArr.length; i++) {
        arr.unshift(toStringArr[i]);
    }
    // 转字符串
    let result = arr.join('')
    // 转为数字
    result = x > 0 ? Number(result) : - Number(result);
    // 超 [-Math.pow(2, 31), Math.pow(2, 31) - 1] 判断
    if (result > Math.pow(2, 31) - 1
    || result < - Math.pow(2, 31)) {
        result = 0;
    }
    
    return result;
};
```
#### 知识点：
[Math](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math)
[pop()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)
[String](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)
[Number](https://github.com/LiangJunrong/document-library/blob/master/JavaScript-library/JavaScript/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Number/README.md)


