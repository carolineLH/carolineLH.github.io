---
title: vue中使用mintui的infinite-scroll组件进行无限滚动加载
date: 2019-10-23 10:45:22
tags: 
  - vue
  - work
categories: 
  - vue
  - work
---
## 背景
  最近在工作中，在移动端开发中，有个数据列表，数据很多。如果不做分页加载的话，用户体验会很不好。因为请求的数据过多，请求会比较慢。所以，就需要做分页加载。
  
  想实现的分页效果是：随着我们在移动端往下滚动的时候，加载更多的数据，每次加载限制加载数据的条数。于是就想到了用mint-ui的infinite scroll组件。
 <!--more-->
## 使用：
#### 1. 保证你的项目中安装了min-ui
```
npm i mint-ui -S
```
#### 2. 在vue中引入mint-ui
针对vue-cli3+，引用组件到vue中。在babel.config.js文件中：

```module.exports = {
  presets: [
    '@vue/app'
  ],
  plugins: [
    [
      'component',
      {
        'libraryName': 'mint-ui',
        'style': true
      }
    ]
  ]
}
```
#### 3. 按需引入InfiniteScroll组件：
```
import { InfiniteScroll } from 'mint-ui';
Vue.use(InfiniteScroll);
```

#### 4.在代码中使用：

```html
    <div class="state-list" 
      v-infinite-scroll="loadMore"
      infinite-scroll-disabled="loading"
      infinite-scroll-immediate-check=false
      infinite-scroll-distance="10">
      <div class="state-item" v-for="(item, index) in popularList" :key="index" @click="toLogistics(item.postcode)">
        <div class="avatar">
          <img src="../../assets/img/delivery.png" alt="" >
        </div>
        <div class="state-info">
          <span class="name">{{item.postcode}}</span>
          <span class="turn-over">{{item.orderPrice}}</span>
          <span class="binding-time">{{item.orderName}}</span>
        </div>
      </div>
    </div>
```

```javascript
  loadMore () {
    this.loading = true
    this.getOrder('2') 
  },
  // 请求加载数据
  async getOrder (type) {
    this.loading = false
    let self = this
    let params = {
      channelType: 'WECHAT',
      personType: this.code,
      queryType: type,
      year: this.year,
      month: this.month,
      orderType: this.checkType
    }
    if (type === '2' && this.page > 0) {
      params.totalNum = 10*this.page
    }
    this.$indicator.open()
    await this.$fetch.post('api/getPopularizationOrder', params)
      .then(res => {
        this.$indicator.close()
        if (res.data.result === 'SUCCESS') {
          let data = res.data.data
          if (type === '2') {
            if (res.data.data.popularList.length > 0) {
              self.popularList = self.popularList.concat(res.data.data.popularList)
              self.page++
            } else {
              self.loading = true
            }
          }
        }
      })
    }
```

#### 5.选项解释

v-infinite-scroll="loadMore"表示回调函数是loadMore

infinite-scroll-disabled="loading"表示由变量loading决定是否执行loadMore，false则执行loadMore，true则不执行，看清楚，loading表示繁忙，繁忙的时候是不执行的。

infinite-scroll-distance="10"这里10决定了页面滚动到离页尾多少像素的时候触发回调函数，10是像素值。通常我们会在页尾做一个几十像素高的“正在加载中...”，这样的话，可以把这个div的高度设为infinite-scroll-distance的值即可。

其他选项：

infinite-scroll-immediate-check 默认值为true，该指令意思是，应该在绑定后立即检查loading的值和是否滚动到底。如果你的初始内容高度不够高、不足以填满可滚动的容器的话，你应设为true，这样会立即执行一次loadMore，会帮你填充一些初始内容。

infinite-scroll-listen-for-event 当事件在Vue实例中发出时，无限滚动将再次检查。

infinite-scroll-throttle-delay 检查busy的值的时间间隔，默认值是200，因为vue-infinite-scroll的基础原理就是，vue-infinite-scroll会循环检查busy的值，以及是否滚动到底，只有当：busy为false且滚动到底，回调函数才会执行。

#### 6.遇到的坑：

###### 一、就是当loadmore触发之后，getOrder方法无限触发，也就是无限调用获取数据函数。

原因： 没有理解loadmore是什么时候执行，loading为true的时候是不执行的，为false的时候执行。

###### 二、滚动不加载：

原因：这种情况，就是在loadMore方法里没有写this.loading = true，同时，第一次加载的数据也没有撑满容器。

解决：所以我们要给最外层容器添加如下样式：
```
  height: 100%;
  overflow-y: auto;
```

###### 三、页面初始化的时候会自动调用一次loadmore:

解决办法：infinite-scroll-immediate-check 设置为false。因为infinite-scroll-immediate-check 指令是在绑定后立即检查loading的值和是否滚动到底。如果你的初始内容高度不够高、不足以填满可滚动的容器的话，你应设为true，这样会立即执行一次loadMore，会帮你填充一些初始内容。如果不需要调用loadmore来填充数据则设置为false。

###### 四、页面中有类似tabbar，在切换tabbar页面的时候，loadmore会自动调用。

原因：其实这种情况跟页面初始化的时候会自动调用一次loadmore是一样的。解决办法也是一样的，将infinite-scroll-immediate-check 设置为false。




