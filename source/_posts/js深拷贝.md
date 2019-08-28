---
title: js深拷贝&浅拷贝
date: 2019-08-26 16:05:47
categories: 
  - js
  - work
tags: 
  - js
  - work
---
#### 背景：
今天在公司写代码的时候，有个场景：需要在一个页面添加证书信息，但是所有添加的信息暂时只能存在前端，在提交表单之后才会提交到后端保存。所以用了vux来存数据代码如下：
<!--more-->

在添加证书页面：（不只是添加，编辑证书也是在这个页面）

```
export default {
  data () {
    return {
      credential: {
        title: '',
        value: ''
      }, // 证件信息
      credentialList: []
    }
  }
}


 mounted () {
   if (temp === 'edit') {
      this.credential = this.$store.state.credential.credential // 当前在vux中存的添加/编辑的证书对象
    }
    this.credentialList = this.$store.getters.getCredentialList // 存储在vuex中的证书列表对象
 }
```
但是在点击保存证书的按钮的时候，会对所添加的证书进行校验，如果有重复的证书就会提示“您已经添加过此证书”！
```
// 校验证书函数
checkForm () {
  // 是否有存入重复的回执编号
  for (let i = 0; i < this.credentialList.length; i++) {
    if (this.credentialList[i].value === this.credential.value) {
      Toast('您的条形码已添加，请勿重复输入。')
      return false
    }
  }
}
// 保存到接口
save () {
  this.credentialList.push(this.credential)
  this.$store.commit('setCredentialList', this.credentialList)
}
```
代码在这里其实是写完了的，但是在编辑证书的情况下，我改变了证书的value，证书列表的信息也会随之改变。其实就是在编辑状态下，改变this.credential.value，this.credentialList也会
随之改变。原因就是：
```
this.credentialList = this.$store.getters.getCredentialList
```
这一句，数组对象的浅拷贝，用深拷贝：
```
this.credentialList = JSON.parse(JSON.stringify(this.$store.getters.getCredentialList))
```
就可以解决。

## 正文：
那这篇文章就来学习一下什么是深拷贝、什么是浅拷贝，他们的区别以及应用。

#### 堆和栈的区别：
其实深拷贝和浅拷贝的主要区别就是其在内存中存储的类型不同。

堆和栈都是内存正划分出来用来存储的区域。

###### 栈（stack）为自动分配的内存空间，它由系统自动释放；而堆（heap）则是动态分配的内存，大小不定也不会自动释放。

#### JS的数据类型：
##### 基本数据类型：undefined、boolean、number、string、null

###### 基本数据类型存放在栈中;
存放在栈内存中的简单数据段，数据大小确定，内存空间大小可以分配，是直接按值存放的，所以可以直接当问。

###### 基本数据类型值不可以变：
```
javascript中的原始值（undefined、null、布尔值、数字和字符串）与对象（包括数组和函数）有着根本区别。原始值是不可更改的：任何方法都无法更改（或“突变”）一个原始值。对数字和布尔值来说显然如此 —— 改变数字的值本身就说不通，而对字符串来说就不那么明显了，因为字符串看起来像由字符组成的数组，我们期望可以通过指定索引来假改字符串中的字符。实际上，javascript 是禁止这样做的。字符串中所有的方法看上去返回了一个修改后的字符串，实际上返回的是一个新的字符串值。

```
基本数据类型的值是不可变的，动态修改了基本数据类型的值，它的原始值也是不会改变的，例如：
```
  var str = "abc";
  console.log(str[1]="f");    // f
  console.log(str)
```
###### 基本类型的比较是值的比较:
基本类型的比较是值的比较，只要它们的值相等就认为他们是相等的，例如：
```
  var a = 1;
  var b = 1;
  console.log(a === b);//true
```
#### 引用类型:
###### 引用类型存放在堆中：
引用类型（object）是存放在堆内存中的，变量实际上是一个存放在栈内存的指针，这个指针指向堆内存中的地址。每个空间大小不一样，要根据情况进行特定的分配。

###### 引用类型值可变
引用类型是可以直接改变其值的，例如：
```
  var a = [1,2,3];
  a[1] = 5;
  console.log(a[1]); // 5
```
###### 引用类型的比较是引用的比较：
我们每次对js中的引用类型进行操作的时候，都是操作其对象的引用（保存在栈内存中的指针），所以比较两个引用类型，是看其引用是否指向同一个对象。例如：
```
  var a = [1,2,3];
  var b = [1,2,3];
  console.log(a === b); // false
```
虽然变量 a 和变量 b 都是表示一个内容为 1，2，3 的数组，但是其在内存中的位置不一样，也就是说变量 a 和变量 b 指向的不是同一个对象，所以他们是不相等的。
## 传值与传址：
在我们进行赋值操作的时候，基本数据类型的赋值（=）是在内存中新开辟一段栈内存，然后再将值赋值到新的栈中。例如：
```
  var a = 10;
  var b = a;

  a ++ ;
  console.log(a); // 11
  console.log(b); // 10
```
###### 所以说，基本类型赋值的两个变量是两个独立相互不影响的变量。
但是引用类型的赋值是传址。只是改变指针的指向，例如，也就是说引用类型的赋值是对象保存在栈中的地址的赋值，这样的话两个变量就指向同一个对象，因此两者之间操作互相有影响。例如：
```
  var a = {}; // a保存了一个空对象的实例
  var b = a;  // a和b都指向了这个空对象

  a.name = 'jozo';
  console.log(a.name); // 'jozo'
  console.log(b.name); // 'jozo'

  b.age = 22;
  console.log(b.age);// 22
  console.log(a.age);// 22

  console.log(a == b);// true

```
## 浅拷贝：

#### 浅拷贝的实现方式：
###### 1.Object.assign()
Object.assign() 方法可以把任意多个的源对象自身的可枚举属性拷贝给目标对象，然后返回目标对象。但是 Object.assign()进行的是浅拷贝，拷贝的是对象的属性的引用，而不是对象本身。
```
var obj = { a: {a: "kobe", b: 39} };
var initalObj = Object.assign({}, obj);
initalObj.a.a = "wade";
console.log(obj.a.a); //wade

```
注意：当object只有一层的时候，是深拷贝
```
let obj = {
    username: 'kobe'
    };
let obj2 = Object.assign({},obj);
obj2.username = 'wade';
console.log(obj);//{username: "kobe"}

```
###### 2.Array.prototype.concat()
```
let arr = [1, 3, {
    username: 'kobe'
    }];
let arr2=arr.concat();    
arr2[2].username = 'wade';
console.log(arr);
```
###### 3.Array.prototype.slice()
###### 4.展开运算符（...）

#### 赋值（=）和浅拷贝的区别：
那么赋值和浅拷贝有什么区别呢，我们看下面这个例子：
```
    var obj1 = {
        'name' : 'zhangsan',
        'age' :  '18',
        'language' : [1,[2,3],[4,5]],
    };

    var obj2 = obj1;


    var obj3 = shallowCopy(obj1);
    function shallowCopy(src) {
        var dst = {};
        for (var prop in src) {
            if (src.hasOwnProperty(prop)) {
                dst[prop] = src[prop];
            }
        }
        return dst;
    }

    obj2.name = "lisi";
    obj3.age = "20";

    obj2.language[1] = ["二","三"];
    obj3.language[2] = ["四","五"];

    console.log(obj1);  
    //obj1 = {
    //    'name' : 'lisi',
    //    'age' :  '18',
    //    'language' : [1,["二","三"],["四","五"]],
    //};

    console.log(obj2);
    //obj2 = {
    //    'name' : 'lisi',
    //    'age' :  '18',
    //    'language' : [1,["二","三"],["四","五"]],
    //};

    console.log(obj3);
    //obj3 = {
    //    'name' : 'zhangsan',
    //    'age' :  '20',
    //    'language' : [1,["二","三"],["四","五"]],
    //};

```
先定义个一个原始的对象 obj1，然后使用赋值得到第二个对象 obj2，然后通过浅拷贝，将 obj1 里面的属性都赋值到 obj3 中。

然后我们改变 obj2 的 name 属性和 obj3 的 name 属性，可以看到，改变赋值得到的对象 obj2 同时也会改变原始值 obj1，而改变浅拷贝得到的的 obj3 则不会改变原始对象 obj1。这就可以说明赋值得到的对象 obj2 只是将指针改变，其引用的仍然是同一个对象，而浅拷贝得到的的 obj3 则是重新创建了新对象。

然而，我们接下来来看一下改变引用类型会是什么情况呢，我又改变了赋值得到的对象 obj2 和浅拷贝得到的 obj3 中的 language 属性的第二个值和第三个值（language 是一个数组，也就是引用类型）。结果见输出，可以看出来，无论是修改赋值得到的对象 obj2 和浅拷贝得到的 obj3 都会改变原始数据。

###### 这是因为浅拷贝只复制一层对象的属性，并不包括对象里面的为引用类型的数据。所以就会出现改变浅拷贝得到的 obj3 中的引用类型时，会使原始数据得到改变。

深拷贝：将 B 对象拷贝到 A 对象中，包括 B 里面的子对象，

浅拷贝：将 B 对象拷贝到 A 对象中，但不包括 B 里面的子对象

| -- | 和原数据是否指向同一对象 | 第一层数据为基本数据类型 | 原数据中包含子对象 |
| ------ | ------ | ------ | ------ |
| 赋值 | 是 | 改变会使原数据一同改变 | 改变会使原数据一同改变 |
| 浅拷贝 | 否 | 改变不会使原数据一同改变 | 改变会使原数据一同改变 |
| 深拷贝 | 否 | 改变不会使原数据一同改变 | 改变不会使原数据一同改变 |

## 深拷贝：

深拷贝是对对象以及对象的所有子对象进行拷贝。
#### 那么问题来了，怎么进行深拷贝呢？

* 方法一： 思路就是递归调用刚刚的浅拷贝，把所有属于对象的属性类型都遍历赋给另一个对象即可。（递归递归去复制所有层级属性）
* 方法二： 除了递归，我们还可以借用JSON对象的parse和stringify
```
function deepClone(obj){
    let _obj = JSON.stringify(obj),
        objClone = JSON.parse(_obj);
    return objClone
}   
```
