---
title: 手写promise
date: 2020-07-17 09:08:45
categories:
  - js
tags:
  - js
---

# 手写 Promise

#### 开始前的思考：

1. Promise 有三种状态: pending resolve reject
2. Promise 是 thenable 的,并且 then 之后可能还是 Promise 对象.
3. 链式调用回调函数的返回可以是基本类型，对象，函数甚至是一个 promise
 <!--more-->

#### 代码实现：

```js
function Promise(executor) {
  var _this = this
  // promise的状态
  this.state = 'pending'
  // 用来保存resolve的状态下得到的值
  this.value = undefined
  // 用来保存reject的状态下得到的值
  this.reason = undefined
  // 使用订阅者模式来解决异步任务的执行
  this.onFulfilledFunc = []
  this.onRejectedFunc = []

  try {
    executor(resolve, reject)
  } catch (e) {
    reject(e.message)
  }

  function resolve(value) {
    if (_this.state === 'pending') {
      _this.value = value
      _this.state = 'resolved'
      _this.onFulfilledFunc.forEach(f => f(value))
    }
  }

  function reject(reason) {
    if (_this.state === 'pending') {
      _this.reason = reason
      _this.state = 'rejected'
      _this.onRejectedFunc.forEach(f => f(reason))
    }
  }
}

/**
 * 状态更新后执行
 * @param {Function} onFulfilled 成功态回调
 * @param {Function} onRejected 失败态回调
 */
Promise.prototype.then = function(onFulfilled, onRejected) {
  // 解决值的传递，如果没给任何参数就使用默认回调
  onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : val => val
  onRejected =
    typeof onRejected === 'function'
      ? onRejected
      : e => {
          throw e
        }

  var promise2 = new Promise((resolve, reject) => {
    if (this.state === 'pending') {
      if (typeof onFulfilled === 'function') {
        this.onFulfilledFunc.push(value => {
          // 使用setTimeout防止阻塞后面的代码执行
          setTimeout(() => {
            try {
              let x = onFulfilled(value) //要保证onFulfilled是异步执行
              resolvePromise(promise2, x, resolve, reject)
            } catch (error) {
              reject(error)
            }
          })
        })
      }

      if (typeof onRejected === 'function') {
        this.onRejectedFunc.push(reason => {
          setTimeout(() => {
            try {
              let x = onRejected(reason)
              resolvePromise(promise2, x, resolve, reject)
            } catch (error) {
              reject(error)
            }
          })
        })
      }
    }

    if (this.state === 'resolved') {
      if (typeof onFulfilled === 'function') {
        setTimeout(() => {
          try {
            let x = onFulfilled(this.value)
            resolvePromise(promise2, x, resolve, reject)
          } catch (error) {
            reject(error)
          }
        })
      }
    }

    if (this.state === 'rejected') {
      if (typeof onRejected === 'function') {
        setTimeout(() => {
          try {
            let x = onRejected(this.reason)
            resolvePromise(promise2, x, resolve, reject)
          } catch (error) {
            reject(error)
          }
        })
      }
    }
  })
  return promise2
}

/**
 * 根据上一个then的结果解析Promise
 * @param {Object} promise2 新Promise对象
 * @param {Object} x 上一个then的返回值
 * @param {Function} resovle 新Promise的成功态回调
 * @param {Function} reject 新Promise的失败态回调
 */
function resolvePromise(promise2, x, resolve, reject) {
  if (promise2 === x) {
    reject(new TypeError('Promise发生了循环引用'))
  }

  let called = false // 调用标记,防止成功和失败都调用
  if (x !== null && (typeof x === 'object' || typeof x === 'function')) {
    // 是对象或函数
    try {
      let then = x.then
      if (typeof then === 'function') {
        // 如果是Promise就执行它,继续递归
        then.call(
          x,
          y => {
            if (called) return
            called = true // 调用过了,将标记更新
            resolvePromise(promise2, y, resolve, reject)
          },
          r => {
            if (called) return
            called = true // 调用过了,将标记更新
            reject(r)
          }
        )
      } else {
        resolve(x)
      }
    } catch (error) {
      if (called) return
      called = true // 调用过了,将标记更新
      reject(error)
    }
  } else {
    // 是普通值
    resolve(x)
  }
}

module.exports = Promise
```
