---
title: IOS 安卓兼容性问题
date: 2019-08-01 11:07:38
tags: 
  - 手机兼容
categories: 
  - 手机兼容
---
## 1. 安卓手机上面软键盘弹起的时候挡住输入框的兼容性问题
 安卓手机跟苹果手机 获取的body 高度不一样 ，所以导致了 当软键盘出现的时候 安卓手机上面页面并没有向上滚动。
 解决办法：
 ```
       window.addEventListener('resize', function () {
        if (document.activeElement.tagName === 'INPUT' || document.activeElement.tagName === 'TEXTAREA') {
          window.setTimeout(function () {
            document.activeElement.scrollIntoViewIfNeeded()
          }, 10)
        }
      })
 ```
 <!--more-->
## 2. ios上下滑动卡顿
因为现在移动设备真是贼辣多，手机屏幕的高度，宽度各式各样。
所以我们有一些页面高度不够长，在iPhone8x、三星长屏手机等页面显示不全，所以我开发的时候，有时候会给html和body都设置上height：100%，所以在ios手机上这个页面滑动的时候就会卡顿了。
正确的做法应该是这样的：给html设置一个min-height:100%;给body设置一个:
```
max-width: 750px;
margin: 0 auto;
```
所以，错误就出在了
```
html,body{

  height: 100%;

}
```

删除上述代码即可。

如果还不行啊，网上有人是这样推荐的

其他原因：增加代码。
```
*{
  -webkit-overflow-scrolling: touch;
}
```
但是这个代码也是有问题的，有时候滑动到底部的时候还是会卡顿。
## 3.history.pushState()在IOS微信浏览器上面不生效
## 4. （tabbar）fixed定位的在底部的按钮，安卓端，input触发键盘弹起的时候底部按钮弹起问题解决。
这个问题解决，首先要判断是否在安卓端，只在安卓端进行处理。
解决思路：在data定义两个变量screenHeight: document.body.clientHeight,originHeight: document.body.clientHeight，都赋值为document.body.clientHeight。然后利用window.onresize方法，这个方法会监听页面上尺寸的变化，在window.onresize回调函数中让一个变量screenHeight在onresize里面改变，watch中监听screenHeight的值，再做操作。
```
data () {
    return {
      screenHeight: document.body.clientHeight,
      originHeight: document.body.clientHeight
    }
  },
watch: {
  screenHeight (newVal) {
    if (this.originHeight != newVal) {
      document.getElementById('tabbarBottom').style.display = 'none'
    } else {
      document.getElementById('tabbarBottom').style.display = 'flex'
    }
  }
},
 mounted () {
    let u = navigator.userAgent
    let isAndroid = u.indexOf('Android') > -1 || u.indexOf('Adr') > -1 //android终端
    let h = document.body.scrollHeight
    // 安卓手机下处理底部tabbar在软键盘弹起的时候一起弹起的问题
    if (isAndroid) {
      window.onresize = () => {
        this.screenHeight = document.body.clientHeight
      }
    }
}
```
## 5. 微信浏览器中安卓和ios路由跳转的差异：
测试测试彻底结束