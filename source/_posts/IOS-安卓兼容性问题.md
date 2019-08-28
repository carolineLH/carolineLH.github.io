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
### 背景：
想要隐藏所有非基础按钮接口
###### 由于在微信浏览器中在跳转页面的时候url改变时，安卓和ios获得签名的原理不同，导致实现的差异。
在ios内部，当页面跳转的时候，url改变，但是当签名存在的时候，即使时url改变了也不会去重新获取签名。所以，只需要在入口组件里面执行一次wx.hideAllNonBaseMenuItem();就好；
但是在安卓，url改变时，就会重新去获取签名。所以就要在每次url改变之后执行一次wx.hideAllNonBaseMenuItem();

### 实现方法：
在vue项目中，给index.html中创建一个script标签，通过这个标签执行wx.hideAllNonBaseMenuItem()方法。核心代码;
```
// 调用微信配置js
export function configWx () {
  return new Promise((resolve, reject) => {
    if (window.wx) {
      resolve(window.wx)
      window.wx.hideAllNonBaseMenuItem()
    } else {
      let script = document.createElement('script')
      script.type = 'text/javascript'
      script.async = true
      script.src = '' // 获得签名的方法
      script.setAttribute('apiList', 'hideAllNonBaseMenuItem')
      script.setAttribute('fn', 'wx.hideAllNonBaseMenuItem()')
      script.id = 'shareScript'
      script.onerror = reject
      document.head.appendChild(script)
      resolve(window.wx)
    }
  })
}

```
在beforeEach中判断是ios还是安卓，如果是ios就执行一次；不是的话就要每个页面执行一次configWx方法：
```
router.beforeEach((to, from, next) => {
  if (isIOS()) {
    if (from.path === '/') {
      configWx()
    }
  }
  next()
})
```

## 6.ios滚动穿透：

#### 背景：
场景就是在页面上有一个popup弹层，当弹层出现的时候，你滑动弹层，发现弹层下面的页面可以滚动。只会出现在ios上面。

#### 解决：
解决思路就是在弹框出现后，禁止页面滑动，对话框关闭的时候再移除监听。

在弹层出现的时候调用禁止默认事件函数，在弹层关闭的时候调用打开默认事件函数：
```
/**
   * 禁止页面滚动和解除滚动的共用函数，具体看这个文章
   * https://blog.csdn.net/qq_29606781/article/details/67650869
   * 1：相同事件绑定和解除，需要使用共用函数；绑定和解除事件时 事件没有"on" 即onclick写成click
   * 2：共用函数不能带参数；（即下面在调用的时候是用的 this.bodyScroll，不能带()。）
   */
  bodyScroll(event){
    event.preventDefault();
  }
  
  /**
   * 禁止页面滚动，解决弹框出现时 IOS 上滚动穿透的问题
   */
  forbidBodyScroll () {
    document.getElementsByTagName('body')[0].addEventListener('touchmove', this.bodyScroll, false)
  }
  
  /**
   * 解除禁止滚动，解决弹框出现时 IOS 上滚动穿透的问题
   */
  allowBodyScroll () {
    document.getElementsByTagName('body')[0].removeEventListener('touchmove', this.bodyScroll, false)
  }

```
