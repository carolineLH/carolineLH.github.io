---
title: 013-罗马数字转整数（roman-to-integer）
date: 2019-12-06 14:44:32
categories: 
  - leetcode
tags: 
  - leetcode
---
## 题目描述
#### 难度：简单
#### 题目地址：https://leetcode-cn.com/problems/roman-to-integer/
 <!--more-->
#### 题目描述：
罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。
```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```
例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：
* I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
* X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
* C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。

给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

示例 1:
```
输入: "III"
输出: 3
```

示例 2:
```
输入: "IV"
输出: 4
```

示例 3:
```
输入: "IX"
输出: 9
```

示例 4:
```
输入: "LVIII"
输出: 58
解释: L = 50, V= 5, III = 3.
```

示例 5:
```
输入: "MCMXCIV"
输出: 1994
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```

## 解题：
#### 解法一：for循环+if判断

* 解题代码：
```js
/**
 * @param {string} s
 * @return {number}
 */
var romanToInt = function(s) {
    let arr = s.split('');
    let result = 0;
    for (let i = 0; i < arr.length;) {
        if (arr[i] === 'I' && arr[i+1] === 'V') {
            result += 4;
            i = i + 2;
        } else if(arr[i] === 'I' && arr[i+1] === 'X') {
            result += 9;
            i = i + 2;
        } else if(arr[i] === 'X' && arr[i+1] === 'L') {
            result += 40;
            i = i + 2;
        } else if(arr[i] === 'X' && arr[i+1] === 'C') {
            result += 90;
            i = i + 2;
        } else if(arr[i] === 'C' && arr[i+1] === 'D') {
            result += 400
            i = i + 2;
        } else if(arr[i] === 'C' && arr[i+1] === 'M') {
            result += 900;
            i = i + 2;
        } else if (arr[i] === 'I') {
            result += 1;
            i = i + 1;
        } else if (arr[i] === 'V') {
            result += 5;
            i = i + 1;
        } else if (arr[i] === 'X') {
            result += 10;
            i = i + 1;
        } else if (arr[i] === 'L') {
            result += 50;
            i = i + 1;
        } else if (arr[i] === 'C') {
            result += 100;
            i = i + 1;
        } else if (arr[i] === 'D') {
            result += 500;
            i = i + 1;
        } else if (arr[i] === 'M') {
            result += 1000;
            i = i + 1;
        }
    }
    return result
};
```

* 解题思路：把输入的字符串先转化为数组，然后再循环遍历数组，根据各种情况一步一步判断。

#### 解法二：Map

* 解题代码：
```js
/**
 * @param {string} s
 * @return {number}
 */
var romanToInt = function(s) {
    let result = 0;
    let map = new Map();
    map.set('I', 1);
    map.set('V', 5);
    map.set('X', 10);
    map.set('L', 50);
    map.set('C', 100);
    map.set('D', 500);
    map.set('M', 1000)
    // js中，字符串也是可以遍历的
    for (let i = 0; i < s.length; i++) {
        if (s[i] + s[i+1] === 'IV') {
            result += 4;
            i = i + 1;
        } else if(s[i] + s[i+1] === 'IX') {
            result += 9;
            i = i + 1;
        } else if(s[i] + s[i+1] === 'XL') {
            result += 40;
            i = i + 1;
        } else if(s[i] + s[i+1] === 'XC') {
            result += 90;
            i = i + 1;
        } else if(s[i] + s[i+1] === 'CD') {
            result += 400
            i = i + 1;
        } else if(s[i] + s[i+1] === 'CM') {
            result += 900;
            i = i + 1;
        } else {
            result += map.get(s[i]);
        }
    }
    return result
};
```

* 解题思路：先创建一个map对象，根据map对象的方法及属性，将正常情况存在map里面。再遍历s参数，利用for循环判断出特殊情况和正常情况，针对不同的情况做处理。

