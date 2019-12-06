---
title: 009-回文数(palindrome-number)
date: 2019-12-05 17:12:55
categories: 
  - leetcode
tags: 
  - leetcode
---

## 题目描述
#### 难度：简单
#### 题目地址：https://leetcode-cn.com/problems/palindrome-number/
 <!--more-->
#### 题目描述：
```
判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

示例 1:

输入: 121
输出: true
示例 2:

输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
示例 3:

输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。

进阶:
你能不将整数转为字符串来解决这个问题吗？
```

## 解题：

#### 解法一：数组
* 解题代码：
```js
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function(x) {
    const arr = String(x).split('');
    for (let i=0; i< arr.length / 2; i++) {
        if (arr[i] !== arr[arr.length - (i + 1)]) {
            return false
        }
    }
    return true
};
```
* 解题思路：首先就是把传入的参数先转化为字符串之后再转化为数组，然后数组的头跟数组尾部一一对应作比较。

