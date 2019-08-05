---
title: vue 实现一个倒计时功能
date: 2019-08-05 17:28:35
categories: 
  - work
  - Vue
tags: 
  - work
  - Vue
---

### 背景：
需要实现一个倒计时，给定一个截止时间，展示一个当前时间到截止时间差的倒计时，表现格式为，天，时分秒。
### 实现：
##### 思路：
根据时间差来调用setInterval,setInterval内部是调用将时间戳转化为年月日时分秒的格式展示的方法。vue中filter字面意思，过滤器，通过管道符号，将参数经过过滤器后返回你需要的格式。所以这里可以使用vue中filter来表示。
##### 注意：
用到了定时器要清除，考虑到内存的问题。
 <!--more-->
##### 实现代码：

```
<tempalet>
<span>支付剩余时间：{{end | countDown}}</span>
</template>



 data () {
    return {
      end: '', // 付款结束时间
      timer: '',
  },
  filters: {
    countDown (val) {
      // 参数为秒为单位
      let totalTime = val
      let day = parseInt(totalTime/(24*60*60)) // 计算整数天数
      let afterDay = totalTime - day*24*60*60 // 取得算出天数后剩余的秒数
      let hour = parseInt(afterDay/(60*60)) > 10 ? parseInt(afterDay/(60*60)) : '0' + parseInt(afterDay/(60*60))  // 计算整数小时数
      let afterHour = totalTime - day*24*60*60 - hour*60*60 // 取得算出小时数后剩余的秒数
      let min = parseInt(afterHour/60) > 10 ? parseInt(afterHour/60) : '0' + parseInt(afterHour/60) // 计算整数分
      let afterMin = parseInt(totalTime - day*24*60*60 - hour*60*60 - min*60) > 10 ? parseInt(totalTime - day*24*60*60 - hour*60*60 - min*60) : '0' + parseInt(totalTime - day*24*60*60 - hour*60*60 - min*60) // 取得算出分后剩余的秒数
      let lastTime = day + '天  '  + hour + ':' + min + ':' + afterMin
      return lastTime
    }
  },
method: {
  test () {
    // 处理倒计时
    let endTime = new Date(this.orderObj.payEndIn).getTime()
    let nowTime = new Date().getTime()
    this.end = Math.floor((endTime - nowTime)/1000)
    if (this.timer) clearInterval(this.timer)
    this.timer = setInterval(() => {
      this.end--
    }, 1000)
  }
}
```