---
title: 上传文件或图片时选择相同文件无法触发change事件的问题
date: 2019-08-28 15:45:20
categories: 
  - work
tags: 
  - work
---
#### 背景：
最近在项目中，遇到做一个上传文件的模块时：打开文件上传，上传成功之后，再次上传，再次选之前的那个文件，change事件不会被触发。
 <!--more-->
代码如下：
```
<input type="file" name="file" class="file-input" @change="setFile" />

```

#### 解决：
原因是第一个选择文件一时，存放的文件由空变成了文件一，上传成功之后，再次选择文件一，此时相当于文件没有change，所以这时候change不会触发了。

解决就是需要在选择的文件清空。
```
setFile(e){
       ......
       e.srcElement.value = ""//及时清空
}

```
