---
title: 001-两数之和（Two Sum）
date: 2019-12-02 11:25:00
categories: 
  - leetcode
tags: 
  - leetcode
---
## 题目描述

#### 难度：简单
#### 题目地址：https://leetcode.com/problems/two-sum/
#### 涉及知识：数组，哈希表
 <!--more-->
#### 题目描述：
```
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

## 解题

#### 解法一：for()

* 解题代码：
```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
  for (let i = 0; i < nums.length; i++) {
    for (let j = i + 1; j < nums.length; j++) {
      if (nums[j] === target - nums[i]) {
        return [i, j];
      }
    }
  }
};
```

* 执行代码：
```js
var nums = [1, 3, 2, 5, 6], target = 8;
twoSum(nums, target)
```
* 结果：
```
[1,3]
```
* 解题思路：双重for循环，找出两个和为target的数，再返回他们的下标。

#### 解法二：indexOf()
* 解题代码：
```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
  for (let i = 0; i < nums.length; i++) {
   if (nums.indexOf(target-nums[i]) !== -1 && nums.indexOf(target-nums[i]) !== i) {
      return [i, nums.indexOf(target-nums[i])]
   }
  }
};

```
* 执行代码：
```js
var nums = [1, 3, 2, 5, 6], target = 8;
twoSum(nums, target)
```
* 结果：
```js
[1,3]
```
* 解题思路：遍历数组nums中的值，判断数组中是否存在判断条件中的值。如果存在，则返回第一次出现的索引；如果不存在，则返回 -1。

#### 解法三：Map
用这种方法解题，需要你先了解[Map](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map)的知识点及用法。
* 解题代码：
```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
  let map = new Map();
  for (let i = 0; i < nums.length; i++) {
    if (map.has(nums[i])) {
      return [map.get(nums[i]), i];
    } else {
      map.set(target - nums[i], i);
    }
  }
};
```

* 执行代码：
```js
var nums = [1, 3, 2, 5, 6], target = 8;
twoSum(nums, target)
```

* 结果：
```js
[1,3]
```

* 解题思路：我们遍历nums数组，判断nums[i]是否存在于Map对象中。没有的话就把target - nums[i]到Map中。






