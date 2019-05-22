# ECMAScript 6入门

## 第一章 ECMAScript 6 简介

## 第二章 let 和 const 命令

### 2.1 let 命令

#### 2.1.1 基本用法

```js
{
    let a = 10;     //let 声明的变量只在其所在代码块内有效
    var b = 1;
}
alert(a);   //error
alert(b);   //1
```
```js
for(let i = 0; i < 10; i ++) {
    //...
}
alert(i);   //error     //使用 var 输出 10
```
```js
var funcs = [];
for(let i = 0; i < 10; i ++) {  // 当前 i 只在本轮循环有效，每次循环 i 都是一个新的变量。 JavaScript 引擎内部会记住上一轮循环的值。
    funcs[i] = function() {
        alert(i);
    };
}
funcs[6]();     //6     //使用 var 输出 10
```
```js
for(let i = 0; i < 3; i ++) {
    let i = 'abc';
    alert(i);
}
//输出三次 abc ，没有报错，说明 函数内部的变量 i 和 循环变量 i 不在同一个作用域，而是有各自单独的作用域。
```

#### 2.1.2 不存在变量提升

```js
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;

// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
```

#### 2.1.3 暂时性死区


在代码块内，使用let命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）
```js
if (true) {
  // TDZ开始
  tmp = 'abc'; // ReferenceError
  console.log(tmp); // ReferenceError

  let tmp; // TDZ结束
  console.log(tmp); // undefined

  tmp = 123;
  console.log(tmp); // 123
}
```

#### 2.1.4 不允许重复声明
```js
// 报错
function func() {
    let a = 10;
    var a = 1;
}

// 报错
function func() {
    let a = 10;
    let a = 1;
}

function func(arg) {
    let arg;
}
func() // 报错

function func(arg) {
    {                 //加了代码块
        let arg;
    }
}
func() // 不报错
```

### 2.2 块级作用域

#### 2.2.1 为什么需要块级作用域

```js
var tmp = new Date();

function f() {
    //var tmp;      //内部的 tmp 变量提升的结果，此时还未定义
    console.log(tmp);
    if (false) {
        var tmp = 'hello world';
    }
}

f(); // undefined
```

#### 2.2.2 ES6 的块级作用域

let 实际上为JavaScript 新增了块级作用域。

ES6 允许块级作用域的任意嵌套。

```js
{{{{
    {
        let insane = 'Hello World';
        {let insane = 'Hello World'}    //内层作用域可以定义外层作用域的同名变量
    }
    console.log(insane); // 报错    外层作用域无法读取内层作用域的变量
}}}};

//不再需要立即执行匿名函数
// IIFE 写法
(function () {
    var tmp = ...;
    ...
}());

// 块级作用域写法
{
    let tmp = ...;
    ...
}
```

#### 2.2.3 块级作用域与函数声明

ES6 引入了块级作用域，明确允许在块级作用域之中声明函数。（ES5 只能在顶层作用域和函数作用域之中声明）但是考虑到环境导致的行为差异太大，应该**避免**在块级作用域内声明函数。如果确实需要，也应该写成函数表达式的形式，而不是函数声明语句。

### 2.3 const 命令

#### 2.3.1 基本用法

const 声明一个只读的常量。一旦声明，常量的值就不能改变。（修改报错）（声明必须立即初始化，不能之后赋值）

const 的作用域与 let 命令相同：只在声明所在的块级作用域内有效。

#### 2.3.2 本质

const 实际上保证的并不是变量的值不得改动，而是变量指向的那个内存地址不得改动。

对于简单类型的数据（数值、字符串、布尔值）而言，值就保存在变量指向的内存地址中，因此等同于常量。

```js
//但对于复合类型的数据（主要是对象和数组）而言，变量指向的内存地址保存的只是一个指针， const 只能保证这个指针是固定的，至于它指向的数据结构是不是可变的，这完全不能控制。
const foo = {};

// 如果真的想将对象冻结，应该使用 Object.freeze 方法。
// const foo = Object.freeze({});
//常规模式时，下面一行不起作用；
//严格模式时，该行会报错

// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop // 123

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError: "foo" is read-only
```

#### 2.3.3 ES6 声明变量的 6 种方法

- var 命令
- function 命令
- let 命令
- const 命令
- import 命令
- class 命令

### 2.4 顶层对象的属性

顶层对象在浏览器环境中指的是 window 对象，在 Node 环境中指的是 global 对象。

顶层对象的属性与全局变量相关，被认为是 JavaScript 语言中最大的设计败笔之一。
- 无法在编译时就提示变量未声明的错误，只有运行时才能知道
- 程序员很容易不知不觉地就创建全局变量（比如打字出错）
- 顶层对象的属性是到处都可以读写的，这非常不利于模块化编程

ES5
```js
window.a = 1;
a // 1

a = 2;
window.a // 2
```

ES6
```js
var a = 1;
window.a // 1               //全局变量 a 由 var 命令声明，所以它是顶层对象的属

let b = 1;  //就是用了 let 而已
window.b // undefined       //全局变量 b 由 let 命令声明，所以它不是顶层对象的属性
```

### 2.5 global 对象

ES5 的顶层对象本身也是一个问题，因为它在各种实现中是不统一的。很难找到一种方法可以在所有情况下都取到顶层对象。以下勉强使用：
```js
var getGlobal = function () {
  if (typeof self !== 'undefined') { return self; }
  if (typeof window !== 'undefined') { return window; }
  if (typeof global !== 'undefined') { return global; }
  throw new Error('unable to locate global object');
};
```

现在有一个提案，在语言标准的层面引入 global 作为顶层对象。也就是说，在所有环境下， global 都是存在的，都可以拿到顶层对象。

垫片库 system.global 模拟了这个提案，可以在所有环境下拿到 global 。