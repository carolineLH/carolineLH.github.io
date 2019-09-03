---
title: -webkit-overflow-scrolling:touch带来的问题
date: 2019-09-03 16:40:41
tags: 
  - 手机兼容
categories: 
  - 手机兼容
---
## 背景
最近开发当中，当页面内容的高度超过屏幕高度的时候，使用overflow-y:scorll属性的时候，发现页面在ios上面很卡顿。所以网上搜了一下，在全局样式文件中加上了:
 <!--more-->
```
 * {
   -webkit-overflow-scrolling: touch;
 }
```
完美解决了ios页面卡顿的问题。

-webkit-overflow-scrolling:touch是何方神圣呢？为什么这么容易就解决了呢？来认识一下-webkit-overflow-scrolling:touch

## -webkit-overflow-scrolling:touch是什么？
MDN上是这样定义的：
* -webkit-overflow-scrolling 属性控制元素在移动设备上是否使用滚动回弹效果.
* auto: 使用普通滚动, 当手指从触摸屏上移开，滚动会立即停止。
* touch: 使用具有回弹效果的滚动, 当手指从触摸屏上移开，内容会继续保持一段时间的滚动效果。继续滚动的速度和持续的时间和滚动手势的强烈程度成正比。同时也会创建一个新的堆栈上下文。

## -webkit-overflow-scrolling:touch产生的问题
以为完美解决了问题，然而，并不是。
#### 1. 新问题出来了：页面滑动偶尔卡住
使用了-webkit-overflow-scrolling:touch的页面，滑动的时候会卡顿，一般都是往底部滑动的时候页面会出现页面卡住的情况。

#### 解决
```
// 修复ios下-webkit-overflow-scrolling:touch样式在顶部和底部滑动偶尔卡住的bug
function isBottom (dom) {
  let lastY = 0 // Needed in order to determine direction of scroll.
  dom.addEventListener('touchstart', (e) => {
    lastY = e.touches[0].clientY
  })
  // 获取滚动元素
  // 先移除监听事件
  dom.removeEventListener('touchmove', (e) => {
    e.preventDefault()
  }, false)
  // 再添加监听事件
  dom.addEventListener('touchmove', (e) => {
    let top = e.touches[0].clientY
    // Determine scroll position and direction.
    let scrollTop = dom.scrollTop
    let scrollHeight = dom.scrollHeight
    let clientHeight = dom.clientHeight
    let direction = (lastY - top) < 0 ? 'up' : 'down'
    // FIX IT!
    if (scrollTop === 0 && direction === 'up') {
      // Prevent scrolling up when already at top as this introduces a freeze.
      e.preventDefault()
      console.log('--到顶部--')
    } else if (scrollTop >= (scrollHeight - clientHeight) && direction === 'down') {
      // Prevent scrolling down when already at bottom as this also introduces a freeze.
      e.preventDefault()
      console.log('--到底部--')
    }
    lastY = top
  })
}

```
简单粗暴可以解决。

#### 2. 第二个新问题：在ios上，页面可以往上往下拖动，若有fixed定位的元素，会被隐藏

就是当页面往上拖动的时候，fixed定位在底部的元素会被隐藏。

原因就是-webkit-overflow-scrolling: touch;导致。

#### 解决
所以就不能用
```
 * {
   -webkit-overflow-scrolling: touch;
 }

```
只能在没有fixed定位的页面使用-webkit-overflow-scrolling: touch;
而且，-webkit-overflow-scrolling: touch;只能在最外层盒子使用才会生效。例如，在vue项目中，只能在id为app的盒子中使用才生效

