---
title: JS面试
date: 2019-08-31 15:34:50
categories:
  - js
  - 面试
tags:
  - js
  - 面试
---

## 1. new Boolean(false)是真还是假？

JavaScript 的其他数据类型都可以转化成 Boolean 类型，但是注意

 <!--more-->

###### 只有这几种类型会转化成 false：

- undefined
- null
- 0
- -0
- NaN
- ''(空字符串)

###### 其他的都会转化成 true。空对象{}，空数组[]，负数，false 的对象包装（值为 false 的 Boolean 对象）等等。

```
if(-1){
  console.log('-1转换为true');
}
if({}){
  console.log('{}转换为true');
}
if([]){
  console.log('[]转换为true');
}
if(new Boolean(false)){
  console.log('new Boolean(false)转换为true');
}

```

new Boolean(false)是布尔值的包装对象 typeof (new Boolean(false)) // 'object' ,所以 转换为 boolean 是 true，而不是 false

new Boolean()出来的一定是个对象，但是 Boolean(value)的值是要看所传递的参数 value 是不是 0,-0,null,undefined,'',NaN 这六个，只要不是这六个就是 true。

## 2. if 语句中条件的判断

1. 对于非布尔类型的数据，会先转换成布尔类型在判断

```
  if(null){
    if(1){
        console.log("语句A");
    }
    console.log("语句B");
  }
```

2. 在 JavaScript 中分号(;)也是一条语句(空语句)

```
if(false);
  {
      console.log("语句A");
      console.log("语句B");
  }

```

总之 if 条件中语句最终就是要得到 true 或者是 false 两个值的。

## 3. javaScript 有哪几种方法定义函数：

1. 函数声明表达式；

```
function calcRectArea(width, height) {
  return width * height;
}

console.log(calcRectArea(5, 6));
```

2. function 操作符

```
var getRectArea = function(width, height) {
    return width * height;
}

console.log(getRectArea(3,4));
```

3. Function 构造函数：

```
var sum = new Function('a', 'b', 'return a + b');

console.log(sum(2, 6));
```

4. ES6: arrow function:

```
var materials = [
  'Hydrogen',
  'Helium',
  'Lithium',
  'Beryllium'
];

console.log(materials.map(material => material.length));
```

## 4. javascript 有哪些方法定义对象

1. 对象字面量： var obj = {};
2. 构造函数： var obj = new Object();
3. Object.create(): var obj = Object.create(Object.prototype);

## 5. ===运算符判断相等的流程是怎样的

(1)不同类型值
如果两个值的类型不同，直接返回 false。

(2)同一类的原始类型值
同一类型的原始类型的值（数值、字符串、布尔值）比较时，值相同就返回 true，值不同就返回 false。-0 等于 0

(3)同一类的复合类型值
两个复合类型（对象、数组、函数）的数据比较时，不是比较它们的值是否相等，而是比较它们是否指向同一个对象。

(4)undefined 和 null
undefined 和 null 与自身严格相等。

```
null === null  //true
undefined === undefined  //true

```

(5) 如果其中有一个是 NaN，它们不相等

## 6. ==运算符判断相等的流程是怎样的

1. 如果两个值类型相同，按照===比较方法进行比较
2. 如果类型不同，使用如下规则进行比较
3. 如果其中一个值是 null，另一个是 undefined，它们相等
4. 如果一个值是数字另一个是字符串，将字符串转换为数字进行比较
5. 如果有布尔类型，将 true 转换为 1，false 转换为 0，然后用==规则继续比较
6. 如果一个值是对象，另一个是数字或字符串，将对象转换为原始值然后用==规则继续比较
7. 其他所有情况都认为不相等

## 7. 对象到字符串的转换步骤

1. 如果对象有 toString()方法，javaScript 调用它。如果它返回一个原始值（primitive value 如：string number boolean），将这个值转换为字符串作为结果。
2. 如果对象没有 toString()方法或者返回值不是原始值，javaScript 寻找对象的 valueOf()方法，如果存在就调用它,返回结果是原始值则转为字符串作为结果。
3. 否则，javaScript 不能从 toString()或者 valueOf()获得一个原始值，此时 throws a TypeError。

## 8. 对象到数字的转换步骤

1. 如果对象有 valueOf()方法并且返回元素值，javascript 将返回值转换为数字作为结果。
2. 否则，如果对象有 toString()并且返回原始值，javascript 将返回结果转换为数字作为结果。
3. 否则，throws a TypeError。

## 9. <,>,<=,>=的比较规则

所有比较运算符都支持任意类型，但是比较只支持数字和字符串，所以需要执行必要的转换然后进行比较，转换规则如下：

1. 如果操作数是对象，转换为原始值，如果 valueOf()方法返回原始值，则使用这个值。否则使用 toString 方法的结果，如果转换失败则报错。
2. 经过必要的对象到原始值的转换后，如果两个操作数都是字符串，按照字母顺序进行比较（他们的 16 位 unicode 值的大小）。
3. 否则，如果有一个操作数不是字符串，将两个操作数转换为数字进行比较。

## 10. +运算符工作流程

1. 如果有操作数是对象，转换为原始值
2. 此时如果有一个操作数是字符串，其他的操作数都转换为字符串并执行连接
3. 否则：所有操作数都转换为数字并执行加法

## 11. 函数内部 arguments 变量有哪些特性，有哪些属性，如何将它转换为数组

- arguments 所有函数中都包含的一个局部变量，是一个类数组对象，对应函数调用时的实参。如果函数定义同名参数会在调用时覆盖默认对象。
- arguments[index]分别对应函数调用时的实参，并且通过 arguments 修改实参会同时修改实参。
- arguments.length 为实参的个数（Function.length 表示形参长度）。
- arguments.callee 为当前正在执行的函数本身，使用这个属性进行递归调用时需注意 this 的变化。
- arguments.caller 为调用当前函数的函数（已被遗弃）
- 转换为数组：var args = Array.prototype.slice.call(arguments, 0);

## 12、offsetWidth/offsetHeight,clientWidth/clientHeight 与 scrollWidth/scrollHeight 的区别

- offsetWidth/offsetHeight 返回值包含 content + padding + border，效果与 e.getBoundingClientRect()相同。
- clientWidth/clientHeight 返回值只包含 content + padding，如果有滚动条，也不包含滚动条。
- scrollWidth/scrollHeight 返回值包含 content + padding + 溢出内容的尺寸。

## 13、focus/blur 与 focusin/focusout 的区别与联系

1.focus/blur 不冒泡，focusin/focusout 冒泡；
2.focus/blur 兼容性好，focusin/focusout 在除 FireFox 外的浏览器下都保持良好兼容性，如需使用事件托管，可考虑在 FireFox 下使用事件捕获 elem.addEventListener('focus', handler, true)； 3.可获得焦点的元素：

- window
- 链接被点击或键盘操作
- 表单空间被点击或键盘操作
- 设置 tabindex 属性的元素被点击或键盘操作

## 为什么有的编程规范要求用 void 0 代替 undefined？

- Undefined 类型表示未定义，它的类型只有一个值，就是 undefined。任何变量在赋值前都是 Undefined 类型，值为 undefined，一般我们可以用全局变量 undefined（就是名为 undefined 的这个变量）来表达这个值，或者 void 运算来把任意一个表达式变成 undefined 值。

- 但是 Undefined 类型的值也是一个变量，它的值是可以被改变的。而 void 运算符对给定的表达式进行求值，最终结果都是返回 undefined。
- 所以为了避免 undefined 值被重写，为了得到 undefined 的值，所以用 void 0 来代替 undefined。

## 字符串有最大长度吗？

- String 用于表示文本数据。String 有最大长度是 2^53 - 1，这在一般开发中都是够用的，但是有趣的是，这个所谓最大长度，并不完全是你理解中的字符数。

- 因为 String 的意义并非“字符串”，而是字符串的 UTF16 编码，我们字符串的操作 charAt、charCodeAt、length 等方法针对的都是 UTF16 编码。所以，字符串的最大长度，实际上是受字符串的编码长度影响的。

- JavaScript 中的字符串是永远无法变更的，一旦字符串构造出来，无法用任何方式改变字符串的内容，所以字符串具有值类型的特征。

- JavaScript 字符串把每个 UTF16 单元当作一个字符来处理，所以处理非 BMP（超出 U+0000 - U+FFFF 范围）的字符时，你应该格外小心。

## 0.1 + 0.2 不是等于 0.3 么？为什么 JavaScript 里不是这样的？

- avaScript 中的 Number 类型基本符合 IEEE 754-2008 规定的双精度浮点数规则，但是 JavaScript 为了表达几个额外的语言场景（比如不让除以 0 出错，而引入了无穷大的概念）。
- JavaScript 中有 +0 和 -0，在加法类运算中它们没有区别，但是除法的场合则需要特别留意区分，“忘记检测除以 -0，而得到负无穷大”的情况经常会导致错误，而区分 +0 和 -0 的方式，正是检测 1/x 是 Infinity 还是 -Infinity。
- 根据浮点数的定义，非整数的 Number 类型无法用 ==（=== 也不行） 来比较，一段著名的代码，这也正是我们第三题的问题，为什么在 JavaScript 中，0.1+0.2 不能 =0.3：

```js
console.log(0.1 + 0.2 == 0.3)
```

这里输出的结果是 false，说明两边不相等的，这是浮点运算的特点，也是很多同学疑惑的来源，浮点数运算的精度问题导致等式左右的结果并不是严格相等，而是相差了个微小的值。

- 所以实际上，这里错误的不是结论，而是比较的方法，正确的比较方法是使用 JavaScript 提供的最小精度值：

```js
console.log(Math.abs(0.1 + 0.2 - 0.3) <= Number.EPSILON)
```

检查等式左右两边差的绝对值是否小于最小精度，才是正确的比较浮点数的方法。这段代码结果就是 true 了。

## ES6 新加入的 Symbol 是个什么东西？

- symbol 表示独一无二的值。他是 javascript 语言的第七种数据类型。
- Symbol 值通过 symbol 函数生成。这就是说，对象的属性名现在可以有两种尅性，一种是原来就有的字符串，另一种就是新增的 Symbol 类型。凡是属性名属于 Symbol 类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。

```js
let s = Symbol()

typeof s
// "symbol
```

- 注意，Symbol 函数前不能使用 new 命令，否则会报错。这是因为生成的 Symbol 是一个原始类型的值，不是对象。也就是说，由于 Symbol 值不是对象，所以不能添加属性。基本上，它是一种类似于字符串的数据类型。

* Symbol 函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。
* Symbol 函数的参数只是表示对当前 Symbol 值的描述，因此相同参数的 Symbol 函数的返回值是不相等的。
* Symbol 值不能与其他类型的值进行运算，会报错。

```js
let sym = Symbol('My symbol')

'your symbol is ' +
  sym`your symbol is ${
    sym // TypeError: can't convert symbol to string
  }`
```

- 但是，Symbol 值可以显式转为字符串。另外，Symbol 值也可以转为布尔值，但是不能转为数值。

```js
let sym = Symbol('My symbol')

String(sym) // 'Symbol(My symbol)'
sym.toString() // 'Symbol(My symbol)'
```

#### symbol 的运用

- 使用 Symbol 来作为对象属性名(key)。因为 symbol 定义的是独一无二的，所以不存在重复的现象
- 注意用 symbol 定义的属性，在用 Object.keys()或者是 for..of..遍历的时候是不能遍历出来的，但是可以通过如下办法补救：

```js
var o = new Object()
o[Symbol.iterator] = function() {
  var v = 0
  return {
    next: function() {
      return { value: v++, done: v > 10 }
    }
  }
}
for (var v of o) console.log(v) // 0 1 2 3 ... 9
```

代码中我们定义了 iterator 之后，用 for(var v of o) 就可以调用这个函数，然后我们可以根据函数的行为，产生一个 for…of 的行为。

## 为什么给对象添加的方法能用在基本类型上？

答：. 运算符提供了装箱操作，它会根据基础类型构造一个临时对象，使得我们能在基础类型上调用对应对象的方法。

```js
Symbol.prototype.hello = () => console.log('hello')
var a = Symbol('a')
console.log(typeof a) //symbol，a并非对象 a.hello(); //hello，有效
```

## 类型转换

- 显示类型转换：

```js
转化为字符串：String(a), a.toString()
转化为数字：Number(),parseInt(),parseFloat()
转化为布尔值：Boolean()

// parseInt()是返回指定位数的整数或者是NaN
// 全局属性 NaN 的值表示不是一个数字（Not-A-Number）。
// 编码中很少直接使用到 NaN。通常都是在计算失败时，作为 Math 的某个方法的返回值出现的（例如：Math.sqrt(-1)）或者尝试将一个字符串解析成数字但失败了的时候（例如：parseInt("blabla")）。
// typeof NaN是number类型，但是判断NaN要用isNaN()或者Number.isNaN()。isNaN()和Number.isNaN()的区别是后者必须是值为NaN才返回true
```

- 强制类型转换
