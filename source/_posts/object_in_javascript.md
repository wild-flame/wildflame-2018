---
title: Javascript 里的面向对象编程
category: tech 
permalink: object_in_javascript
author: David Lin
tags: 
  - diary

---
# 前言

Javascript 在设计之初，并没有考虑到 OOP（面向对象编程），但是随着 node.js 以及前端工程化的发展，项目越来越大，为了项目维护的方便，也不得不考虑面向对象编程了。

<!-- more -->

# Javascript 里面向对象的实现有哪几种方法？

Javascript 的面向对象的实现一共有三种。

1. 构造函数的方法
2. Object.create 的方法
3. 极简主义的实现

我们先来看第一种方法。

## 1.通过构造函数来实现面向对象的编程

首先我们来看看如何使用构造函数的方法来实现。

``` javascript
function Person(name, sex) {
    this.name = name
    this.sex = sex
}
```

类函数的定义

``` javascript
Person.prototype.say = function() {
    console.log("my name is " + this.name, ",And I am " + this.sex)
}
```

我们打印出来 Person 来看一下

``` javascript
> Person
[Function: Person]
```

接下来我们新建一个 Person 的实例 boss。

``` javascript
boss = new Person("David", "male")
```

然后我们打印出 boss 来看一下

``` javascript
> boss
Person { name: 'David', sex: 'male' }
```

总结：**boss 是一个对象，Person 是一个函数 (类的构造函数)，如果想要定义类函数，定义在 Person.prototype 上面。**

## 2.通过 Object.create 方法来创造对象。

``` javascript
const person = {
  isHuman: false,
  say: function () {
    console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
  }
};

const me = Object.create(person);

me.name = "David"; // "name" is a property set on "me", but not on "person"
me.isHuman = true; // inherited properties can be overwritten

me.say();
// expected output: "My name is David. Am I human? true"
```

我们打印出 person 和 me 来看一下。

``` javascript
> person
{ isHuman: false, say: [Function: say] }

> me
{ name: 'David', isHuman: true }
```

对比前一种方法，我们发现方法二是通过一个对象来构建一个对象。

### 深入理解 Object.create

如果了解 javascript 的原型链的话，我们可以更深入的研究下去：

``` javascript
> me.__proto__ 
{ isHuman: false, say: [Function: say] }

> me.__proto__ === person
true
```

所以实际上 `me = Object.create(person)` 就是将一个新的对象 `me` 的 `__proto__` 设置成 `person`。

### 总结: 
- me 是一个对象，person 也是一个对象，如果想要定义类函数，定义在 person 上面。
- me 和 person 都是不存在 prototype 属性，但他们都存在 constructor 为 Object（即 Object 构造函数）
- `Object.create` 实际上就是在原型链上加上 `__proto__` 属性。

## 3.极简主义的实现

荷兰程序员 Gabor de Mooij 提出了一种比 Object.create() 更好的新方法，他称这种方法为"极简主义法"（minimalist approach）。

### 封装

这种方法不使用this和prototype，代码部署起来非常简单，这大概也是它被叫做"极简主义法"的原因。

首先，跟方法二一样，它也是用一个对象模拟"类"。在这个类里面，定义一个构造函数`createNew()`，用来生成实例。

``` javascript
var Cat = {
　　createNew: function(){
　　　　// some code here
　　}
};
```

然后，在createNew()里面，定义一个实例对象，把这个实例对象作为返回值。

``` javascript
var Cat = {
　　createNew: function(){
　　　　var cat = {};
       cat.name = "大毛";
       cat.makeSound = function(){ alert("喵喵喵"); };
       return cat;
　　}
};
```

使用的时候，调用createNew()方法，就可以得到实例对象。

``` javascript
var cat1 = Cat.createNew();
cat1.makeSound(); // 喵喵喵
```

这种方法的好处是，容易理解，结构清晰优雅，符合传统的"面向对象编程"的构造，因此可以方便地部署下面的特性。

### 继承

让一个类继承另一个类，实现起来很方便。只要在前者的createNew()方法中，调用后者的createNew()方法即可。

先定义一个Animal类。

``` javascript
var Animal = {
　　createNew: function(){
　　　　var animal = {};
　　　　animal.sleep = function(){ alert("睡懒觉"); };
　　　　return animal;
　　}
};
```

然后，在Cat的createNew()方法中，调用Animal的createNew()方法。

``` javascript
var Cat = {
　　createNew: function(){
　　　　var cat = Animal.createNew();
　　　　cat.name = "大毛";
　　　　cat.makeSound = function(){ alert("喵喵喵"); };
　　　　return cat;
　　}
};
```

这样得到的Cat实例，就会同时继承Cat类和Animal类。

``` javascript
var cat1 = Cat.createNew();
cat1.sleep(); // 睡懒觉
```

### 私有属性和私有方法

在createNew()方法中，只要不是定义在cat对象上的方法和属性，都是私有的。

``` javascript
var Cat = {
　　createNew: function(){
　　　　var cat = {};
　　　　var sound = "喵喵喵";
　　　　cat.makeSound = function(){ alert(sound); };
　　　　return cat;
　　}
};
```

上例的内部变量sound，外部无法读取，只有通过cat的公有方法makeSound()来读取。

``` javascript
var cat1 = Cat.createNew();
alert(cat1.sound); // undefined
```

### 数据共享

有时候，我们需要所有实例对象，能够读写同一项内部数据。这个时候，只要把这个内部数据，封装在类对象的里面、createNew()方法的外面即可。

``` javascript
var Cat = {
　　sound : "喵喵喵",
　　createNew: function(){
　　　　var cat = {};
　　　　cat.makeSound = function(){ alert(Cat.sound); };
　　　　cat.changeSound = function(x){ Cat.sound = x; };
　　　　return cat;
　　}
};
```

然后，生成两个实例对象：

``` javascript
var cat1 = Cat.createNew();
var cat2 = Cat.createNew();
cat1.makeSound(); // 喵喵喵
```

这时，如果有一个实例对象，修改了共享的数据，另一个实例对象也会受到影响。

``` javascript
cat2.changeSound("啦啦啦");
cat1.makeSound(); // 啦啦啦
```


# Javascript 的继承的实现？

Javascript 的继承是通过原型链来实现的。

总结：

- **定理1：每个对象都有 __proto__ 属性，但只有函数对象才有 prototype 属性**

# 后记

因为成都的空气实在是太差了，所以这段时间搬到了青城山来居住。

身体还是常常不适，现在严重怀疑是神经官能症。（如果不是的话，那我也不知道是啥了。）

半吊子如我又重新开始写代码了，这段时间除了给 Howard 那边的 [KeyMesh](http://keymesh.io/) 做运营、翻译白皮书、宣言以外，还在着手写 node.js 上的一个机器人的框架，基于 ccxt 的。 

虽然我很菜，但是写代码很好玩呀，然后我打算要好好温习一下 Javascript 的基础知识，所以有了这篇文章。

在 ES6 里，Javascript 可以用 Class 来写了，但是这样掩盖了其底层的实现方法，不太方便用户了解其更底层的原理。其次 Javascript 的原型链也曾经是一个很重要的概念。虽然 ES6 引入了很多的语法糖。但由于历史的原因，现在网络上还有大量的历史代码。所以了解一些底层原理是有利而无一害的。

## 参考

- [Javscript 定义类的三种方法](http://www.ruanyifeng.com/blog/2012/07/three_ways_to_define_a_javascript_class.html)
- [Mozilla 上关于 Object.create 的介绍](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create)
- [最详尽的 JS 原型与原型链终极详解](https://www.jianshu.com/p/dee9f8b14771)
