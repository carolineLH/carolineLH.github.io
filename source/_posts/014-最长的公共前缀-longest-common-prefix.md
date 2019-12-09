---
title: 014-最长的公共前缀(longest-common-prefix)
date: 2019-12-09 10:50:36
categories: 
  - leetcode
tags: 
  - leetcode
---
## 题目描述
#### 难度：简单
#### 设计知识：字符串、数组
 <!--more-->
#### [题目地址](https://leetcode-cn.com/problems/longest-common-prefix/)
#### 题目描述：
```
编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

示例 1:

输入: ["flower","flow","flight"]
输出: "fl"
示例 2:

输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
说明:

所有输入只包含小写字母 a-z 。
```
## 解题：
#### 解法一-暴力破解
* 解题代码：
```js
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs) {
    // 当数组长度为0时，返回空
    if (!strs.length) {
        return '';
    }
    // 初始化赋值长度最短的那一项为第一项
    let shorStr = strs[0];
    let shortStrLen = strs[0].length;
    let shortStrIndex = 0;
    // 找出长度最短的那一项
    for(let i=0; i<strs.length; i++) {
        if (strs[i].length < shortStrLen) {
            shorStr = strs[i];
            shortStrIndex = i;
        }
    }
    // 将找出的最短的那一项中的每一个字符跟数组中每一项的字符进行比较，找出公共前缀
    let result = []
    for (let i=0; i< shortStrLen; i++) {
        for (let j=0; j < strs.length; j++) {
            if (shorStr[i] !== strs[j][i]) {
                return result.join('');
            }
            if (j === strs.length -1) {
                result[i] = strs[j][i];
            }
        }
    }
    return result.join('');
};
```

* 知识点：[join()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/join)

#### 解法二-水平扫描
* 解题代码：
```js
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs) {
    // 判断strs数组长度，如果只有一项结果就返回第一项，如果数组长度为0，结果就返回空
    if (strs.length < 2) {
        return !strs.length ? '' : strs[0];
    }
    // 将初始化数组的第一项为依据，跟数组中每一项的每一个字符相比较
    let result = strs[0];
    for (let i=0; i<strs[0].length; i++) {
        for (let j = 1; j<strs.length; j++) {
            if (strs[0][i] !== strs[j][i]) {
                return result.substring(0, i)
            }
        }
    }
    return result;
};
```
* 知识点：[substring()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/substring)

#### 解法三-水平扫描（reduce方法）
* 解题代码：
```js
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs) {
    // 判断strs数组长度，如果只有一项结果就返回第一项，如果数组长度为0，结果就返回空
   if (strs.length < 2) {
       return !strs.length ? '' : strs[0]
   }
    // 将初始化数组的第一项为依据，跟数组中每一项的每一个字符相比较
    return strs.reduce((pre, next) => {
        let i = 0;
        while (pre[i] && next[i] && pre[i] === next[i]) {
            i++;
        }
        return pre.slice(0, i);
    }) 
};
```
* 知识点：[reduce()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce),[slice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/slice)


