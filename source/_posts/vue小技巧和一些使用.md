---
title: vue小技巧和一些使用
date: 2019-08-05 18:17:12
tags: 
  - vue
categories: 
  - vue
---
## vue中的小技巧：
#### 1. 组件style的scoped:
当style标签有scoped属性时，它的CSS只作用于当前组件中的元素。它会为组建中所有的标签和class样式添加一个scoped标识，就像这样子的:data-v-1b971ada.

问题：在组件中用js动态创建的dom，css中添加的样式不生效？
答：因为动态添加的dom没有添加上标识，而css中的样式因为scoped的原因添加上了标识，标识对不上，所以css不在dom上面生效。
<!--more-->
#### 2. Vue 数组/对象更新 视图不更新：
很多时候，我们会这样操作数组：
```
data() { // data数据
    return {
      arr: [1,2,3],
      obj:{
          a: 1,
          b: 2
      }
    };
  },
// 数据更新 数组视图不更新
this.arr[0] = 'OBKoro1';
this.arr.length = 1;
console.log(arr);// ['OBKoro1'];
// 数据更新 对象视图不更新
this.obj.c = 'OBKoro1';
delete this.obj.a;
console.log(obj);  // {b:2,c:'OBKoro1'}

```
由于js的限制，Vue不能检测以上数组的变动，以及对象的添加/删除，很多人会因为像上面的操作，出现视图没有更新的问题。

解决方式：
1. this.$set(你要改变的数组/对象，你要改变的位置/key，你要改成什么value)
```
this.$set(this.arr, 0, "OBKoro1"); // 改变数组
this.$set(this.obj, "c", "OBKoro1"); // 改变对象
```
2. 数组原生方法触发视图更新：
Vue可以监测到数组变化的，数组原生方法：
```
splice()、push()、pop()、shift()、unshift()、sort()、reverse()
```
意思是使用这些方法不用我们再进行额外的操作，视图自动进行更新。

#### 3. vue filters过滤器的使用：
过滤器，通常用于后台管理系统，或者一些约定类型，过滤。

在html模板中的两种用法：
```
<!-- 在双花括号中 -->
{{ message | filterTest }}
<!-- 在 `v-bind` 中 -->
<div :id="message | filterTest"></div>
```
在组建script中的用法：
```
export default {    
     data() {
        return {
         message:1   
        }
     },
    filters: { // 一点是返回某个结果，将参数经过过滤器后返回你需要的格式
        filterTest(value) {
            // value在这里是message的值
            if(value===1){
                return '最后输出这个值';
            }
        }
    }
}
```
#### 4. 列表渲染相关：
###### v-if尽量不要与v-for在同一节点使用:
v-for的优先级比v-if的更高，如果他们处于同一节点的话，那么每一个循环都会执行一遍v-if。如果是想要根据循环中的每一项的数据来判断是都渲染，那可以那样做。
###### 如果你想要根据某些条件跳过循环，而又跟将要渲染的每一项数据没有关系的话，你可以将v-if放在v-for的父节点：

#### 5. 这些情况下不要使用箭头函数：
* 不应该使用剪头函数来定义一个生命周期方法；
* 不应该使用箭头函数来定义method函数；
* 不应该使用箭头函数来定义计算属性函数；
* 不应该对data属性使用箭头函数；
* 不应该使用箭头函数来定义watche函数

箭头函数绑定了父级作用域的上下文，this将不会按照期望指向Vue实例。
也就是说，你不能使用this来访问你组件中的data数据以及method方法了。
this将会指向undefined。 