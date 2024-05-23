---
title: 面经-ES6
date: 2022-08-01
categories: 面经
tags: [面经,js]

---

## 1.变量

let 和 const 都是ES6新增的两种命令，其和var的不同之处在于：（1）var存在变量提升，即var可以在声明前调用，值为undefined （2）let和const存在暂时性死区，即在声明之后才能使用变量 （3）let和const有块级作用域，var只有函数级作用域 （4）var允许重复声明，let和const在同一作用域中不允许重复声明。

实战中能用const就用const，其次是let，尽量不用var

<br/>

## 2.数组扩展

- 新增了扩展运算符...
- 新增了Array.from()和Array.of()两种数组构造方法
- 新增了find(),findIndex(),includes(),flat(),fill(),values(),keys(),entries()等方法

<br/>

## 3.对象扩展

- 属性简写
- 属性表达式  obj["attr1"]
- super关键字指向了当前对象的原型对象
- 属性遍历 for..in...    Object.keys(obj)
- 新增对象方法 Object.is(a,b) ; Object.assign(a,b,c) ; Object.getProtoTypeOf(obj) ; Object.keys(obj) ; Object.values(obj) ; Object.entries(obj)

<br/>

## 4.函数扩展

- 参数 可设置函数的默认参数；可使用扩展运算符定义参数
- 属性 新增了length属性 返回未指定默认值的参数个数；新增了name属性返回具名函数的名称
- 箭头函数 箭头函数中的this是定义时所在的对象，而不是使用时所在的对象

<br/>

## 5.Set和Map

Set为集合数据结构，不含重复值，其增删改查为size;add;delete;has;clear;遍历方法为keys;values();entries()。Set的key值与value值一样。可用于数组/字符串去重及交并补集的计算

Map为键值对数据结构，用于根据键快速找到值。其增删改查为size;set;get;has;delete;clear。

<br/>

## 6.Promise

异步编程的解决方案，用于解决回调地狱问题。Promise只有三种状态pending(进行中)；fulfilled(已成功)；rejected(已失败)。

使用以下方式创建一个Promise对象

const promise = new Promise(function(resolve, reject) {});

Promise接收一个函数，函数有两个参数resolve和reject。resolve函数负责将状态从未完成变为已成功，rejected函数负责将状态从未完成变为已失败。

Promise构建出来的实例有以下方法

- .then是实例状态发生改变时的回调函数，第一个参数是resolved状态的回调函数，第二个参数是rejected状态的回调函数。返回一个新的Promise。
- .catch是.then(null,rejection)的别名，只用于处理发生错误时的回调函数
- .finally方法用于指定不管最终状态如何都会执行的操作

Promise构造函数有以下方法

.all 参数为一个Promise对象数组，返回一个新的Promise对象p，只有数组中的全部Promise都成功，p的状态才为成功，数组中的实例返回值组成数组传给p。只要数组中的实例有一个状态变为失败，则p的状态为失败，且第一个变为失败的实例将其返回值传给p.

.race 同样将多个Promise实例包装为一个，若有一个率先改变状态，则p的状态变为率先改变状态的实例的状态。

.allSettled 当所有参数实例都返回结果，包装实例才会结束

<br/>

## 7.Generator

迭代器，可以中断函数的执行

<br/>

## 8.Proxy

用于定义基本操作的自定义行为。Proxy用于创建一个对象的代理，从而实现基本操作的拦截和自定义（如属性查找、赋值、枚举、函数调用等）

使用以下方法创建一个Proxy对象：

var proxy = new Proxy(target, handler)

target表示所要拦截的目标对象（任何类型的对象，包括原生数组，函数，甚至另一个代理））

handler通常以函数作为属性的对象，各属性中的函数分别定义了在执行各种操作时代理 p 的行为。

handler的主要属性有

get(target,propKey,receiver)：拦截对象属性的读取
set(target,propKey,value,receiver)：拦截对象属性的设置
has(target,propKey)：拦截propKey in proxy的操作，返回一个布尔值

<br/>

## 9.Module

模块，（Module），是能够单独命名并独立地完成一定功能的程序语句的集合（即程序代码和数据结构的集合体）。

需要模块化的原因：代码封装、代码抽象、代码复用、代码管理。

常用的模块管理 CommonJS和ES6 Module。

CommonJS 使用`module.exports={a,b}`导出模块(假设模块定义在a.js)

使用`let {a,b}=require('./a.js')`加载模块

ES6 Module 使用`export a` 导出模块 (假设模块定义在a.js)

使用`import a from './a.js'`导入模块

主要区别为

- CommonJS为动态导入，ES6 Module为静态导入  CommonJS在运行时根据条件进行导入，ES6在代码的静态分析时就确定了导入的模块
- CommonJS为值拷贝，ES6 为值引用 
- CommonJS为单个导出，ES6为多个导出 CommonJS只能导出一个对象，ES6可以导出多个对象
- CommonJS为同步导入，ES6为异步导入 CommonJS在加载代码时，有模块需要导入，就会暂停执行，等待模块加载完成后再继续执行；ES6 Moudle是异步导入，也就是在代码运行过程中，加载某个模块时，不会暂停代码的执行，而是继续执行代码，代码加载完成后，再回调到导入模块的代码中

## 10.箭头函数与普通函数

1. 书写方法不同。箭头函数使用=>声明，而普通函数使用function声明
2. this指向不同，普通函数中的this指的是函数调用时的对象，箭头函数中的this继承自父级作用域中的this
3. 箭头函数不能作为构造函数，不能使用new关键字
4. 箭头函数不绑定arguments，可以使用剩余参数代替
5. 箭头函数不能使用call(),apply(),bind()方法改变this的指向
