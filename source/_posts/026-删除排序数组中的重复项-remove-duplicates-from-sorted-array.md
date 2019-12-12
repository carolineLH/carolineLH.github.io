---
title: 026-删除排序数组中的重复项(remove-duplicates-from-sorted-array)
date: 2019-12-12 15:06:41
categories: 
  - leetcode
tags: 
  - leetcode
---
## 题目描述
#### 难度：简单
#### 涉及知识：数组、双指针
#### 题目地址：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/
#### 题目内容：
```
给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

示例 1:

给定数组 nums = [1,1,2], 
函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 
你不需要考虑数组中超出新长度后面的元素。

示例 2:

给定 nums = [0,0,1,1,1,2,2,3,3,4],
函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。
你不需要考虑数组中超出新长度后面的元素。

说明:

为什么返回数值是整数，但输出的答案是数组呢?
请注意，输入数组是以“引用”方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中该长度范围内的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

## 解题
#### 解法一-替换
* 解题代码：
```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function(nums) {
    let len = 1;
    for (let i = 1; i < nums.length; i++) {
        if (nums[i] !== nums[i - 1]) {
            nums[len++] = nums[i];
        }
    }
    return len;
};
```
#### 解法二-双指针：
* 解题代码：
```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function (nums) {
    for (let i = 0; i < nums.length; i++) {
    if (nums[i] === nums[i + 1]) {
      nums.splice(i, 1);
      i--;
    }
  }
};
```
* 知识点：[splice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)

* 解题思路：首先，这道题涉及到了**双指针**的概念，看这里，了解[双指针](https://hk029.gitbooks.io/leetbook/twopoint.html)的概念。

#### 解法三-违规操作（es6的知识点Set和...扩展运算符把Set类型变成数组）
* 解题代码：
```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function (nums) {
    var a = [...new Set(nums)];
    for (var i = 0;i < a.length;i++) nums[i] = a[i];
    return a.length;
};
```
* 知识点：`Set`：`Set` 对象允许你存储任何类型的唯一值，无论是原始值或者是对象引用。[set详细介绍](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set)


