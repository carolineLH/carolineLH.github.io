---
title: '从promise、process.nextTick、setTimeout出发，谈谈Event Loop中的Job queue '
date: 2019-11-06 10:25:08
categories: 
  - js
  - 面试
tags: 
  - js
  - 面试
---
## 背景
之前有写过一篇文章来总结[事件循环](https://juejin.im/post/5aacd1766fb9a028cb2d6766)，最近重新学习了下，感觉需要重新总结一下。
 <!--more-->
之前文章中也说过，event loop就是指主线程从“任务队列”中循环读取任务，例如：
```
setTimeout(function(){console.log('a')},0);

console.log('b')
// 输出 b a
```

在这个例子中，我们明白首先执行主线程中的同步任务，档主线程任务执行完毕之后，再从event loop中读取任务，因此先输出b再输出a

event loop读取任务的先后顺序，取决于任务队列（Job queue）中对于不同任务读取规则的限定
```
setTimeout(function () {
  console.log(3);
}, 0);

Promise.resolve().then(function () {
  console.log(2);
});
console.log(1);
//输出为  1  2 3
```
先输出1，没有问题，因为是同步任务在主线程中优先执行，这里的问题是setTimeout和Promise.then任务的执行优先级是如何定义的。

## Job queue中的执行顺序

在Job queue中的队列分为两种类型：macro-task和microTask。我们举例来看执行顺序的规定,我们设

macro-task队列包含任务: a1, a2 , a3
micro-task队列包含任务: b1, b2 , b3

执行顺序为，首先执行marco-task队列开头的任务，也就是 a1 任务，执行完毕后，在执行micro-task队列里的所有任务，也就是依次执行b1, b2 , b3，执行完后清空micro-task中的任务，接着执行marco-task中的第二个任务，依次循环。

了解完了macro-task和micro-task两种队列的执行顺序之后，我们接着来看，真实场景下这两种类型的队列里真正包含的任务（我们以node V8引擎为例），在node V8中，这两种类型的真实任务顺序如下所示：

macro-task队列真实包含任务：

**script(主程序代码),setTimeout, setInterval, setImmediate, I/O, UI rendering**

micro-task队列真实包含任务:

**process.nextTick, Promises, Object.observe, MutationObserver**

由此我们得到的执行顺序应该为：

**script(主程序代码)—>process.nextTick—>Promises...——>setTimeout——>setInterval——>setImmediate——> I/O——>UI rendering**

在ES6中macro-task队列又称为ScriptJobs，而micro-task又称PromiseJobs

## 实例

#### 1.setTimeout和promise
```
setTimeout(function () {
  console.log(3);
}, 0);

Promise.resolve().then(function () {
  console.log(2);
});

console.log(1);
```
这里遵循的顺序为：

**script(主程序代码)——>promise——>setTimeout**
对应的输出依次为：1 ——>2————>3

#### 2.process.nextTick和promise、setTimeout

```
setTimeout(function(){console.log(1)},0);

new Promise(function(resolve,reject){
   console.log(2);
   resolve();
}).then(function(){console.log(3)
}).then(function(){console.log(4)});

process.nextTick(function(){console.log(5)});

console.log(6);
//输出2,6,5,3,4,1
```

**这里要注意的一点在定义promise的时候，promise构造部分是同步执行的**

首先分析Job queue的执行顺序：

**script(主程序代码)——>process.nextTick——>promise——>setTimeout**

#### 3.更复杂的例子
```
setTimeout(function(){console.log(1)},0);

new Promise(function(resolve,reject){
   console.log(2);
   setTimeout(function(){resolve()},0)
}).then(function(){console.log(3)
}).then(function(){console.log(4)});

process.nextTick(function(){console.log(5)});

console.log(6);

//输出的是  2 6 5 1 3 4
```
这种情况跟我们（2）中的例子，区别在于promise的构造中，没有同步的resolve，因此promise.then在当前的执行队列中是不存在的，只有promise从pending转移到resolve，才会有then方法，而这个resolve是在一个setTimout时间中完成的，因此3,4最后输出。