---
title: npm包 rimraf
date: 2019-11-20 10:11:30
categories: 
  - npm
  - work
tags: 
  - npm
  - work
---
## 背景
在每次删除项目中的node_modules文件时都特别慢，希望可以有方法快速删除。。
 <!--more-->
## rimraf 

#### 含义
rimraf包的作用：以包的形式包装rm-rf命令，用来删除文件和文件夹。不管文件夹是否为空，都可以删除。

#### 安装
```
npm install rimraf --save-dev
```

#### 使用
```
const rimraf = require('rimraf');
rimraf('./test.txt', function (err) { // 删除当前目录下的 test.txt
  console.log(err);
});
```
或者直接在命令行下输入如下命令：
```
rimraf node_modules
```


