---
title: 面经-JS
date: 2022-08-01
categories: 面经
tags: [面经,js]

---

## 1.数据类型

JS有==6==个基本类型，分别为String；Number；Boolean；Undefined；Symbol；null；

引用类型主要包括：Object；function；Array；Set；...

<br/>

## 2.数组的常用方法

增：push;splice;unshift;concat; 其中caocat产生新数组

删：pop;shift;splice;slice; 其中slice产生新数组

改：splice

查：find；indexOf；includes

排序：reverse;sort

转换：join

迭代：for...of...;forEach;some;every;map;filter;     参数基本都为(item,index,array)

<br/>

## 3.字符串常用方法

增：+；concat；

删：slice;substr;substring;substring参数为(strat,end)

改：trim;trimLeft;trimRight;(删除前后字符串) repeat(x)(重复x编后输出);padStart(n,chr)(如果长度小于n，则在一边填充字符);toLowerCase;toUpperCase;

查：charAt；indexOf；startWith；includes

转换：split

<br/>

## 4.类型转换

常见显示转换 Number;parseInt;String;Boolean

<br/>

## 5.==与\=\==与Object.is()

有\=\=、\=\=\=和Object.is()三种方法来判断两个变量是否相等
\=\=判断前会将不同类型的变量转换为同一类型再进行比较，在比较不同类型时，会将其转换为数字之后再进行比较；字符串比较时若不能转为数字，则转为NaN，空字符串会转为0；布尔值比较时，会将true转为1，false转为0；null和undefined与数字比较皆返回false；null和undefined比较时返回true
\=\=\=判断前会判断两个变量的类型，类型不一样直接返回false
Object.is()的行为与\=\=\=基本类似，但有两个例外，使用Object.is()比较+0和-0时会输出false，比较NaN和NaN时会输出true

<br/>

## 6.浅拷贝与深拷贝

浅拷贝只拷贝一层，深层有引用则不会进一步拷贝。Object.assign；concat；slice；和[...arr]都存在此现象。

深拷贝全部进行复制，主要方法有JSON.stringfy；手写递归循环

<br/>

## 7.闭包

闭包指的是一个函数和对其周围状态的引用绑定在一起形成的组合。主要作用包括：创建私有变量和延长变量生命周期

<br/>

## 8.作用域链

作用域指的是变量和函数生效的区域或集合。

JS中使用一个变量时会依次在当前作用域，上级作用域，依次向上直到全局作用域来寻找该变量。寻找所经过的作用域称为该变量的作用域链

<br/>

## 9.原型和原型链

构造函数有prototype属性指向其原型对象，原型对象有constructor属性指向构造函数，而每个对象都有\_\_proto\_\_ 属性指向其原型对象。访问一个对象的属性时，不仅在该对象上查找，还在它的原型上查找，甚至原型的原型，一次类推，查找过程中，经过的对象就是其原型链。

<br/>

## 10.JS中实现继承

1.原型链继承

将父类型实例设为子类型的原型

```javascript
function Parent() {
  this.name = 'parent1';
  this.play = [1, 2, 3]
}
function Child() {
  this.type = 'child2';
}
Child1.prototype = new Parent();
console.log(new Child())
```

缺点，子类实例无法共享属性

2.构造函数继承

在子类构造函数中调用父类构造函数

```javascript
function Parent(){
    this.name = 'parent1';
}

Parent.prototype.getName = function () {
    return this.name;
}

function Child(){
    Parent1.call(this);
    this.type = 'child'
}

let child = new Child();
console.log(child);  // 没问题
console.log(child.getName());  // 会报错
```

缺点：无法继承父类方法

3.组合式继承

综合了原型继承和构造函数继承

```
function Parent3 () {
    this.name = 'parent3';
    this.play = [1, 2, 3];
}

Parent3.prototype.getName = function () {
    return this.name;
}
function Child3() {
    // 第二次调用 Parent3()
    Parent3.call(this);
    this.type = 'child3';
}

// 第一次调用 Parent3()
Child3.prototype = new Parent3();
// 手动挂上构造器，指向自己的构造函数
Child3.prototype.constructor = Child3;
var s3 = new Child3();
var s4 = new Child3();
s3.play.push(4);
console.log(s3.play, s4.play);  // 不互相影响
console.log(s3.getName()); // 正常输出'parent3'
console.log(s4.getName()); // 正常输出'parent3'
```

缺点：父类构造函数被调用了两次(一次继承属性，一次继承方法)，属性也被继承了两次，只不过构造函数继承的属性屏蔽了原型链继承的属性

4.原型式继承

借助了Object.create(father) 该方法创建一个对象，让该对象以father对象为原型，继承了父对象的属性和方法，且不会调用父对象的构造函数。

```javascript
let parent4 = {
    name: "parent4",
    friends: ["p1", "p2", "p3"],
    getName: function() {
      return this.name;
    }
  };

let person4 = Object.create(parent4);
person4.name = "tom";
person4.friends.push("jerry");

let person5 = Object.create(parent4);
person5.friends.push("lucy");

console.log(person4.name); // tom
console.log(person4.name === person4.getName()); // true
console.log(person5.name); // parent4
console.log(person4.friends); // ["p1", "p2", "p3","jerry","lucy"]
console.log(person5.friends); // ["p1", "p2", "p3","jerry","lucy"]
```

缺点：Object.create是浅拷贝，父对象上的引用对象会在子对象中共享

5.寄生式继承

是原型式继承的改进版，寄生式继承在子对象继承后，又给子对象添加了一些方法

```javascript
let parent5 = {
    name: "parent5",
    friends: ["p1", "p2", "p3"],
    getName: function() {
        return this.name;
    }
};

function clone(original) {
    let clone = Object.create(original);
    clone.getFriends = function() {
        return this.friends;
    };
    return clone;
}

let person5 = clone(parent5);

console.log(person5.getName()); // parent5
console.log(person5.getFriends()); // ["p1", "p2", "p3"]
```

缺点与原型式继承一样，存在浅拷贝问题

6.寄生组合式继承

是组合式继承与寄生式继承的组合版本，是目前最好的继承方案

```javascript
function clone (parent, child) {
    // 这里改用 Object.create 就可以减少组合继承中多进行一次构造的过程
    child.prototype = Object.create(parent.prototype);
    child.prototype.constructor = child;
}

function Parent6() {
    this.name = 'parent6';
    this.play = [1, 2, 3];
}
Parent6.prototype.getName = function () {
    return this.name;
}
function Child6() {
    Parent6.call(this);
    this.friends = 'child5';
}

clone(Parent6, Child6);

Child6.prototype.getFriends = function () {
    return this.friends;
}

let person6 = new Child6();
console.log(person6); //{friends:"child5",name:"child5",play:[1,2,3],__proto__:Parent6}
console.log(person6.getName()); // parent6
console.log(person6.getFriends()); // child5
```

ES6中的class就是寄生组合式继承的语法糖。

<br/>

## 11.关于this对象的理解

this 关键字是函数运行时自动生成的一个内部对象，只能在函数内部使用，总指向调用它的对象。

默认绑定：全局环境下定义的this指向window

隐式绑定：函数作为某个对象的方法调用时，this就指向该对象

new绑定：使用new创建实例对象时，this指向这个实例对象

显示修改：使用apply，bind，call显示修改this指向。

箭头函数，不绑定this，箭头函数中的this由它所在的==函数==上下文决定(只有函数中才存在this)

## 12.事件模型

事件流会经历三个阶段：捕获阶段，处于目标阶段，冒泡阶段

事件模型分为两种：原始事件模型(DOM0级)和标准事件模型(DOM2级)

原始事件模型：

原始事件模型的监听函数绑定方式为：

```javascript
<input type="button" onclick="fun()">
```

优点：速度快，简单

缺点：只支持冒泡，不支持捕获，同一类型的事件只能绑定一次

标准事件模型：

标准事件模型的监听函数绑定方式为：

```javascript
addEventListener(eventType, handler, useCapture)
```

useCapture为true时在捕获阶段处理 ，反之在冒泡阶段处理。可以绑定多个事件监听函数。

## 13.typeof和instanceof的区别

typeof可以识别出6个基本类型（识别null时输出object，小bug），对于引用类型，除了函数可以识别为function外，其余都识别为object。可以使用typeof a！=‘undefined’来判断变量是否定义。

instanceof用于判断一个对象是否是一个构造函数的实例，使用方法为

```javascript
object instanceof constructor
```

返回一个布尔值。使用instanceof可以判断复杂引用数据类型

## 14.new操作符到底干了什么

- 创建一个新的对象obj
- 将对象与构造函数通过原型链连接起来
- 将构造函数中的this绑定到新建对象obj上
- 根据构造函数的返回类型判断，如果是原始值则被忽略(输出this)，如果返回对象则正常处理。

## 15.bind，call，apply的区别，如何实现bind

apply接收两个参数，第一个是要绑定的对象，第二个是函数的参数数组

call接受n个参数，第一个是要绑定的对象，后面的参数则是函数的参数

bind和call类似，但返回一个新的函数，该函数一直绑定obj。

bind的实现方式为记录了绑定的对象，输出一个使用call改变绑定的函数，利用了闭包。

## 16.事件循环

JS是单线程语言。任务分为同步任务和异步任务，执行时，遇到异步任务就把异步任务放入队列，遇到同步任务则执行，同步任务执行完成后，再去异步任务队列中执行异步任务。异步任务又分为了宏任务与微任务，每次执行时先执行完所有微任务，再去执行一个宏任务，宏任务执行完后，再去执行因此产生的所有微任务，以此类推。

常见微任务包括：

Promise.then;process.nextTick;MutationObserver

常见宏任务包括：

script(外层同步代码)

setTimeout/setInterval

UI rendering

I/O 

await也会阻塞代码(将后面的代码放入微任务队列，但await 同行后面的代码正常执行)

## 17.尾递归

在递归的基础上只在函数返回值调用自身，可以减少函数调用栈

## 18.JS中的内存泄漏

JS垃圾回收机制的两种实现方式：标记清除，引用计数

标记清除：变量进入环境时标记为“进入环境”，离开时标记为离开环境，定期清除被标记为离开环境的变量

引用计数：统计每个对象的被引用次数，若无一个对象无其他引用，则可以将其清除，可能有循环引用的问题。

可能导致内存泄漏的原因有：

- 意外的全局变量
- 闭包
- 没有清理对Dom的引用，事件监听函数，在不监听时要取消监听。

## JS本地存储的方式

主要有三种本地存储的方式：cookie，sessionStorage，localStorage

cookie主要用来辨别用户身份，解决http无状态导致的一些问题。它由名称(name)-值(value)对组成，还有一些额外的用于用于控制有效期、安全性、使用范围等数据。它在每次请求中都会发送。其生命周期由其自身决定。

localStorage是HTML5的新方法。其存储的信息在同一域中是共享的，若不主动删除，不会过期。只能存字符串不能存对象。

sessionStorage与localStorage基本一致，唯一不同的是生命周期，一旦页面关闭，sessionStorage会删除数据。

## 柯里化

柯里化是把一个多参数函数转化成一个嵌套的一元函数的过程

## 防抖和节流

节流指的是一段时间内重复触发，只有一次生效

防抖指的是x秒后再执行该事件，若x秒内被反复触发，则重新计时

节流：

```javascript
function throttled2(fn, delay = 500) {
    let timer = null
    return function (...args) {
        if (!timer) {
            timer = setTimeout(() => {
                fn.apply(this, args)
                timer = null
            }, delay);
        }
    }
}
```

防抖

```javascript
function debounce(fn,wait){
  let timer;
  return function(...args){
    clearTimer(timer);
    setTimeout(()=>{
      fn.apply(this,args);
    },wait)
  }
}
```

## 防止外部JS文件阻塞HTML解析的方法

使用async和defer属性来防止外部JS文件阻塞HTML的解析。

这两个的区别在于，async会在外部JS文件下载完成后立刻进行JS脚本的执行，执行的时机不可确定，而defer即使在外部JS文件下载完成后，也会等待HTML解析完成后，再进行JS脚本的执行

## JS是单线程的还是多线程的

JS是一门单线程的语言。虽然使用Web Workers API可以实现JS多线程并行运行，但它的本质是利用了浏览器的多线程框架，创建了多个JS引擎实例，定义了一种引擎间的通讯方法来实现的多线程。而JS的单线程指的是在一个JS引擎中执行JS脚本，引擎只会分配一个线程给他。
