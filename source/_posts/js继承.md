---
title: js继承
date: 2019-09-10 22:02:51
categories: 
  - js
  - 面试
tags: 
  - js
  - 面试
---

## 一、原型链继承

```
function Parent(name) {
  this.name = name;
}
Parent.prototype.sayName = function() {
  console.log('parent name:', this.name);
}
function Child(name) {
    this.name = name;
}

Child.prototype = new Parent('father');
Child.prototype.constructor = Child;

Child.prototype.sayName = function() {
    console.log('child name:', this.name);
}

var child = new Child('son');
child.sayName();    // child name: son

```

只要是原型链中出现过的原型，都可以说是该原型链派生的实例的原型。

这种方式方式的优缺点：

* 优点：正确设置原型链实现继承
* 优点：父类实例属性得到继承，原型链查找效率提高，也能为一些属性提供合理的默认值。

* 缺点：父类实例属性为引用类型时，不恰当地修改会导致所有子类被修改。例如：Child.prototype.sayName必须写在Child.prototype = new Parent('father')之后，不然就会被覆盖掉。
* 缺点：子类型无法给超类型传递参数，在面向对象的继承中，我们总希望通过var child = new Child('son', 'father')；让子类去调用父类的构造器来完成继承。而不是通过像这样new Parent('father')去调用父类。

## 类式继承（借用构造函数继承）

```
function Parent(name) { 
    this.name = name;
}
Parent.prototype.sayName = function() {
    console.log('parent name:', this.name);
}
Parent.prototype.doSomthing = function() {
    console.log('parent do something!');
}
function Child(name, parentName) {
    Parent.call(this, parentName);
    this.name = name;
}

Child.prototype.sayName = function() {
    console.log('child name:', this.name);
}

var child = new Child('son');
child.sayName();      // child name: son
child.doSomthing();   // TypeError: child.doSomthing is not a function

```

相当于Parent这个函数在Child函数中执行了一遍，并且将所有与this绑定的变量都切换到了Child上，这样就克服了第一种方式带来的问题。

这种方式的特点：

* 只继承了父类构造函数的属性，没有继承父类原型的属性。
* 可以继承多个构造函数属性（call多个）。
* 在子实例中可向父实例传参。

* 缺点：只能继承父类构造函数的属性。
* 没有原型，每次创建一个 Child 实例对象时候都需要执行一遍 Parent 函数，无法复用一些公用函数。

## 组合式继承： 前两种方式的结合

```
function Parent(name) { 
    this.name = name;
}

Parent.prototype.sayName = function() {
    console.log('parent name:', this.name);
}
Parent.prototype.doSomething = function() {
    console.log('parent do something!');
}
function Child(name, parentName) {
    Parent.call(this, parentName);
    this.name = name;
}

Child.prototype = new Parent();      
Child.prototype.constructor = Child;
Child.prototype.sayName = function() {
    console.log('child name:', this.name);
}

var child = new Child('son');
child.sayName();       // child name: son
child.doSomething();   // parent do something!

```

组合式继承是比较常用的一种继承方法，其背后的思路是使用原型链实现对原型属性和方法的继承，而通过借用构造函数来实现对实例属性的继承。

这样，既通过在原型上定义方法实现了函数复用，又保证每个实例都有它自己的属性。

###### 组合式继承是 JS 最常用的继承模式，但组合继承使用过程中会被调用两次：一次是创建子类型的时候，另一次是在子类型构造函数的内部。

```
function Parent(name) { 
    this.name = name;
}
Parent.prototype.sayName = function() {
    console.log('parent name:', this.name);
}
Parent.prototype.doSomething = function() {
    console.log('parent do something!');
}
function Child(name, parentName) {
    Parent.call(this, parentName);      // 第二次调用
    this.name = name;
}

Child.prototype = new Parent();         // 第一次调用
Child.prototype.constructor = Child;
Child.prototype.sayName = function() {
    console.log('child name:', this.name);
}

var child = new Child('son');
child.sayName();      
child.doSomething();
```

显然从上述的代码中可以看出，第一次调用构造函数显然是没有必要的，因为第一次调用构造函数时候不需要函数内部的那些实例属性，这么写只是想获得其原型上的方法罢了，所以这时候你可能会这样写：
```
Child.prototype = Parent.prototype;
```

这样写显然是不对的：
1、首先，你这样写的话相当于是子类和父类都指向同一个对象，这时候如果你添加了新的方法给 Child 但实际上 Parent 并不需要，相当于强行给 Parent 添加了一个未知的方法。
2、其次，仔细想想，这样体现不出继承的多态性，比如此时子类想要重写父类的 getName 的方法，那么父类的方法也就会随之修改，这显然违背了多态性。

也就是说我们第一次调用构造函数的时候，其实是不管构造函数里面的内容，所以我们何不 new 一个空函数，将其 prototype 指向 Parent.prototype，代码如下：

## 四、继承组合式继承

```
function Parent(name) {
    this.name = name;
}
Parent.prototype.sayName = function() {
    console.log('parent name:', this.name);
}

function Child(name, parentName) {
    Parent.call(this, parentName);  
    this.name = name;    
}

function create(proto) {
    function F(){}
    F.prototype = proto;
    return new F();
}

Child.prototype = create(Parent.prototype);
Child.prototype.sayName = function() {
    console.log('child name:', this.name);
}
Child.prototype.constructor = Child;

var parent = new Parent('father');
parent.sayName();    // parent name: father


var child = new Child('son', 'father');
child.sayName();     // child name: son

```

这就是所谓的寄生组合式继承方式，跟组合式继承的区别在于，他不需要在一次实例中调用两次父类的构造函数，假如说父类的构造器代码很多，还需要调用两次的话对系统肯定会有影响，寄生组合式继承的思想在于：用一个 F 空的构造函数去取代执行了 Parent 这个构造函数。

在上面的代码中，我们手动创建了一个 create 函数，但是其实是存在于 Object 对象中，不需要我们手动去创建，所以上面的代码可以改为：
```
function Parent(name) {
    this.name = name;
}
Parent.prototype.sayName = function() {
    console.log('parent name:', this.name);
}

function Child(name, parentName) {
    Parent.call(this, parentName);  
    this.name = name;    
}

function inheritPrototype(Parent, Child) {
    Child.prototype = Object.create(Parent.prototype);   //修改
    Child.prototype.constructor = Child;
}

inheritPrototype(Parent, Child);

Child.prototype.sayName = function() {
    console.log('child name:', this.name);
}

var parent = new Parent('father');
parent.sayName();      // parent name: father

var child = new Child('son', 'father');
child.sayName();       // child name: son

```

## 五、ES6继承：

将上面代码改为：

```
class Parent {
    constructor(name) {
	this.name = name;
    }
    doSomething() {
	console.log('parent do something!');
    }
    sayName() {
	console.log('parent name:', this.name);
    }
}

class Child extends Parent {
    constructor(name, parentName) {
	super(parentName);
	this.name = name;
    }
    sayName() {
 	console.log('child name:', this.name);
    }
}

const child = new Child('son', 'father');
child.sayName();            // child name: son
child.doSomething();        // parent do something!

const parent = new Parent('father');
parent.sayName();           // parent name: father

```


