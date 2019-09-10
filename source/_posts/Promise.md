---
title: Promise
date: 2019-09-09 22:04:30
categories: 
  - ES6
  - 面试
tags: 
  - ES6
  - 面试
---

相信大家一定很熟悉Promise这个单词了吧，就算你不是很理解它，应该也挺嫌弃它的吧。毕竟面试官都喜欢问：你了解Promise吗？能不能说一下Promise.....

那我们就来彻底的认识一下Promise吧。
 <!--more-->
## 背景
在Promise之前，在js中的异步编程都是采用回调函数和事件的方式。比如当我们要处理一个异步网络请求，大概是这样处理：
```
// 请求 代表一个异步网络调用
// 请求结果 代表网络请求的相应。

请求1(function(请求结果1){
    处理请求结果1
})
```
这样处理是没毛病的。
但是，现在需求变了，我们需要根据第一次网络请求的结果，再去执行第二个网络请求，实现代码如下：
```
请求1(function(请求结果1){
    请求2(function(请求结果2){
        处理请求结果2
    })
})
```
目前这种情况也是可以接受的，但是需求越来越复杂的时候，很容易出现callback hell(回调地狱)，使得代码很难被理解和维护。
就会出现下面的场景：
```
请求1(function(请求结果1){
    请求2(function(请求结果2){
        请求3(function(请求结果3){
            请求4(function(请求结果4){
                请求5(function(请求结果5){
                    请求6(function(请求结果3){
                        ...
                    })
                })
            })
        })
    })
})

```
是不是很吓人。

###### 回调地狱带来的负面作用有以下几点：
* 代码臃肿
* 可读性差
* 耦合度过高，可维护性差
* 代码复用性差
* 容易滋生bug
* 只能在回调里处理异常

为了改善回调地狱的问题，Promise就出现了。

## Promise的含义

promise是异步编程的一种解决办法，比传统的回调函数和事件更合理更强大。它由社区最早提出和实现，ES6将其写进语言标准，统一了用法，原声提供了promise对象。

所谓promise，简单说是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果，从语法上说，promise是一个对象，从它可以获取异步操作的消息，promise提供了统一的API，各种异步操作都可以用同样的方法进行处理。

#### Promise理解
其实通俗一点来讲，promise就是可以让我们用同步的思想来实现异步编程。

对于回调地狱一层层嵌套的回调函数，我们希望的应该是这种写代码的方式吧：
```
let 请求结果1 = 请求1();
let 请求结果2 = 请求2(请求结果1); 
let 请求结果3 = 请求3(请求结果2); 
let 请求结果4 = 请求2(请求结果3); 
let 请求结果5 = 请求3(请求结果4); 
```
当然这是我们希望的，通过promise来编写就是这样的：
```
new Promise(请求1)
    .then(请求2(请求结果1))
    .then(请求3(请求结果2))
    .then(请求4(请求结果3))
    .then(请求5(请求结果4))
    .catch(处理异常(异常信息))
```
是不是符合我们的预期。
这些都只是伪代码，为了让我们更好的理解写的。

## Promise对象的特点：

1.对象的状态不受外界影响，promise对象代表一个异步操作，有三种状态，pending（进行中）、fulfilled（已成功）、rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态，这也是promise这个名字的由来“承诺”；

2.一旦状态改变就不会再变，任何时候都可以得到这个结果，promise对象的状态改变，只有两种可能：从pending变为fulfilled，从pending变为rejected。这时就称为resolved（已定型）。如果改变已经发生了，你再对promise对象添加回调函数，也会立即得到这个结果，这与事件（event）完全不同，事件的特点是：如果你错过了它，再去监听是得不到结果的。

3.Promise也有一些缺点。首先，无法取消Promise，一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。第三，当处于pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

## promise用法：
promise对象是一个构造函数，用来生成promise实例;
创建一个promise对象实例
```
var promise = new Promise( function( resolve, reject) {
       //some code 
      if(//异步操作成功){
        resolve(value);
         }else{
         reject(error);
         }
});

```
Promise构造函数接收一个函数作为参数，该函数的两个参数分别是resolve和reject，他们是两个函数，由Javascript引擎提供，不用自己部署。

resolve函数的作用是，将promise对象的状态从“pending”变为‘’resolved‘’，在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；

reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

promise实例生成以后，可以用then方法分别指定resolved状态和rejected状态的回调函数
```
promise.then(
    function(value){
   //success
   },
    function(error){
   //failure
 });

```
then方法可以接受连个回调函数作为参数，第一个回调函数是promise对象的状态变为resolved时调用，第二个回调函数是promise对象的状态变为rejected时调用，其中，第二个函数是可选的，不一定要提供，这两个函数都接受promise对象传出的值作为参数；

promise对象的简单例子
```
function timeOut (ms) {
      return new Promise(function(resolve,reject) {
                 return    setTimeout(resolve,ms,"done");
       })
}
timeOut(3000).then( function(value){
console.log(value);
})

```

timeOut方法返回一个promise实例，表示一段时间以后才会发生的结果，过了指定的时间（ms）以后，promise实例的状态变为resolved，就会触发then方法绑定的回调函数

Promise新建后就会立即执行
```
let promise = new Promise(function(resolve, reject) {
  console.log('Promise');
  resolve();
});

promise.then(function() {
  console.log('resolved.');
});

console.log('Hi!');

// Promise
// Hi!
// resolved

```

Promise 新建后立即执行，所以首先输出的是Promise。然后，then方法指定的回调函数，将在当前脚本所有同步任务执行完才会执行，所以resolved最后输出。

## Promsie 与事件循环
Promise在初始化时，传入的函数是同步执行的，然后注册then回调。注册完之后，继续往下执行同步代码，在这之前，then中回调不会执行。同步代码块执行完毕后，才会在事件循环中检测是否有可用的 promise 回调，如果有，那么执行，如果没有，继续下一个事件循环。

关于 Promise 在事件循环中还有一个 微任务的概念（microtask），感兴趣的话可以看这篇[关于nodejs 时间循环](https://juejin.im/post/5af1413ef265da0b851cce80)的文章 剖析nodejs的事件循环，虽然和浏览器端有些不同，但是Promise 微任务的执行时机相差不大。

## API
Promise 的常用 API 如下：

#### Promise.resolve(value)
类方法，该方法返回一个以 value 值解析后的 Promise 对象
1、如果这个值是个 thenable（即带有 then 方法），返回的 Promise 对象会“跟随”这个 thenable 的对象，采用它的最终状态（指 resolved/rejected/pending/settled）
2、如果传入的 value 本身就是 Promise 对象，则该对象作为 Promise.resolve 方法的返回值返回。
3、其他情况以该值为成功状态返回一个 Promise 对象。

```
//如果传入的 value 本身就是 Promise 对象，则该对象作为 Promise.resolve 方法的返回值返回。  
function fn(resolve){
    setTimeout(function(){
        resolve(123);
    },3000);
}
let p0 = new Promise(fn);
let p1 = Promise.resolve(p0);
// 返回为true，返回的 Promise 即是 入参的 Promise 对象。
console.log(p0 === p1);
```
传入 thenable 对象，返回 Promise 对象跟随 thenable 对象的最终状态。
jQuery.ajax，它的返回值就是 thenable 对象
```
//如果传入的 value 本身就是 thenable 对象，返回的 promise 对象会跟随 thenable 对象的状态。
let promise = Promise.resolve($.ajax('/test/test.json'));// => promise对象
promise.then(function(value){
   console.log(value);
});

```

#### Promise.reject

类方法，且与 resolve 唯一的不同是，返回的 promise 对象的状态为 rejected。

#### Promise.prototype.then
Promise实例具有then方法，也就是说,then方法定义在原型对象Promise.prototype上的。他的作用是为Promise实例添加状态改变时的回调函数。前面说过，then方法的第一个参数是resolved状态的回调函数 ,第二个参数是rejected状态的回调函数。
then方法返回的是一个新的Promise实例（不是原来那个Promise实例），因此可以采用链式写法，即then方法后面再调用另一个then方法
```
getJSON("posts.json").then(function (json){
           return post.json;
}).then(function(post){
           //...
});

```

依次指定了两个回调函数，第一个回到函数完成以后会将返回结果作为参数，传入第二个回调函数。

#### Promise.prototype.catch
实例方法，捕获异常，函数形式：fn(err){}, err 是 catch 注册 之前的回调抛出的异常信息。

#### Promise.race
类方法，多个 Promise 任务同时执行，返回最先执行结束的 Promise 任务的结果，不管这个 Promise 结果是成功还是失败。 。

#### Promise.all
类方法，多个 Promise 任务同时执行。
如果全部成功执行，则以数组的方式返回所有 Promise 任务的执行结果。 如果有一个 Promise 任务 rejected，则只返回 rejected 任务的结果。

## ES6 实现自己的 Promise(手写promise)
可以参考这篇[文章](https://github.com/Jocs/jocs.github.io/issues/7)

## Promise的使用总结

* 首先初始化一个 Promise 对象，可以通过两种方式创建， 这两种方式都会返回一个 Promise 对象。
  1. new Promise(fn)
  2. Promise.resolve(fn)
* 然后调用上一步返回的 promise 对象的 then 方法，注册回调函数。
  1. then 中的回调函数可以有一个参数，也可以不带参数。如果 then 中的回调函数依赖上一步的返回结果，那么要带上参数。比如
  ```
  new Promise(fn)
    .then(fn1(value）{
        //处理value
    })
  ```
* 最后注册 catch 异常处理函数，处理前面回调中可能抛出的异常.

#### 例子：
比如你要买菜做饭送给孩子。
所以你需要做以下几件事情：
1. 你先去超市买菜。
2. 用超市买回来的菜做饭。
3. 将做好的饭菜送到学校

```
// 先去超市买菜
new Promise(买菜)
//用买好的菜做饭
.then((买好的菜)=>{
    return new Promise(做饭);
})
//把做好的饭送到学校
.then((做好的饭)=>{
    return new Promise(送饭);
})

```

###### 请一定要谨记：如果我们的后续任务是异步任务的话，必须return 一个 新的 promise 对象。如果后续任务是同步任务，只需 return 一个结果即可。

## Promise 的升级
ES6 出现了 generator 以及 async/await 语法，使异步处理更加接近同步代码写法，可读性更好，同时异常捕获和同步代码的书写趋于一致。
上面的列子可以写成这样：
```
(async ()=>{
    let 蔬菜 = await 买菜();
    let 饭菜 = await 做饭(蔬菜);
    let 送饭结果 = await 送饭(饭菜);
})();

```
需要记住的是，async/await也是基于 Promise 实现的，所以，我们仍然有必要深入理解 Promise 的用法。









