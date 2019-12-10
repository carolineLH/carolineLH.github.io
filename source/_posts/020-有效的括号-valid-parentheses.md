---
title: 020-有效的括号(valid-parentheses)
date: 2019-12-10 11:47:58
categories: 
  - leetcode
tags: 
  - leetcode
---
## 题目描述
#### 难度：简单
#### 设计知识：栈、字符串
 <!--more-->
#### 题目地址：https://leetcode-cn.com/problems/valid-parentheses/
#### 题目内容：
```
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

示例 1:
输入: "()"
输出: true

示例 2:
输入: "()[]{}"
输出: true

示例 3:
输入: "(]"
输出: false

示例 4:
输入: "([)]"
输出: false

示例 5:
输入: "{[]}"
输出: true
```
## 解题：
* 解题代码：
```js
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
    // 定义一个对象，作为匹配标准
    let judge = {
        '[': ']',
        '(': ')',
        '{': '}',
    }
    // 将输入的字符串s分割成字符串数组
    let parameter = s.split('');
    // 定义一个空数组来存储需要待匹配的字符
    let stack = [];
    for (let i = 0; i < parameter.length; i++) {
        // 如果存储在待匹配数组stuck中的字符能跟当前项匹配，说明匹配成功，则把stuck中的这个字符串从stuck中删除
        if (judge[stack[stack.length - 1]] === parameter[i]) {
            stack.pop();
        } else {
            // 当当前项不匹配则把当前项存储到stuck中，待匹配
            stack.push(parameter[i]);
        }
    }
    // 只有当stuck中没有待匹配项时，说明全部匹配成功，返回真
    if (stack.length == 0) {
        return true;
    }
    return false;
};
```
* 知识点：[split()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split)
[pop()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)
[push()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/push)
