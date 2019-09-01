---
title: nuxt学习
date: 2019-08-25 15:19:48
tags: 
  - nuxt
categories: 
  - nuxt
---
#### 介绍：
Nuxt.js是一个基于Vus.js的通用应用框架，是一个基于Vue.js的服务端渲染应用框架。

官方说明：目标是创建一个灵活的应用框架，你可以基于它初始化新项目的基础结构代码，或者在已有Node.js项目中使用nuxt.js。
 <!--more-->
#### 特性：
* 基于Vue.js
* 自动代码分层
* 服务端渲染
* 强大的路由功能，支持异步数据
* 静态文件服务
* ES2015+语法支持
* 打包和压缩JS和CSS
* HTML头部标签管理
* 本地开发支持热加载
* 集成ESlint
* 支持各种样式预处理器：SASS、LESS、Stylus等
* 支持HTTP/2推送
#### 流程图：
下图阐述了Nuxt.js应用一个完整的服务器请求到渲染（或用户通过<nuxt-link>切换路由渲染页面）的流程：
![nuxtFlow](/img/nuxtFlow.png)

#### 开始使用：
为了快速入门，Nuxt.js团队创建了脚手架工具create-nuxt-app

确保安装了npx（npx在NPM版本5.2.0默认安装了）：
```
npx create-nuxt-app <项目名>
```
或者用yarn：
```
yarn create nuxt-app <项目名>
```
之后有一些选择，可以根据自己项目的需要进行选择。
安装完成之后，就是启动项目：
```
cd <project-name>
npm run dev
```
应用在http://localhost:3000上运行：
###### 注意：Nuxt.js 会监听 pages 目录中的文件更改，因此在添加新页面时无需重新启动应用程序。

#### pages目录：
Nuxt.js会根据pages目录中所有的*.vue文件生成应用的路由配置。
* 更多关于nuxt.js路由的配置以及实现还是可以看看文档。
Nuxt.js为页面提供的特殊的配置项：
![nuxtFlow](/img/pages.png)
###### HTML头部
Nuxt.js 使用了 vue-meta 更新应用的 头部标签(Head) and html 属性。
#### 资源文件：
资源目录 assets 用于组织未编译的静态资源如 LESS、SASS 或 JavaScript。

默认情况下 Nuxt 使用 vue-loader、file-loader 以及 url-loader 这几个 Webpack 加载器来处理文件的加载和引用。对于不需要通过 Webpack 处理的静态资源文件，可以放置在 static 目录中。
如果你的静态资源文件需要 Webpack 做构建编译处理，可以放到 assets 目录，否则可以放到 static 目录中去。
#### 组件目录：
组件目录components用于组织应用的Vue.js组件。Nuxt.js不会扩展增强该目录下Vue.js组件，即这些组件不会像页面组件那样有asyncData方法的特性。
#### 布局目录：
布局目录layouts用于组织应用的布局组件。若无额外配置，该目录不能被重命名。

* Nuxt.js 允许你扩展默认的布局，或在 layout 目录下创建自定义的布局。
* 可通过添加 layouts/default.vue 文件来扩展应用的默认布局。
###### 提示: 别忘了在布局文件中添加 <nuxt/> 组件用于显示页面的主体内容。
* layouts 目录中的每个文件 (顶级) 都将创建一个可通过页面组件中的 layout 属性访问的自定义布局。
假设我们要创建一个 博客布局 并将其保存到layouts/blog.vue:
```
<template>
  <div>
    <div>我的博客导航栏在这里</div>
    <nuxt/>
  </div>
</template>
```
然后我们必须告诉页面 (即pages/posts.vue) 使用您的自定义布局：
```
<template>
<!-- Your template -->
</template>
<script>
export default {
  layout: 'blog'
  // page component definitions
}
</script>
```

#### 中间件目录：
middleware 目录用于存放应用的中间件。
* 中间件允许您定义一个自定义函数运行在一个页面或一组页面渲染之前。(中间件可以使您自定义的函数在渲染页面之前运行。)
* 每一个中间件应放置在 middleware/ 目录。文件名的名称将成为中间件名称(middleware/auth.js将成为 auth 中间件)。
一个中间件接收 context 作为第一个参数：
```
export default function (context) {
  context.userAgent = process.server ? context.req.headers['user-agent'] : navigator.userAgent
}
```
中间件可以异步执行,只需要返回一个 Promise 或使用第2个 callback 作为第一个参数：
middleware/stats.js
```
import axios from 'axios'

export default function ({ route }) {
  return axios.post('http://my-stats-api.com', {
    url: route.fullPath
  })
}
```
然后在你的 nuxt.config.js 、 layouts 或者 pages 中使用中间件:
nuxt.config.js
```
module.exports = {
  router: {
    middleware: 'stats'
  }
}
```
您也可以将 middleware 添加到指定的布局或者页面:

pages/index.vue 或者 layouts/default.vue
```
export default {
  middleware: 'stats'
}
```
#### 静态文件目录:
静态文件目录 static 用于存放应用的静态文件，此类文件不会被 Nuxt.js 调用 Webpack 进行构建编译处理。 服务器启动的时候，该目录下的文件会映射至应用的根路径 / 下。
举个例子: /static/robots.txt 映射至 /robots.txt

若无额外配置，该目录不能被重命名。
#### Store 目录:
store 目录用于组织应用的 Vuex 状态树 文件。 Nuxt.js 框架集成了 Vuex 状态树 的相关功能配置，在 store 目录下创建一个 index.js 文件可激活这些配置。
若无额外配置，该目录不能被重命名。
###### nuxtServerInit 方法,如果你使用_状态树模块化_的模式，只有主模块（即 store/index.js）适用设置该方法（其他模块设置了也不会被调用）。
#### nuxt.config.js 文件:
nuxt.config.js 文件用于组织Nuxt.js 应用的个性化配置，以便覆盖默认配置。

若无额外配置，该文件不能被重命名。
#### asyncData 方法:
asyncData方法会在组件（限于页面组件）每次加载之前被调用。它可以在服务端或路由更新之前被调用。 在这个方法被调用的时候，第一个参数被设定为当前页面的上下文对象，你可以利用 asyncData方法来获取数据，Nuxt.js 会将 asyncData 返回的数据融合组件 data 方法返回的数据一并返回给当前组件。

###### 注意：由于asyncData方法是在组件 初始化 前被调用的，所以在方法内是没有办法通过 this 来引用组件的实例对象。
#### 关于nuxt中的vuex：
nuxt中的vuex是客户端和服务端共享的，里面的资源都是在客户端和服务端共享的。

#### 重要理解的点：
* nuxt里面创建即配置：
例如，我们再nuxt-link中我们使用了to跳转到注册页面"/register"，只需要创建一个register页面就可以，也不用配置路由。
*  rout-link、a标签、nuxt-link实现的其实都是a标签。
*  package-lock.json的作用：
其实用一句话来概括很简单，就是锁定安装时的包的版本号，并且需要上传到git，以保证其他人在npm install时大家的依赖能保证一致。

根据官方文档，这个package-lock.json 是在 `npm install`时候生成一份文件，用以记录当前状态下实际安装的各个npm package的具体来源和版本号。
#### 什么叫ssr？ssr意义是什么？
ssr意为Server Side Rendering（服务端渲染），目的是为了解决单页面应用的SEO的问题，对于一般网站影响不大，但对于论坛类，内容类网站来说是致命的。搜索引擎无法抓取页面相关内容，也就是用户搜不到此网站的相关信息。

抓取页面的前提是html含有被抓取内容，我们不妨看看基于vue的线上SPA页面请求时返回了什么:
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset=utf-8>
    <title>iDareX敢玩</title>
    <meta name=keywords content="敢玩, iDareX, 敢玩TV, 敢玩活动, 敢玩自频道, 敢玩主题, 户外, 极限运动, 周边游, 探险, 时尚, 新潮, 运动视频, 体育, 新奇, 生活方式, 刺激, 惊险, 户外装备, 达人, 90后">
    <meta name=description content=自2014年10月创办以来，敢玩专注于极限户外和娱乐体育。从顽童、玩具、玩法三个方面，产出更专注于‘玩’的内容，已打造了一系列深受喜爱的娱乐体育真人秀和引爆网络的运动视频。!>
    <meta name=renderer content=webkit>
    <meta name=force-rendering content=webkit>
    <meta name=viewport content="width=1140">
    <meta http-equiv=X-UA-Compatible content="IE=edge,chrome=1">
    <link rel="shortcut icon" href=static/favicon.ico type=image/x-icon>
    <link href=/static/css/app.eef5b81a3d1bee5054a791f452a34147.css rel=stylesheet>
  </head>
  <body>
    <div id=app></div>
    <script type=text/javascript src=/static/js/manifest.6d0adb8f2d8884be1c03.js></script>
    <script type=text/javascript src=/static/js/vendor.ec1cc90c9847c434ba7d.js></script>
    <script type=text/javascript src=/static/js/app.d7fd10ae7e4a68598037.js></script>
  </body>
</html>

```
我们的组件都是这个 html 文件返回后再渲染到 <div id=app></div> 里的。这就合理的解释了 SEO 缺陷的原因。(网页是通过服务端渲染生成后输出给客户端。 )

###### 另外 SSR 还适用以下场景

* 客户端的网络比较慢

* 客户端运行在老的或者直接没有 JavaScript 引擎上
