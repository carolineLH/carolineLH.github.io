---
title: js数组
date: 2019-08-29 10:22:41
categories: 
  - js
  - 数组
tags: 
  - js
  - 数组
---
## foreach无法终止遍历：
foreach 正常不可以终端遍历过程。可以抛除一个异常终止循环。用some 和 every 可以轻松实现。
 <!--more-->
```
  var arr = [1,2,3,4,5,6,7];

  try {
    arr.forEach((item,index) => {
      console.log(item);
      if(item == 3){
        throw new Error('break');
      }
    });
  } catch (e) {
    // console.log(e);
  } finally {
  
  }

  // some 返回true时终端遍历
  arr.some(item => {
    console.log(item); // 1 2 3
    if(item == 3){
        return true;
      }
  });

  // every 返回false时终端遍历,返回true继续遍历
  arr.every(item => {
    console.log(item); // 1 2 3 4
    if(item == 4){
        return false;
      }else {
        return true;
      }
  });

```

## JS数组遍历：

#### 1. forEach()
有三个参数，第三个参数是数组本身（可选）
循环数组，不会改变元素，不会返回新的数组。只是遍历数组内的每个元素对他进行某些操作。没有返回值。
```
arr.foreach((value,index)=>{})

```
#### 2. map()
有三个参数，第三个参数是数组本身（可选）
返回每个元素返回值的集合.不会改变原数组的值。
遍历数组，对每个元素进行处理，之后返回元素，会返回一个新数组。
 ```
 var arr2 = arr.map((value,index)=>{value++ return value})

 ```

#### 3. some()
遍历数组，在回调函数里进行条件的判断，返回 true 或 false 。当有一个元素满足条件时就停止遍历并返回true；当全部的元素都不满足要求时，返回false
```
var bool = arr.some((value,index)=>{return value>3})

```
#### 4. every()
遍历数组，同上。当有一个元素不满足条件时就停止遍历并返回false；当全部元素都满足时，返回true
```
var bool = arr.every((value,index)=>{return value>3})

```
#### 5. filter()
得到返回值为true的元素的集合   (过滤器)
遍历数组，在回调函数里进行条件判断，当结果为true时，将该元素返回，组成一个新数组。
```
var arr2 = arr.filter((value,index)=>{return value>3})

var arr2 = arr.filter((value,index)=>{ if(value>3){return true}  })

```
#### 6. reduce()
-依次执行ck(prv.next) 
reduce(callback,init)  init为初始值。

当init为空时，回调函数第一次执行的res是数组的第一个元素，value是第二个；第二次执行的时候res是第一次执行返回的内容，value是第三个元素；之后同第二次

当init不为空时，回调函数第一次执行的res是init，value是第一个元素；当第二次执行时res是第一次执行返回的内容，value是第二个元素；第三次的value是第三个元素
```
var result = arr.reduce((res,value)=>{res = res+value return res})

var result = arr.reduce((res,value)->{res = res+value return res},100)

```
#### 7. for...in循环
for...in循环可用于循环对象和数组,推荐用于循环对象,可以用来遍历json
```
let obj = {
    name: '王大锤',
    age: '18',
    weight: '70kg'
}
for(var key in obj) {
    console.log(key);       //  name age weight
    console.log(obj[key]);  //  王大锤 18 70kg
}
let arr = ['a','b','c','d','e'];
for(var key in arr) {
    console.log(key);  //  0 1 2 3 4 返回数组索引
    console.log(arr[key]) //  a b c d e
}

```
#### 8. for...of循环
可循环数组和对象，推荐用于遍历数组。
###### for...of提供了三个新方法：
1. key()是对键名的遍历；
2. value()是对键值的遍历；
3. entries()是对键值对的遍历；
```
let arr = ['科大讯飞', '政法BG', '前端开发'];
for (let item of arr) {  
  console.log(item); //  科大讯飞  政法BG  前端开发
}
// 输出数组索引
for (let item of arr.keys()) {  
  console.log(item);  // 0 1 2
}
// 输出内容和索引
for (let [item, val] of arr.entries()) {  
  console.log(item + ':' + val); //  0:科大讯飞  1：政法BG  2：前端开发
}

```
#### 总结：
forEach、map、filter、reduce、every、some 都会有 break 和 continue 不生效的问题，因为是在function中，但function解决了闭包陷阱的问题；要使用 break、continue 可以使用 for、for...in、for...of、while。

## foreach（）和map（）的区别
1. forEach没有返回值；不会改变原数组的值。
2. map会返回回调函数返回的值的集合,不会改变原数组的值。
3. map的执行速度会比foreach更快
4. forEach()允许callback更改原始数组的元素。map()返回新的数组。

#### 使用场景：
forEach适合于你并不打算改变数据的时候，而只是想用数据做一些事情 -- 比如存入数据库或则打印出来。
```
let arr = ['a', 'b', 'c', 'd'];
arr.forEach((letter) => {
    console.log(letter);
});
// a
// b
// c
// d
```


map()适用于你要改变数据值的时候。不仅仅在于它更快，而且返回一个新的数组。
```
let arr = [1, 2, 3, 4, 5];
let arr2 = arr.map(num => num * 2).filter(num => num > 5);
// arr2 = [6, 8, 10]

```