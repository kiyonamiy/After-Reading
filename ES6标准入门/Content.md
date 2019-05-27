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
  // TDZ 开始
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

## 第三章 变量的解构赋值

### 3.1 数组的解构赋值

#### 3.1.1 基本用法

ES6 允许按照一定模式从数组和对象中提取值，然后对变量进行赋值，这被称为解构（ Destructuring ）。
```js

/**
*以前，为变量赋值，只能直接指定值。
**/
let a = 1;
let b = 2;
let c = 3;

/**
*ES6 允许写成下面这样。
**/
let [a, b, c] = [1, 2, 3];

let [foo, [[bar], baz]] = [1, [[2], 3]];

let [ , , third] = ["foo", "bar", "baz"];     // third = "baz"

let [x, , y] = [1, 2, 3];       //x = 1, y = 3

let [x, y, ...z] = ['a'];       //x = "a", y = undefined, z = []

let [x, y] = [1, 2, 3];         //不完全解构 x = 1, y = 2


/**
*如果解构不成功，变量的值就等于undefined。
**/
let [foo] = [];         //foo = undefined
let [bar, foo] = [1];   //bar = 1, foo = undefined


/**
*如果等号的右边不是数组（或者严格来说不是可遍历的结构， 参见第 15 章），那么将会报错。
**/
//因为等号右边的值或是转为对象以后不具备 Iterator 接口（前两个表达式），或是本身就不具备 Iterator 接口（最后一个表达式〕。
let [foo] = 1;
let [foo] = null;
let [foo] = {};

/**
*对于Set 结构，也可以使用数组的解构赋值。
**/
let [x, y, z] = new Set([1, 2, 3]);     //x = 1

/**
*事实上，只要某种数据结构具有Iterator 接口，都可以采用数组形式的解构赋值。
**/
//fibs 是一个 Generator 函数（参见第 16 章），原生具有Iterator 接口
function* fibs() {
    let [a, b] = [0, 1];
    while(true) {
        yield a;
        [a, b] = [b, a + b];
    }
}

let [first, second, third, fourth, fifth, sixth] = fibs();      // sixth = 5
```

#### 3.1.2 默认值

解构赋值允许指定默认值。

```js
let [foo = true] = [];     //foo = true

let [x, y = 'b'] = ['a'];               //x = 'a', y = 'b'
let [x, y = 'b'] = ['a', undefined];    //x = 'a', y = 'b'      //ES6 内部使用严格相等运算符（===）判断一个位直是否有值
let [x = 1] = [null];       //x = null      //，默认值就不会生效，因为null 不严格等于 undefined 。

/**
*默认值可以引用解构赋值的其他变量，但该变量必须己经声明。
**/
let [x = 1, y = x] = [];        //x = 1, y = 1
let [x = 1, y = x] = [2];       //x = 2, y = 2
let [x = y, y = 1] = [];        //ReferenceError x 用到默认值 y 时， y还没有声明。
```

### 3.2 对象的解构赋值

解构不仅可以用于数组，还可以用于对象。

```js
/**
* 对象的属性没有次序，变量必须与属性同名才能取到正确的值。
**/
let {bar, foo} = {foo: "QQ", bar: 12};     //foo = "QQ", bar = 12   //次序完全没有影响

let {a, b} = {foo: "QQ", bar: 12};         //a = undefined, b = undefined

//如果解构模式是嵌套的对象，而且子对象所在的父属性不存在，那么将会报错。
let {foo: {bar}} = {baz: 'baz'};    //报错

/**
* 如果变量名与属性名不一致，必须写成下面这样。
**/
var {foo: baz} = {foo: 'aa', bar: 'bb'};    //baz = 'aa'

/**
* 实际上说明，对象的解构赋值是下面形式的简写（参见第 9 章）。
**/
//也就是说，对象的解构赋值的内部机制是先找到同名属性，然后再赋值给对应的变量。
//真正被赋值的是后者，而不是前者。
let {foo: foo, bar: bar} = {foo: "aaa", bar: "bbb"};        // foo = "aaa", bar = "bbb"

//foo 是匹配的模式， baz 才是变量。真正被赋值的是变量 baz ，而不是模式 foo 。
let {foo: baz} = {foo: "AA", bar: "BB"};                    // baz = "AA"

/**
* 与数组一样，解构也可以用于嵌套结构的对象
**/
//其实就是照着格式写，把值的地方替换为变量
let obj = {
    p: [
        'Hello',
        {y: 'World'}
    ]
};
let {p: [a, {y: b}]} = obj;     //a = 'Hello', b = 'World'
//
var node = {
    loc: {
        start: {
            line: 1,
            column: 5
        }
    }
};
var {loc, loc: {start}, loc: {start: {line}}} = node;
//有三次解构赋值，分别是对 loc 、start 、line 三个属性的解构赋值。
alert(loc);     //[object Object]   {start: Object}
alert(start);   //[object Object]   {line: 1, column: 5}
alert(line);    //1
//
let obj = {};
let arr = [];
let {foo: obj.prop, bar: arr[0]} = {foo: "AA", bar: true};      //obj = {prop: "AAA"}, arr[0] = true

/**
* 对象的解构也可以指定默认值。（默认值生效的条件是，对象的属性值严格等于undefined 。
**/

var {x, y = 5} = {x: 1};    //x = 1, y = 5
var {x: y = 3} = {x: 5};    //y = 5
var {x = 3} = {x: undefined};   //x = 3


/**
* 如果要将一个已经声明的变量用于解构赋值，必须非常小心。
**/
//错误的写法
let x;
{x} = {x: 1};   //SyntaxError
//正确的写法
let x;
({x} = {x: 1}); //只有不将大括号写在行首，避免 JavaScript 将其解释为代码块， 才能解决这个问题

/**
* 由于数组本质是特殊的对象， 因此可以对数组进行对象属性的解构。
**/
let arr = [1, 2, 3];
let {0: first, [arr.length - 1]: last} = arr;   //first = 1, last = 3   方括号这种写法属于“属性名表达式”
```

### 3.3 字符串的解构赋值

字符串也可以解构赋值。这是因为此时字符串被转换成了一个类似数组的对象。

```js
const [a, b, c, d, e] = "hello";    //a = "h", b = "e", c = "l", d = "l", e = "o"
//类似数组的对象都有一个length 属性， 因此还可以对这个属性进行解构赋值。
let {length: len} = "hello";        //len = 5
```

### 3.4 数值和布尔值的解构赋值

```js
//解构赋值时， 如果等号右边是数值和布尔值，则会先转为对象。
let {toStirng: s} = 123;
s === Number.prototype.toString //true

let {toString: s} = true;
s === Boolean.prototype.toString // true

//由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错。
let { prop: x } = undefined; // TypeError
let { prop: y } = null; // TypeError
```

### 3.5 函数参数的解构赋值

函数的参数也可以使用解构赋值。
```js
//函数 add 的参数表面上是一个数组，但在传入参数的那一刻，数组参数就被解构成变量 x 和 y 。
function add([x, y]) {
    return x + y;
}
add([1, 2]);    //3

let mapResult = [[1, 2], [3, 4]].map(([a, b]) => a + b);  //mapResult = [3, 7];      //map 第一个参数为 item

/**
* 函数参数的解构也可以使用默认值。
**/
function move({x: 0, y: 0} = {}) {      //后面的 = {} 是在 move() 调用没有传递参数的默认参数
    return [x, y];
}
//注意，下面的写法会得到不一样的结果。
function anotherMove({x, y} = {x: 0, y: 0}) {       //这里的 x ， y 并没有设置默认值，只设置了一个默认参数
    return [x, y];
}
move({x: 3});   // [3, 0]
anotherMove({x: 3});    //[3, undefined]
```

### 3.6 圆括号问题

解构赋值虽然很方便，但是解析起来并不容易。对于编译器来说，一个式子到底是模式还是表达式，没有办法从一开始就知道，必须解析到（或解析不到）等号才能知道。

如果模式中出现圆括号该怎么处理？ ES6 的规则是，只要有可能导致解构的歧义，就不得使用圆括号。

但是，这条规则实际上不那么容易辨别， 处理起来相当麻烦。因此建议，只要有可能，就**不要**在模式中放置圆括号。


#### 3.6.1 不能使用圆括号的情况

1. 变量声明语句
```js
let [(a)] = [1];            //错误
```

2. 函数参数（也属于变量声明）
```js
function f([(z), x]) {
    return z;
}            //错误
```

3. 赋值语句的模式
```js
([a]) = [5];
```

#### 3.6.2 可以使用圆括号的情况

可以使用圆括号的情况只有一种： **赋值语句**的非模式部分可以使用圆括号。

```js
//因为它们都是赋值语旬，而不是声明语句，另外它们的圆括号都不属于模式的一部分。
[(b)] = [3];                //正确
({p: (d)} = {});            //正确
[(parseInt.prop)] = [3];    //正确
```

### 3.7 用途

1. 交换变量的值
```js
let x = 1, y = 2;
[x, y] = [y, x];
```

2. 从函数返回多个值
```js
function example() {
    return  [1, 2, 3];
}
let [a, b, c] = example();  //方便取值
```

3. 函数参数的定义
```js
function f([x, y, z] = [1, 2, 3]) {
    alert(z);
}
f([1, 2, 3]);
//f(1, 2, 3);   //错误，不能这样调用
//f();          //正确，提供默认参数
```

4. 提取 JSON 数据
```js
let jsonData = {
    id: 42,
    status: "OK",
    data: [867, 5309]
};

let {id, status, data: number} = jsonData;  //42, "OK", [867, 5309]
```

5. 函数参数的默认值

指定参数的默认值，这样就避免了在函数体内部再写`var foo = config.foo || 'default foo';`这样的语句

```js
let ajax = function (url, {
    async = true,
    beforeSend = function () {},
    cache = true,
    complete = function () {},
    crossDomain = false,
    global = true,
    // ... more config
}) {
    alert(async);
};
ajax({});
//ajax();   //报错 除非 那一大串后面 {......} = {} 提供默认参数
```

6. 遍历 Map 结构

任何部署了Iterator 接口的对象都可以用for ... of 循环遍历。Map 结构原生支持Iterator接口，配合变量的解构赋值获取键名和键值就非常方便。

```js
var map = new  Map();
map.set('first', 'hello');
map.set('second', 'world');

for(let [key, value] of map) {
    alert(key + " is " + value);
}
```

7. 输入模块的指定方法

加载模块时，往往需要指定输入的方法。解构赋值使得输入语句非常清晰。

```js
const {SourceMapConsumer, SourceNode} = require("source-map");
```

## 第四章 字符串的扩展（略）

### 4.1 字符的 Unicode 表示法

JavaScript 共有6 种方法可以表示一个字符。
```js

/**
* JavaScript 允许采用 \uxxxx 形式表示一个字符，其中 xxxx 表示字符的 Unicode 码点。但是，这种表示法只限于码点在\u0000~\uFFFF之间的字符
**/
// 1字节(Byte） = 8位(bit) 
"\u0061"    //"a"
/**
* 超出这个范围的字符，必须用 2 个双字节的形式表达。
**/
"\uD842\uDFB7"
// "𠮷"

/**
* 只要将码点放入大括号，就能正确解读该字符。
**/
"\u{20BB7}"
```

JavaScript 共有6 种方法可以表示一个字符。
```js
'\z' === 'z'  // true
'\172' === 'z' // true
'\x7A' === 'z' // true
'\u007A' === 'z' // true
'\u{7A}' === 'z' // true
```

### 4.2 codePointAt()

ES6 提供了 codePointAt 方法，能够正确处理4 个字节储存的字符，返回一个字符的码点。

### 4.3 String.fromCodePoint()

ES6 提供了String .fromCodePoint 方法，可以识别大于OxFFFF 的字符，弥补了String . fromCharCode 方法的不足。在作用上，正好与codePointAt 方法相反。

### 4.4 字符串的遍历器接口

ES6 为字符串添加了遍历器接口（详见第 15 章） ，使得字符串可以由 for ... of 循环遍历。

这个遍历器最大的优点是可以识别大于 0xFFFF 的码点，传统的 for 循环无法识别这样的码点（for(let i = 0; i < text.length; i ++) 无法做到）。

```js
for(let codePoint of 'foo') {
    console.log(codePoint);
}
// f
// o
// o
```

### 4.5 at()

可以识别 Unicode 编号大于 0xFFFF 的字符，返回正确的字符。
```js
'𠮷'.at(0);     //𠮷 
//'𠮷'.charAt(0);   //\uD842 无法显示 该方法不能识别码点大于 0xFFFF 的字符。
```

### 4.6 normalize()

许多欧洲语言有语调符号和重音符号。ES6 为字符串实例提供了 normalize 方法， 用来将字符的不同表示方法统一为同样的形式，这称为 Unicode 正规化。

### 4.7 includes() 、 startsWith() 、 endsWith()

- includes() 返回布尔值，表示是否找到了参数字符串。
- startsWith() 返回布尔值， 表示参数字符串是否在源字符串的头部。
- endsWith() 返回布尔值， 表示参数字符串是否在源字符串的尾部。

```js
var s = 'Hello world!';

s.startsWith('world', 6);   //true      从第 6 个字符开始
s.endsWith('Hello', 5);     //true      针对前 5 个字符
s.includes('Hello', 6);     //false     从第 6 个字符开始
```

### 4.8 repeat()

repeat 方法返回一个新字符串，表示将原字符串重复 n 次。
```js
'Hello'.repeat(2)   //'HelloHello'
'na'.repeat(1.9)    //'nana'    取整
```

### 4.9 padStart() 、 padEnd()

如果某个字符串不够指定长度，会在头部或尾部补全。 padStart() 用于头部补全， padEnd() 用于尾部补全。
```js
'x'.padStart(4, 'ab');  //'abax'

'xxx'.padStart(2, 'ab');    //'xxx' 如果原字符串的长度等于或大于指定的最小长度，则返回原字符串

'x'.padStart(4)     // '   x' 如果省略第二个参数， 则会用空格来补全。

'09-12'.padStart(10, 'YYYY-MM-DD')  // "YYYY-09-12"
```

### 4.10 模板字符串

传统的 JavaScript 输出模板通常是这样写的。
```js
$('#result').append(
    'There are <b>' + basket.count + '</b> ' +
    'items in your basket, ' +
    '<em>' + basket.onSale +
    '</em> are on sale!'
);
```

上面这种写法相当烦琐且不方便， ES6 引入了模板字符串来解决这个问题。
```js
$('#result').append(`
    There are <b>${basket.count}</b> items
     in your basket, <em>${basket.onSale}</em>
    are on sale!
`)
```

模板字符串（ template string ）是增强版的字符串，用反引号（ ` ）标识。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。

在模板字符串中嵌入变量，需要将变量名写在 $() 中。模板字符串中还能调用函数。

### 4.12 标签模板

模板字符可以紧跟在一个函数名后面， 该函数将被调用来处理这个模板字符串。这被称为“ 标签模板”功能（ tagged template ）。

标签模板其实不是模板，而是函数调用的一种特殊形式。“标签”指的就是函数，紧跟在后面的模板字符串就是它的参数。
```js
alert`123`;
//等同于
alert(123);
```

但是，如果模板字符中*有变量*，就不再是简单的调用了，而是要将模板字符串*先处理成多个参数*，再调用函数。
```js
//tag 函数的第一个参数是一个数组，该数组的成员是模板字符串中那些没有变量替换的部分
//tag 函数的其他参数都是模板字符串各个变量被替换后的值。（本里中包含两个变量）
function tag(stringArr, value1, value2) {
    alert(stringArr[0]);
    alert(stringArr[1]);
    alert(stringArr[2]);
    alert(value1);
    alert(value2);
}
var a = 5;
var b = 10;

tag`Hello ${a + b} world ${a * b}`;
//"Hello "
//" world "
//""
//15
//50

//等同于
tag(['Hello ', ' world', ''], 15, 50);
```

“标签模板”的一个重要应用就是过滤 HTML 字符串，防止用户输入恶意内容。

### 4.13 String.raw()

往往用来充当模板字符串的处理函数， 返回一个反斜线都被转义（即反斜线前面再加一个反斜线）的字符串，对应于替换变量后的模板字符串。
```js
String.raw`Hi\n${2+3}!`;
//"Hi\\n5!"     // \\表示\
```

### 4.14 模板字符串的限制

前面提到，标签模板中可以内嵌其他语言。但是，模板字符串默认会将字符串转义，导致无法嵌入其他语言。

## 第五章 正则的扩展

## 第六章 数值的扩展

### 6.1 二进制和八进制表示法

ES6 提供了二进制和八进制数值的新写法，分别用前缀 0b （ 或 0B ）和 0o （或 0O ）表示。（ binary octal ）
```js
0b111110111 === 503 // true
0o767 === 503       // true
```

如果要将 0b 和 0o 前缀的字符串数值转为十进制，要使用 Number 方法。
```js
Number('0b111')  // 7
Number('0o10')  // 8
```

### 6.2 Number.isFinite(), Number.isNaN() 

Number.isFinite() 用来检查一个数值是否为有限的（ finite ），即不是 Infinity 。

Number.isNaN() 用来检查一个值是否为 NaN 。

这两个新方法与传统的全局方法 isFinite() 和 isNaN() 的区别在于：
- 传统方法先调用 Number() 将非数值转为数值，再进行判断，
- 而新方法只对数值有效，对于非数值一律返回 false 。
```js
isFinite(25)            //true
isFinite("25")          //true
Number.isFinite(25)     //true
Number.isFinite("25")   //false

//Number.isNaN() 只有对于 NaN 才返回 true ，非 NaN 一律返回 false 。
isNaN(NaN)              //true
isNaN("NaN")            //true
Number.isNaN(NaN)       //true
Number.isNaN("NaN")     //false
Number.isNaN(1)         //false
```

### 6.3 Number.parseInt() 、 Number.parseFloat()

ES6 将全局方法 parseInt() 和 parseFloat() 移植到了 Number 对象上面，行为完全保持不变。

### 6.4 Number.isInteger()

Number.isInteger() 用来判断一个值是否为整数。 3 和 3.0 被视为同一个值。

#### 6.5 Number.EPSILON

新增的一个极小的常量。目的在于为浮点数计算设置一个误差范围。

```js
Number.EPSILON
// 2.220446049250313e-16
```

### 6.6 安全整数和 Number.isSafeInteger()

JavaScript 能够准确表示的整数范围在 -2^53 到 2^53 之间（不含两个端点），超过这个范围就无法精确表示。

```js
Math.pow(2, 53) // 9007199254740992

9007199254740992  // 9007199254740992
9007199254740993  // 9007199254740992   //超过了，无法精确表示

Math.pow(2, 53) === Math.pow(2, 53) + 1     //true
// true
```

ES6 引入了 Number.MAX_SAFE_INTEGER 和 Number.MIN_SAFE_INTEGER 两个常量，用来表示这个范围的上下限。
```js
Number.MAX_SAFE_INTEGER === Math.pow(2, 53) - 1
// true
Number.MAX_SAFE_INTEGER === 9007199254740991
// true
```

Number.isSafeInteger() 则是用来判断一个整数是否落在这个范围之内。实际使用这个函数时，需要注意验证运算结果是否落在安全整数的范围内，另外不要只验证运算结果，还要同时验证参与运算的每个值。
```js
Number.isSafeInteger(9007199254740993)
// false
Number.isSafeInteger(990)
// true
Number.isSafeInteger(9007199254740993 - 990)
// true
9007199254740993 - 990
// 返回结果 9007199254740002
// 正确答案应该是 9007199254740003
// 因为 9007199254740993 这个数超出了精度范围，导致在计算机内部以 9007199254740992 的形式储存。
```

### 6.7 Math 对象的扩展

ES6 在 Math 对象上新增了 17 个与数学相关的方法。所有这些方法都是静态方法，只能在 Math 对象上调用。

#### 6.7.1 Math.trunc()

Math.trunc 方法用于去除一个数的小数部分，返回整数部分。

```js
Math.trunc(-0.1234);     //-0

//对于非数值， Math.trunc 内部使用 Number 方法将其先转为数值。
Math.trunc("123.456");   //123

//对于空值和无法截取整数的值， 返回 NaN 。
Math.trunc("foo");      //NaN
```

#### 6.7.2 Math.sign()

Math.sign 方法用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。

它会返回五种值
- 参数为正数，返回 +1
- 参数为负数，返回 -1
- 参数为 0 ，返回 0
- 参数为 -0 ，返回 -0
- 其他值，返回 NaN

```js
Math.sign('')  // 0
Math.sign(true)  // +1
Math.sign(false)  // 0
Math.sign(null)  // 0
Math.sign('9')  // +1
Math.sign('foo')  // NaN
Math.sign()  // NaN
Math.sign(undefined)  // NaN
```

#### 6.7.3 Math.cbrt()

Math.cbrt 方法用于计算一个数的立方根。

```js
Math.cbrt(2)        //1 . 2599210498948734

//对于非数值， Math.cbrt 方法内部也是先使用 Number 方法将其转为数值。
Math.cbrt('8')      //2
Math.cbrt('Hello')  //NaN
```

#### 6.7.4 Math.clz32()

JavaScript 的整数使用 32 位二进制形式表示， Math.clz32 方法返回一个数的 32 位无符号整数形式有多少个前导 0 。

```js
Math.clz32(0)   //32
Math.clz32(1)   //31
```

#### 6.7.5 Math.imul()

Math.imul 方法返回两个数以 32 位带符号整数形式相乘的结果，返回的也是一个 32 位的带符号整数。

之所以需要部署这个方法，是因为 JavaScript 有精度限制，超过 2 的 53 次方的值无法精确表示。这就是说，对于那些很大的数的乘法，低位数值往往都是不精确的， Math.imul 方法可以返回正确的低位数值。

```js
Math.imul(-2, -2)   //4
```

#### 6.7.6 Math.fround()

Math.fround 方法返回一个数的单精度浮点数形式。
```js
Math.fround(1)  //1
Math.fround(1.337)  //1. 3370000123977661
Math.fround(1.5)    //1.5
```

#### 6.7.7 Math.hypot()

Math.hypot 方法返回所有参数的平方和的平方根。

```js
Math.hypot(3, 4);   //5
Math.hypot(3, 4, '5');    //7.0710678118654755
```

#### 6.7.8 对数方法

- Math.expm1()  返回 e^x - 1 ，即 Math.exp(x) - 1
- Math.log1p()  返回 ln(1 + x) ，即 Math.log(1 + x)
- Math.log10()  返回以 10 为底的 x 的对数
- Math.log2()   返回以 2 为底的 x 的对数

#### 6.7.9 双曲函数方法

- Math.sinh(x) 返回x的双曲正弦（hyperbolic sine）
- Math.cosh(x) 返回x的双曲余弦（hyperbolic cosine）
- Math.tanh(x) 返回x的双曲正切（hyperbolic tangent）
- Math.asinh(x) 返回x的反双曲正弦（inverse hyperbolic sine）
- Math.acosh(x) 返回x的反双曲余弦（inverse hyperbolic cosine）
- Math.atanh(x) 返回x的反双曲正切（inverse hyperbolic tangent）

### 6.8 Math.signbit()

Math.sign() 用来判断一个值的正负，但是如果参数是 -0 ，它便会返回 -0 。

sighbit() 判断一个数的符号位是否己经设置。

```js
Math.signbit(2)     //false
Math.signbit(-2)    //true
Math.signbit(0)     //false
Math.signbit(-0)    //true
```

### 6.9 指数运算符

ES2016 新增了一个指数运算符（ ** ）。
```js
2 ** 4  //16

let b = 4;
b **= 3;
//b = b * b * b;
```

### 6.10 Integer 数据类型

#### 6.10.1 简介

JavaScript 所有数字都保存成 64 位浮点数，这决定了整数的精确程度只能到 53 个二进制位。

大于这个范围的整数， JavaScript 是无法精确表示的，这使得 JavaScript 不适合进行科学和金融方面的精确计算。

现在有一个提案，其中引入了新的数据类型 Integer （整数）来解决这个问题。整数类型的数据只用来表示整数，**没有位数的限制**，任何位数的整数都可以精确表示。

为了与 Number 类型区别， Integer 类型的数据必须使用后缀`n`来表示。
```js
1n + 2n //3n

//二进制、八进制、十六进制的表示法都要加上后缀 n
0b1101n     //二进制
0o777n      //八进制
0xFFn       //十六进制

//对于 Integer 类型的数据， typeof 运算符将返回 integer
typeof 123n     //'integer'

//JavaScript 原生提供 Integer 对象， 用来生成 Integer 类型的数值。转换规则基本与 Number() 一致。
Integer(123)    //123
Integer('123')  //123n
Integer(true)   //1
new Integer()   //error
```

#### 6.10.2 运算

在数学运算方面， Integer 类型的 + 、 - 、 * 、 ** 这四个二元运算符与 Number 类型的行为一致。除法运算 / 会舍去小数部分，返回一个整数。
```js
9n / 5n // 1n

//Integer 类型不能与 Number 类型进行混合运算。
1n + 1  //error
//相等运算符（ == ）会改变数据类型，也是不允许混合使用的。
0n == 0 //error
//精确相等运算符（ === ）不会改变数据类型，因此可以混合使用。
0n === 0    //false
```

## 第七章 函数的扩展

### 7.1 函数参数的默认值

#### 7.1.1 基本用法

ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面。

好处：
- 阅读代码的人可以立刻意识到哪些参数是可以省略的，不用查看函数体或文档
- 有利于将来的代码优化，即使未来的版本彻底拿掉这个参数，也不会导致以前的代码无法运行。

```js
function foo(x = 0, y = 0) {
    this.x = x;
    this.y = y;
}
var p = new Point();
p   // {x: 0, y: 0}

//参数默认值不是传值的，而是每次都重新计算默认值表达式的值。也就是说，参数默认值是惰性求值的。
let x = 99;
function foo(p = x + 1) {
    alert(p);
}
foo();  //100

x = 200;
foo();  //201   //重新计算 x + 1
```

#### 7.1.2 与解构赋值默认值结合使用

```js
function foo({x, y = 5}) {
    alert(x, y);
}
foo({});    //undefined, 5
foo({x: 1}); //1, 5
foo({x:1, y: 2});    //1, 2

foo();   //error 只使用了对象的解构赋值默认值，而没有使用函数参数的默认值。只有当 foo 的参数是一个对象时，才解构。
```

对比下面两种写法的差别：
```js
//1
function m1({x: 0, y: 0} = {}) {
    return [x, y];
}
//2
function m2({x, y} = {x: 0, y: 0})  {
    return [x, y];
}
```
区别：
- 写法一中函数参数的默认值是*空对象*，但是设置了对象解构赋值的默认值
- 写法二中函数参数的默认值是*一个有具体属性的对象*，但是没有设置对象解构赋值的默认值
```js
//都有值正常

//函数没有参数的情况
m1()    //[0, 0]
m2()    //[0, 0]
//x 有值， y 无值的情况
m1({x:1})   //[1, 0]
m2({x:1})   //[1, undefined]    //因为传入了对象，不需要默认值，没有解构导致 y undefined

//x 和 y 都无值的情况
m1({})  //[0, 0]
m2({})  //[undefined, undefined]
m1({z: 1})  //[0, 0]
m1({z: 1})  //[undefined, undefined]
```

#### 7.1.3 参数默认值的位置

通常情况下，定义了默认值的参数应该是函数的尾参数。因为这样比较容易看出到底省略了哪些参数。如果非尾部的参数设置默认值，实际上这个参数是无法省略的。（设置默认值只能设置在尾部的，不能尾部不设置，设置头部的，调用时报错）

```js
function f(x = 1, y) {
    return [x, y];
}
f();        //[1, undefined]
f(2)        //[2, undefined]
f(, 2);     //error!
f(undefined, 2);    //[1, 2]
f(null, 2);         //[null, 2] //如果传入 undefined ，将触发该参数等于默认值， null 则没有这个效果。
```

#### 7.1.4 函数的 length 属性

指定了默认值以后，函数的 length 属性将返回没有指定默认值的参数个数。（失真）
```js
(function (a) {}).length // 1
(function (a = 5) {}).length // 0
(function (a, b, c = 5) {}).length // 2
(function (a, b = 1, c) {}).length // 1     //如果设置了默认值的参数不是尾参数，那么length属性也不再计入后面的参数了。
```

#### 7.1.5 作用域

```js
var x = 1;
function f(x, y = x) {
    alert(y);
}
//调用函数 f 时，参数形成一个单独的作用域。
f(2)    //2     在这个作用域里面，默认值变量 x 指向第一个参数 x ，而不是全局变量 x
```

```js
var x = 1;
function foo(x, y = function() {x = 2;}) {      //y 函数内部的 x 是指 第一个参数 x （参数形成单独作用域）
    var x = 3;
    y();
    console.log(x);
}
foo()   //3 foo内部的 var x = 3;
x       //1 全局的 var x = 1;
```

#### 7.1.6 应用

利用参数默认值可以指定某一个参数不得省略，如果省略就抛出一个错误。
```js
function throwIfMissing() {
  throw new Error('Missing parameter');
}

function foo(mustBeProvided = throwIfMissing()) {       //如果参数省略，默认值等于等于一个函数调用，抛出错误
  return mustBeProvided;
}

foo()
// Error: Missing parameter
```

### 7.2 rest 参数

ES6 引入 rest 参数（形式为...变量名），用于获取函数的多余参数，这样就不需要使用arguments对象了。rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中。

```js

// arguments变量的写法
//arguments对象不是数组，而是一个类似数组的对象。所以为了使用数组的方法，必须使用Array.prototype.slice.call先将其转为数组。
function sortNumbers() {
  return Array.prototype.slice.call(arguments).sort();
}

//// rest参数的写法
//rest 参数就不存在这个问题，它就是一个真正的数组，数组特有的方法都可以使用。
const sortNumbers = (...numbers) => numbers.sort();

/**
 * * 注意，rest 参数之后不能再有其他参数（即只能是最后一个参数），否则会报错。
 * /
// 报错
function f(a, ...b, c) {
  // ...
}

/**
* 函数的length属性，不包括 rest 参数。
**/
(function(a) {}).length  // 1
(function(...a) {}).length  // 0
(function(a, ...b) {}).length  // 1
```

### 7.3 严格模式

从 ES5 开始，函数内部可以设定为严格模式。
```js
function doSomething(a, b) {
  'use strict';
  // code
}
```

ES2016 做了一点修改，规定只要函数参数使用了默认值、解构赋值、或者扩展运算符，那么函数内部就不能显式设定为严格模式，否则会报错。
```js
// 报错
function doSomething(a, b = a) {
  'use strict';
  // code
}

// 报错
const doSomething = function ({a, b}) {
  'use strict';
  // code
};

// 报错
const doSomething = (...a) => {
  'use strict';
  // code
};

const obj = {
  // 报错
  doSomething({a, b}) {
    'use strict';
    // code
  }
};
```

两种方法可以规避这种限制。第一种是设定全局性的严格模式。第二种是把函数包在一个无参数的立即执行函数里面。

### 7.4 name 属性

函数的 name 属性，返回该函数的函数名。
```js
function foo() {}
foo.name // "foo"
```

### 7.5 箭头函数

#### 7.5.1 基本用法

允许使用“箭头”（ => ）定义函数，使得表达更加简洁。简化回调函数。
```js
var f = v => v;
//等同于
var f = function(v) {
    return v;
}

/**
 * 如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分。
**/
var f = () => 5;
//等同于
var f = function() {
    return 5;
}

/**
 * 如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用return语句返回。
**/
var sum = (num1, num2) => {return num1 + num2;}

/**
 * 由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号，否则会报错。
**/
//报错
let getTempItem = id => {id: id, name: "temp"};
//不报错
let getTempItem = id => ({id: id, name: "temp"});
//特殊情况，可以运行，但是拿到错误的结果 // foo()  undefined
let foo = () => {a: 1};     //愿意图返回对象。但大括号作为代码块，执行语句 a: 1 。解析 a 为标签，执行语句 1 ，函数没有返回值。

/**
 * 箭头函数可以与变量解构结合使用。
**/
const full = ({first, last}) => first + ' ' + last;
//等同于
function full(person) {
    return person.first + ' ' + person.last;
}

/**
 * 如果箭头函数只有一行语句，且不需要返回值，可以采用下面的写法，就不用写大括号了。
**/
let fn = () => void doesNotReturn();
```

#### 7.5.2  使用注意点
箭头函数有几个使用注意点：
- 函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
- 不可以当作构造函数，也就是说，不可以使用 new 命令，否则会抛出一个错误。
- 不可以使用 arguments 对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
- 不可以使用 yield 命令，因此箭头函数不能用作 Generator 函数。


上面四点中，第一点尤其值得注意。 this 对象的指向是可变的，但是在箭头函数中，它是固定的。 this 指向的固定化，并不是因为箭头函数内部有绑定 this 的机制，实际原因是箭头函数根本没有自己的 this ，导致内部的 this 就是外层代码块的 this 。正是因为它没有 this ，所以也就不能用作构造函数。

除了 this ，以下三个变量在箭头函数之中也是不存在的，**指向外层函数的对应变量**： arguments 、 super 、 new.target 。

另外，由于箭头函数没有自己的 this ，所以当然也就不能用 call() 、 apply() 、 bind() 这些方法去改变 this 的指向。
```js
//ES6
function foo() {
    setTimeout(() => {
        console.log('id: ', this.id);   //箭头函数里面根本没有自己的 this ，而是引用外层的 this 。
    }, 100);
}
//ES5 等价
function foo() {
    var _this = this;   //
    setTimeout(function() {
        console.log('id: ', _this.id);
    }, 100);
}
```

#### 7.5.3 不适用场合

第一个场合是定义对象的方法，且该方法内部包括this。
```js
const cat = {
    lives: 9,
    jumps: () => {
        this.lives--;    //使得this指向全局对象（对象不构成单独的作用域），因此不会得到预期结果
    }
}
```
第二个场合是需要动态this的时候，也不应使用箭头函数。
```js
var button = document.getElementById('press');
button.addEventListener('click', () => {
  this.classList.toggle('on');  //this 全局对象
});
```

#### 7.5.4 嵌套的箭头函数

```js
let insert = (value) => ({into: (array) => ({after: (afterValue) => {
    array.splice(array.indexOf(afterValue) + 1, 0, value);
    return array;
}})});
insert(2).into([1, 3]).after(1);    //[1, 2, 3]
//insert(value) 返回一个对象， 这个对象只包含一个函数 into(array) ，
//into(array) 数也返回一个对象，这个对象也只包含一个函数 after(afterValue)
//after(afterValue) 返回最后组合的结果
```

### 7.6 尾调用优化

#### 7.6.1 什么是尾调用？

尾调用（ Tail Call ）是函数式编程的一个重要概念，本身非常简单，一句话就能说清楚，就是指*某个函数的最后一步是调用另一个函数*。
```js
function f(x) {
    return g(x);
}

//以下三种情况，都不属于尾调用。
//情况一 是调用函数g之后，还有赋值操作
function f(x){
  let y = g(x);
  return y;
}
//情况二 调用后还有操作，即使写在一行内
function f(x){
  return g(x) + 1;
}
//情况三 等同于 g(x); return undefined;
function f(x){
  g(x);
}
```

#### 7.6.2 尾调用优化

因为调用栈。尾调用由于是函数的最后一步操作，所以不需要保留外层函数的调用帧（调用记录），因为调用位置、内部变量等信息都不会再用到了，只要直接用内层函数的调用帧，取代外层函数的调用帧就可以了。

注意，只有不再用到外层函数的内部变量，内层函数的调用帧才会取代外层函数的调用帧，否则就无法进行“尾调用优化”。
```js
function f() {
  let m = 1;
  let n = 2;
  return g(m + n);
}
f();

// 等同于
function f() {
  return g(3);
}
f();

// 等同于
g(3);
```

#### 7.6.3 尾递归

函数调用自身，称为递归。如果尾调用自身，就称为尾递归。

递归非常耗费内存，因为需要同时保存成千上百个调用帧，很容易发生“栈溢出”错误（stack overflow）。但对于尾递归来说，由于只存在一个调用帧，所以永远不会发生“栈溢出”错误。
```js
//计算 n 的阶乘，最多需要保存 n 个调用记录，复杂度 O(n)
function factorial(n) {
    if(n === 1) return 1;
    return n * factorial(n - 1);
}
factorial(5)    //120

//改写成尾递归，只保留一个调用记录，复杂度 O(1)
function factorial(n, total) {
    if(n === 1) return total;
    return factorial(n - 1, n * total);
}
factorial(5, 1);    //120
```

#### 7.6.4 递归函数的改写

#### 7.6.5 严格模式

ES6 的尾调用优化只在严格模式下开启，正常模式是无效的。

这是因为在正常模式下，函数内部有两个变量，可以跟踪函数的调用栈。严格模式禁用这两个变量，所以尾调用模式仅在严格模式下生效。

#### 7.6.6 尾递归优化的实现

### 7.7 函数参数的尾逗号

ES2017 允许函数的最后一个参数有尾逗号（trailing comma）。

```js
function clownsEverywhere(
  param1,
  param2,           //此前，函数定义和调用时，都不允许最后一个参数后面出现逗号。
) { /* ... */ }

clownsEverywhere(
  'foo',
  'bar',
);
```

## 第八章 数组的扩展

### 8.1 扩展运算符

#### 8.1.1 含义

扩展运算符（spread）是三个点（ ... ）。它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列。该运算符主要用于函数调用。
```js
function push(array, ...items) {
    array.push(...items);   //函数的调用
}
function add(x, y) {
    return x + y;
}
const numbers = [4, 38];
add(...numbers);

/**
 * 扩展运算符与正常的函数参数可以结合使用，非常灵活。
**/
function f(v, w, x, y, z) {
}
const args = [0, 1];
f(-1, ...args, 2, ...[3]);

/**
 * 扩展运算符后面还可以放置表达式。
**/
const arr = [
    ...(x > 0 ? ['a'] : []),    //如果扩展运算符后面是一个空数组，则不产生任何效果。
    'b',
];

/**
 * 注意，只有函数调用时，扩展运算符才可以放在圆括号中，否则会报错。
**/
(...[1, 2])
// Uncaught SyntaxError: Unexpected number 因为扩展运算符所在的括号不是函数调用。

console.log((...[1, 2]))
// Uncaught SyntaxError: Unexpected number 因为扩展运算符所在的括号不是函数调用。

console.log(...[1, 2])
// 1 2
```

#### 8.1.2 替代函数的 apply 方法

由于扩展运算符可以展开数组，所以不再需要apply方法，将数组转为函数的参数了。
```js

function f(x, y, z) {
    // ...
}
var args = [1, 2, 3];
// ES5 的写法
f.apply(null, args);
// ES6 的写法
f(...args);
```

#### 8.1.3 扩展运算符的应用

1. 复制数组

数组是复合的数据类型，直接复制的话，只是复制了指向底层数据结构的指针，而不是克隆一个全新的数组。
```js
const a1 = [1, 2];
const a2 = a1;
a2[0] = 2;
alert(a1);  //[2, 2]

//ES5 只能用变通方法来复制数组
const a1 = [1, 2];
const a2 = a1.concat();
a2[0] = 2;
alert(a1);  //[1, 2]

//扩展运算符提供了复制数组的简便写法。
const a1 = [1, 2];
//写法一
const a2 = [...a1];
//写法二
const [...a2] = a1;
```

2. 合并数组

扩展运算符提供了数组合并的新写法。不过，这两种方法都是浅拷贝，使用的时候需要注意。
```js
cosnt a1 = [{foo: 1}, {baz: 4}];
const a2 = [{bar: 2}];

const a3 = a1.concat(a2);
const a4 = [...a1, ...a2];

//它们的成员都是对原数组成员的引用（指向相同对象），这就是浅拷贝
a3[0] === a1[0] //true
a4[0] === a1[0] //true
```

3. 与解构赋值结合

扩展运算符可以与解构赋值结合起来，用于生成数组。
```js
const [first, ...rest] = [1, 2, 3, 4, 5];
first   //1
rest    //[2, 3, 4, 5]

const [first, ..rest] = [];
first   //undefined
rest    //[]

const [first, ...rest] = [1];
first   //1
rest    //[]

//如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。
const [first, ...middle, last] = [1, 2, 3, 4, 5];   //error!
```

4. 字符串

扩展运算符还可以将字符串转为真正的数组。有一个重要的好处，那就是能够正确识别四个字节的 Unicode 字符。
```js
[...'hello']
// [ "h", "e", "l", "l", "o" ]
```

5. 实现了 Iterator 接口的对象

任何定义了遍历器（Iterator）接口的对象（参阅 Iterator 一章），都可以用扩展运算符转为真正的数组。
```js
//querySelectorAll方法返回的是一个NodeList对象。它不是数组，而是一个类似数组的对象。
let nodeList = document.querySelectorAll('div');
//扩展运算符可以将其转为真正的数组，原因就在于NodeList对象实现了 Iterator 。
let array = [...nodeList];
```

6. Map 和 Set 结构， Generator 函数

扩展运算符内部调用的是数据结构的 Iterator 接口，因此只要具有 Iterator 接口的对象，都可以使用扩展运算符，比如 Map 结构。
```js
let map = new Map([
    [1, 'one'],
    [2, 'two'],
    [3, 'three'],
]);
let arr = [...map.keys()];
```

### 8.2 Array.from()

Array.from方法用于将两类对象转为真正的数组：
- 类似数组的对象（本质特征只有一点，即必须有length属性。）
- 可遍历（iterable）的对象（包括 Set 和 Map）。

```js
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};
let arr2 = Array.from(arrayLike);   // ['a', 'b', 'c']

Array.from({ length: 3 });
// [ undefined, undefined, undefined ]

/**
 * 只要是部署了 Iterator 接口的数据结构，Array.from都能将其转为数组。
**/
Array.from('hello')
// ['h', 'e', 'l', 'l', 'o']

let namesSet = new Set(['a', 'b'])
Array.from(namesSet) // ['a', 'b']

/**
 * Array.from还可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组。
**/
Array.from(arrayLike, x => x * x);
// 等同于
Array.from(arrayLike).map(x => x * x);

Array.from([1, 2, 3], (x) => x * x)
// [1, 4, 9]

/**
 * 将字符串转为数组，然后返回字符串的长度。因为它能正确处理各种 Unicode 字符
**/
function countSymbols(string) {
  return Array.from(string).length;
}
```

### 8.3 Array.of()

Array.of 方法用于将一组值，转换为数组。

这个方法的主要目的，是弥补数组构造函数Array()的不足。因为参数个数的不同，会导致Array()的行为有差异。
```js
Array.of(3, 11, 8) // [3,11,8]
Array.of(3) // [3]
Array.of(3).length // 1

Array() // []
Array(3) // [, , ,] 参数个数只有一个时，实际上是指定数组的长度。
Array(3, 11, 8) // [3, 11, 8] 只有当参数个数不少于 2 个时，Array()才会返回由参数组成的新数组
```

### 8.4 数组实例的 copyWithin()

数组实例的copyWithin方法，在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。也就是说，使用这个方法，会修改当前数组。

它接受三个参数。

- target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
- start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示倒数。
- end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示倒数。

```js
[1, 2, 3, 4, 5].copyWithin(0, 3)
// [4, 5, 3, 4, 5]  将从 3 号位直到数组结束的成员（4 和 5），复制到从 0 号位开始的位置，结果覆盖了原来的 1 和 2

// 将3号位复制到0号位
[1, 2, 3, 4, 5].copyWithin(0, 3, 4)
// [4, 2, 3, 4, 5]
// -2相当于3号位，-1相当于4号位
[1, 2, 3, 4, 5].copyWithin(0, -2, -1)
// [4, 2, 3, 4, 5]

// 将3号位复制到0号位
[].copyWithin.call({length: 5, 3: 1}, 0, 3)
// {0: 1, 3: 1, length: 5}  //本来该对象没有 0 号位

// 将2号位到数组结束，复制到0号位
let i32a = new Int32Array([1, 2, 3, 4, 5]);
i32a.copyWithin(0, 2);
// Int32Array [3, 4, 5, 4, 5]
```

### 8.5 数组实例的 find() 和 findIndex()

数组实例的 find 方法，用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为true的成员，然后返回该成员。如果没有符合条件的成员，则返回undefined。

数组实例的 findIndex 方法，返回第一个符合条件的数组成员的位置。无则返回 -1 .



```js
[1, -4, -5, 10].find(n=> n < 0)     //-4

//find方法的回调函数可以接受三个参数，依次为当前的值、当前的位置和原数组。
[1, 5, 10, 15].find((value, index, arr) => value > 9)   //10

[1, 5, 10, 15].findIndex(n => n == 10); //2
```

这两个方法都可以接受第二个参数，用来绑定回调函数的this对象。

另外，这两个方法都可以发现NaN，弥补了数组的indexOf方法的不足。
```js
let person = {name: 'John', age: 20};
[10, 12, 26, 15].find(function(v) {
    return v > this.age;
}, person);
// var age = 10;
// [10, 12, 26, 15].find(v => v > this.age, person);    这里的 this 是 window


[NaN].indexOf(NaN)
// -1

[NaN].findIndex(y => Object.is(NaN, y))
// 0
```

### 8.6 数组实例的 fill()

fill 方法使用给定值，填充一个数组。
```js
//fill 方法用于空数组的初始化非常方便。数组中已有的元素，会被全部抹去。
['a', 'b', 'c'].fill(7)     // [7, 7, 7]
new Array(3).fill(7)        // [7, 7, 7]

//注意，如果填充的类型为对象，那么被赋值的是同一个内存地址的对象，而不是深拷贝对象。
let arr = new Array(3).fill({name: "Mike"});
arr[0].name = "Ben";
arr
// [{name: "Ben"}, {name: "Ben"}, {name: "Ben"}]
```

### 8.7 数组实例的 entries() 、 keys() 和 values()

三个都用于遍历数组。

它们都返回一个遍历器对象（详见《Iterator》一章），可以用for...of循环进行遍历。

唯一的区别是 keys() 是对键名的遍历、 values() 是对键值的遍历， entries() 是对键值对的遍历。

```js
let array = ['a', 'b'];
for(let index of array.keys()) {
    console.log(index);
}
//0
//1
for(let elem of array.values()) {
    console.log(elem);
}
for(let [index, elem] of ['a', 'b'].entries()) {
    console.log(index, elem);
}
// 0 "a"
// 1 "b"
```

### 8.8 数组实例的 includes()

表示某个数组是否包含给定的值，返回布尔值。
```js
[1, 2, 3].includes(2)     // true
[1, 2, 3].includes(4)     // false
[1, 2, NaN].includes(NaN) // true

//该方法的第二个参数表示搜索的起始位置，默认为0。
//如果第二个参数为负数，则表示倒数的位置，如果这时它大于数组长度（比如第二个参数为-4，但数组长度为3），则会重置为从0开始。
[1, 2, 3].includes(3, 3);  // false
[1, 2, 3].includes(3, -1); // true
```

### 8.9 数组实例的 flat() ， flatMap()

Array.prototype.flat() 用于将嵌套的数组“拉平”，变成一维的数组。该方法返回一个新数组，对原数据没有影响。
```js
[1, 2, [3, 4]].flat()
//[1, 2, 3, 4]

//默认只会“拉平”一层，如果想要“拉平”多层的嵌套数组，可以将 flat() 方法的参数写成一个整数，表示想要拉平的层数，默认为1。
[1, 2, [3, [4, 5]]].flat(2)
// [1, 2, 3, 4, 5]

//如果不管有多少层嵌套，都要转成一维数组，可以用Infinity关键字作为参数。
[1, [2, [3]]].flat(Infinity)
// [1, 2, 3]

//如果原数组有空位，flat()方法会跳过空位。
[1, 2, , 4, 5].flat()
// [1, 2, 4, 5]
```

flatMap() 方法对原数组的每个成员执行一个函数（相当于执行Array.prototype.map()），然后对返回值组成的数组执行 flat() 方法。该方法返回一个新数组，不改变原数组。

flatMap() 只能展开一层数组。

flatMap() 方法的参数是一个遍历函数，该函数可以接受三个参数，分别是当前数组成员、当前数组成员的位置（从零开始）、原数组。

flatMap() 方法还可以有第二个参数，用来绑定遍历函数里面的 this 。
```js
//执行回调函数，对返回的结果进行展平 flat 一次

// 相当于 [[2, 4], [3, 6], [4, 8]].flat()
[2, 3, 4].flatMap((x) => [x, x * 2])
// [2, 4, 3, 6, 4, 8]

// 相当于 [[[2]], [[4]], [[6]], [[8]]].flat()
[1, 2, 3, 4].flatMap(x => [[x * 2]]);
// [[2], [4], [6], [8]]
```

### 8.10 数组的空位

ES6 则是明确将空位转为 undefined ！由于空位的处理规则非常不统一，所以建议避免出现空位。

数组的空位指，数组的某一个位置没有任何值。比如， Array 构造函数返回的数组都是空位。

注意，空位不是 undefined ，一个位置的值等于 undefined ，依然是有值的。空位是没有任何值， in 运算符可以说明这一点。
```js
0 in [undefined, undefined, undefined] // true
0 in [, , ,] // false
```

```js
//数组 arr 有两个空位， for...of 并没有忽略它们。如果改成 map 方法遍历，空位是会跳过的。
let arr = [,,1];
for(let i of arr) {
    console.log(1111);
}
// 1111
// 1111
// 1111     //打印三次
arr.map(()=> void console.log(1111));
// 1111     //只打印一次

/**
 * entries()、keys()、values()、find()和findIndex()会将空位处理成undefined。
**/
// entries()
[...[,'a'].entries()] // [[0,undefined], [1,"a"]]

// keys()
[...[,'a'].keys()] // [0,1]

// values()
[...[,'a'].values()] // [undefined,"a"]

// find()
[,'a'].find(x => true) // undefined

// findIndex()
[,'a'].findIndex(x => true) // 0
```

## 第九章 对象的扩展

### 9.1 属性的简洁表达式

ES6 允许直接写入变量和函数，作为对象的属性和方法。这样的书写更加简洁。

```js
const foo = 'bar';
const baz = {foo};      //在对象之中，直接写变量 属性名为变量名, 属性值为变量的值。
baz // {foo: "bar"}
//等同于
const baz = {foo: foo};

function f(x, y) {
    return {x, y};
}
//等同于
function f(x, y) {
    return {x: x, y: y};
}
f(1, 2) // {x: 1, y: 2}

/**
 * 除了属性简写，方法也可以简写。
**/
const o = {
    method() {
        return "Hello";
    }
};
//等同于
const o = {
    method: function() {
        return "Hello";
    }
};

//实际例子
let age = 23;
const person = {
    name: "Kiyonami",
    age,
    Method() {
        console.log("hello");
    }
}

/**
 * 属性的赋值器（ setter ）和取值器（ getter ），事实上也是采用这种写法。
**/
const cart = {
  _wheels: 4,

  get wheels () {
    return this._wheels;
  },

  set wheels (value) {
    if (value < this._wheels) {
      throw new Error('数值太小了！');
    }
    this._wheels = value;
  }
}

/**
 * 如果某个方法的值是一个 Generator 函数，前面需要加上星号。
**/
const obj = {
  * m() {
    yield 'hello world';
  }
};
```

### 9.2 属性名表达式

JavaScript 定义对象的属性，有两种方法。
```js
//方法一 直接用 标识符 作为 属性名
obj.foo = true;
//方法二 用 表达式 作为属性名，这时要将表达式放在方括号之内
obj['a' + 'bc'] = 123;
```
但是，如果使用字面量方式定义对象（使用大括号），在 ES5 中只能使用方法一（标识符）定义属性。

ES6 允许字面量定义对象时，用方法二（表达式）作为对象的属性名，即把表达式放在方括号内。
```js
//ES5
var obj = {
    foo: true,
    abc: 123
}
//ES6
let propKey = 'foo';
let obj = {
    [proKey]: true,
    ['a' + 'bc']: 123
    ['h' + 'ello']() {
        return 'hi';
    }
};
obj.hello();

/**
 * 注意，属性名表达式与简洁表示法，不能同时使用，会报错。
**/
// 报错
const foo = 'bar';
const bar = 'abc';
const baz = { [foo] };

// 正确
const foo = 'bar';
const baz = { [foo]: 'abc'};    // {bar: "abc"}

/**
 * 注意，属性名表达式如果是一个对象，默认情况下会自动将对象转为字符串[object Object]，这一点要特别小心。
**/
const keyA = {a: 1};
const keyB = {b: 2};

const myObject = {
  [keyA]: 'valueA',
  [keyB]: 'valueB'
};

myObject // Object {[object Object]: "valueB"}
```


### 9.3 方法的 name 属性

函数的name属性，返回函数名。对象方法也是函数，因此也有name属性。
```js
const person = {
  sayName() {
    console.log('hello!');
  },
};

person.sayName.name   // "sayName"

/**
 * 如果对象的方法使用了取值函数（getter）和存值函数（setter），则name属性不是在该方法上面，而是该方法的属性的描述对象的get和set属性上面，返回值是方法名前加上get和set。
**/
const obj = {
  get foo() {},
  set foo(x) {}
};

obj.foo.name
// TypeError: Cannot read property 'name' of undefined

const descriptor = Object.getOwnPropertyDescriptor(obj, 'foo');

descriptor.get.name // "get foo"
descriptor.set.name // "set foo"

/**
 * 有两种特殊情况： bind 方法创造的函数， name 属性返回 bound 加上原函数的名字； Function 构造函数创造的函数， name 属性返回 anonymous 。
**/
(new Function()).name // "anonymous"

var doSomething = function() {
  // ...
};
doSomething.bind().name // "bound doSomething"

/**
 * 如果对象的方法是一个 Symbol 值，那么 name 属性返回的是这个 Symbol 值的描述。
**/
const key1 = Symbol('description');
const key2 = Symbol();
let obj = {
  [key1]() {},
  [key2]() {},
};
obj[key1].name // "[description]"
obj[key2].name // ""
```

### 9.4 Object.is()

ES5 比较两个值是否相等，只有两个运算符：相等运算符（ == ）和严格相等运算符（===）。它们都有缺点，前者（ == ）会自动转换数据类型，后者（===）的 NaN 不等于自身，以及 +0 等于 -0 。

Object.is 用来比较两个值是否严格相等，与严格相等运算符（ === ）的行为基本一致。
```js
Object.is('foo', 'foo');    //true
Object.is({}, {})           //false     //依旧只是比较引用
//不同之处只有两个： 一是 +0 不等于 -0 ， 二是 NaN 等于自身。
+0 === -0               //true
NaN === NaN             //false
Object.is(+0 , -0)      //false
Object.is(NaN , NaN)    //true
```

### 9.5 Object.assign()

#### 9.5.1 基本用法

方法用于将源对象（ source ）的所有*可枚举属性*复制到目标对象（ target ）。

如果目标对象与源对象有同名属性，或多个源对象有对象有同名属性，则后面的属性会覆盖前面
的属性。

Object.assign() 复制的属性是有限制的，只复制源对象的自身属性（不复制继承属性），也不复制不可枚举的属性（ enumerable: false ）。

```js
var target = {a: 1, b: 1};
var source1 = {b: 2, c: 2};
var source2 = {c: 3, d: 3};

Object.assign(target, source1, source2);
target  //{a: 1, b: 2, c: 3}

/**
*   如果只有一个参数， Object.assign 会直接返回该参数。
**/
var obj = {a: 1};
Object.assign(obj) == obj   //true  说明直接返回源引用

/**
*   如果该参数不是对象， 则会先转成对象，然后返回。
**/
typeof Object.assign(2) // “object"

/**
*   由于 undefined 和 null 无法转成对象，所以如果将它们作为参数， 就会报错。
**/
Object.assign(undefined)    //error
Object.assign(null)         //error
```

#### 9.5.2 注意点

Object.assign 方法实行的是浅复制，而不是深复制。
```js
var obj1 = {a: {b: 1}};
var obj2 = Object.assign({}, obj1);
obj1.a.b = 2;
obj2.a.b    // 2    复制得到的是这个对象的引用
```

#### 9.5.3 常见用途

1. 为对象添加属性、方法

```js
class Point {
    constructor(x, y) {
        Object.assign(this, {x, y});    //将 x 属性和 y 属性添加到了 Point 类的对象实例中
    }
}

Object.assgin(SomeClass.prototype, {
    someMethod(arg1, arg2) {
        //...
    },
    anotherMethod() {
        //...
    }
})
```
2. 克隆对象
3. 合并多个对象

```js
const merge = (...sources) => Object.assign({}, ...sources);
```

4. 为属性指定默认值


### 9.6 属性的可枚举性

对象的每个属性都有一个描述对象（Descriptor），用来控制该属性的行为。Object.getOwnPropertyDescriptor方法可以获取该属性的描述对象。

目前，有四个操作会忽略 enumerable 为 false 的属性。
- for...in 循环：只遍历对象自身的和**继承**的可枚举的属性。
- Object.keys()：返回**对象自身**的所有可枚举的属性的键名。
- JSON.stringify()：只串行化对象自身的可枚举的属性。
- Object.assign()： 忽略 enumerable 为 false 的属性，只拷贝对象自身的可枚举的属性。

### 9.7 属性的遍历

ES6 一共有 5 种方法可以遍历对象的属性。

1. for...in

for...in循环遍历对象自身的和**继承**的可枚举属性（不含 Symbol 属性）。

2. Object.keys(obj)

Object.keys返回一个数组，包括**对象自身**的（不含继承的）所有可枚举属性（不含 Symbol 属性）的键名。

3. Object.getOwnPropertyNames(obj)

返回一个数组，包含对象**自身**的所有属性（不含 Symbol 属性，但是**包括不可枚举属性**）的键名。

4. Object.getOwnPropertySymbols(obj)

返回一个数组，包含对象自身的所有 Symbol 属性的键名。

5. Reflect.ownKeys(obj)

返回一个数组，包含对象**自身**的**所有键名**，不管键名是 Symbol 或字符串，也不管是否可枚举。

以上的 5 种方法遍历对象的键名，都遵守同样的属性遍历的次序规则。
- 首先遍历所有数值键，按照数值升序排列。
- 其次遍历所有字符串键，按照加入时间升序排列。
- 最后遍历所有 Symbol 键，按照加入时间升序排列。
```js
Reflect.ownKeys({ [Symbol()]:0, b:0, 10:0, 2:0, a:0 })
// ['2', '10', 'b', 'a', Symbol()]
```

### 9.8 `__proto__` 属性、 Object.setPrototypeOf() 、 Object.getPrototypeOf()

#### 9.8.1  `__proto__` 属性

`__proto__ `用来读取或设置当前对象的 prototype 对象。

标准明确规定， 只有浏览器必须部署这个属性， 其他运行环境不一定要部署，而且新的代码最好认为**这个属性是不存在**的。

在实现上， `__proto__` 调用的是 `Object.prototype.__proto__`

```js
// ES6 的写法
var obj = {
    method: function() {
        //...
    }
}
obj.__proto__ = someOtherObj;

// ES5 的写法
var obj = Object.create(someOtherObj);
obj.method = function() {
    //...
}
```

#### 9.8.2 Object.setPrototypeOf()

Object.setPrototypeOf 方法的作用与 `__proto__` 相同，用来设置一个对象的 prototype 对象，返回参数对象本身。它是 ES6 **正式推荐的设置原型对象**的方法。

```js
let proto = {};
let obj = {x: 10};

Object.setPrototypeOf(obj, proto);  //设置为原型
proto.y = 20;
proto.z = 40;   //改变原型属性值

obj.x //10
obj.y //20
obj.z //40      //读取 proto 对象的属性
```

#### 9.8.3 Object.getPrototypeOf()

该方法与 setPrototypeOf 方法配套，用于读取一个对象的 prototype 对象。

```js
function Rectangle() {
    //...
}
var rec = new Rectangle();
Object.getPrototypeOf(rec) === Rectangle.prototype  //true
```

### 9.9 Object.keys() Object.values() Object.entries()

#### 9.9.1 Object.keys()

返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（ enumerable ）属性的*键名*。

```js
var obj = {foo: 'bar', baz: 42};
Object.keys(obj);   //["foo", "baz"]
```

#### 9.9.2 Object.values()

返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（ enumerable ）属性的*键值*。

#### 9.9.3 Object.entries()

返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（ enumerable ）属性的*键值对数组*。

```js
var obj = {foo: 'bar', baz: 42};
Object.entries(obj);    //[["foo", "bar"], ["baz", 42]]
```

Object.entries 的基本用途是遍历对象的属性。的另一个用处是将对象转为真正的 Map 结构。
```js
var obj = {one: 1, two: 2};
for(let [k, v] of Object.entries(obj)) {
    console.log(k, v);
}

var map = new Map(Object.entries(obj));
map.set(1, 2);
map     //{"one" => 1, "two" => 2, 1 => 2}
```

### 9.10 对象的扩展运算符

1. 解构赋值

对象的解构赋值用于从一个对象取值，相当于将所有可遍历的、*但尚未被读取的属性*分配到指定的对象上面。所有的键和它们的值都会复制到新对象上面。

如果等号右边是 undefined 或 null 就会报错， 因为它们无法转为对象。

解构赋值也不会复制继承自原型对象的属性。

```js
let {x: test, y, ...z} = {x: 1, y: 2, a: 3, b: 4};
test //1    x error
y //2
z //{a: 3, b: 4}
```

2. 扩展运算符

扩展运算符（ ... ）用于取出参数对象的所有可遍历属性，并将其复制到当前对象之中。

```js
let z = {a: 3, b: 4};
let n = {...z};
n   //{a: 3, b: 4}
//等同于
let n = Object.assign({}, z);

//合并对象
let ab = {...a, ...b};
//等同于
let ab = Object.assign({}, a, b);
```

### 9.11 Object.getOwnPropertyDescriptors()

### 9.12 Null 传导运算符

简化判断一个对象是否存在。

```js
//比如，要读取 message.body.user.firstName ，安全的写法如下。
cosnt firstName = (message
    && message.body
    && message.body.user
    && message.body.user.firstName) || "default";

//引入 Null 传到运算符--- ?.
const firstName = message?.body?.user?.firstName;   //，只要其中一个返回 null 或 undefined ，就不再继续运算，而是返回undefined 。

```

## 第十章 Symbol

### 10.1 概述

ES5 的对象属性名都是字符串，这容易造成属性名的冲突。

比如，你使用了一个他人提供的对象，但又想为这个对象添加新的方法（ mixin 模式），新方法的名字就有可能与现有方法产生冲突。

期望有一种机制，**从根本上防止属性名的冲突**。这就是 ES6 引入Symbol的原因。

ES6 引入了一种新的原始数据类型Symbol，*表示独一无二的值*。它是 JavaScript 语言的*第七种数据类型*，前六种是：undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。

对象的属性名现在可以有两种类型。一种是原来就有的字符串，另一种就是新增的 Symbol 类型。凡是属性名属于 Symbol 类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。

```js
/**
* Symbol 值通过 Symbol 函数生成
**/
let s = Symbol();
typeof s // "symbol"

/**
* 接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。否则输出都是 Symbol()
**/
let s1 = Symbol('foo');
let s2 = Symbol('bar');
s1 // Symbol(foo)
s2 // Symbol(bar)
s1.toString() // "Symbol(foo)"
s2.toString() // "Symbol(bar)"

/**
* 如果 Symbol 的参数是一个对象，就会调用该对象的toString方法，将其转为字符串，然后才生成一个 Symbol 值。
**/
const obj = {
  toString() {
    return 'abc';
  }
};
const sym = Symbol(obj);
sym // Symbol(abc)
/**
* Symbol函数的参数只是表示对当前 Symbol 值的描述，因此相同参数的Symbol函数的返回值是不相等的。
**/
// 有参数的情况
let s1 = Symbol('foo');
let s2 = Symbol('foo');

s1 === s2 // false

/**
* Symbol 值不能与其他类型的值进行运算，会报错。
**/
let sym = Symbol('My symbol');
"your symbol is " + sym
// TypeError: can't convert symbol to string
`your symbol is ${sym}`
// TypeError: can't convert symbol to string

/**
* Symbol 可以显示转为字符串，值也可以转为布尔值，但是不能转为数值。
**/
let sym = Symbol("My symbol");
String(sym) // 'Symbol(My symbol)'
sym.toString() // 'Symbol(My symbol)'

Boolean(sym) // true
!sym  // false
if (sym) {
  // ...
}

Number(sym) // TypeError
sym + 2 // TypeError

```

### 10.2 作为属性名的 Symbol

由于每一个 Symbol 值都是不相等的，这意味着 Symbol 值可以作为标识符，用于对象的属性名，就能保证不会出现同名的属性。

```js
let mySymbol = Symbol();

//第一种写法
let a = {};
a[mySymbol] = "Hello!";

//第二种写法
let a = {
    [mySymbol]: 'Hello!'
};

//第三种写法
let a = {};
Object.defineProperty(a, mySymbol, {value: "Hello!"});

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"
```
```js
/**
* Symbol 值作为对象属性名时，不能用点运算符。
**/
const mySymbol = Symbol();
const a = {};
a.mySymbol = "A";   //根本没用到 Symbol对象，点运算符后面总是字符串
a[mySymbol]     //undefined
a["mySymbol"]   //"A"
a.mySymbol      //"A"
```

```js
/**
* 同理，在对象的内部，使用 Symbol 值定义属性时，Symbol 值必须放在方括号之中。
**/
let s = Symbol();
let k = Symbol();
let obj = {
    [s]: function(arg) {
        //...
    }
    [k](arg) {  //采用增强的对象写法
        //...
    }
}
obj[k](123);
```

```js
/**
* Symbol 类型还可以用于定义一组常量，保证这组常量的值都是不相等的。
**/
const log = {};

log.levels = {
  DEBUG: Symbol('debug'),
  INFO: Symbol('info'),
  WARN: Symbol('warn')
};
console.log(log.levels.DEBUG, 'debug message');
console.log(log.levels.INFO, 'info message');
```

### 10.3 实例：消除魔术字符串

魔术字符串：代码中的多次出现，与代码形成强耦合的常量。

例如代码中多次出现`Triangle`这个字符常量，那就提取出来，放在一个类里统一管理。
```js
const shapeType = {
  triangle: 'Triangle'
};

//更可以这样
const shapeType = {
  triangle: Symbol()
};

shapeType.triangle
```

### 10.4 属性名的遍历

Symbol 作为属性名，该属性不会出现在 for...in 、 for...of 循环中，也不会被 Object.keys() 、 Object.getOwnPropertyNames() 、 JSON.stringify() 返回。

但是，它也不是私有属性，有一个 Object.getOwnPropertySymbols 方法，可以获取指定对象的所有 Symbol 属性名。

```js
const obj = {};

let foo = Symbol("foo");

Object.defineProperty(obj, foo, {
  value: "foobar",
});

for (let i in obj) {
  console.log(i); // 无输出
}

Object.getOwnPropertyNames(obj)
// []

Object.getOwnPropertySymbols(obj)
// [Symbol(foo)]
```

```js
/**
* 另一个新的 API，Reflect.ownKeys方法可以返回所有类型的键名，包括常规键名和 Symbol 键名。
**/

let obj = {
  [Symbol('my_key')]: 1,
  enum: 2,
  nonEnum: 3
};

Reflect.ownKeys(obj)
//  ["enum", "nonEnum", Symbol(my_key)]
```

### 10.5 Symbol.for() 、 Symbol.keyFor()

1. Symbol.for() 

重新使用同一个 Symbol 值。

它接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 Symbol 值。如果有，就返回这个 Symbol 值，否则就新建并返回一个以该字符串为名称的 Symbol 值。

Symbol.for() 与 Symbol() 这两种写法，都会生成新的 Symbol。它们的区别是，前者会被登记在全局环境中供搜索，后者不会。
```js
let s1 = Symbol.for('foo');
let s2 = Symbol.for('foo');     //搜索到
s1 === s2 // true
//
let s1 = Symbol('foo');
let s2 = Symbol.for('foo');     //搜索到
s1 === s2 // false
```

2. Symbol.keyFor()

返回一个已登记的 Symbol 类型值的 key 。

```js
let s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"

let s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined
```

### 10.6 实例：模块的 Singleton 模式

### 10.7 内置的 Symbol 值
#### 10.7.1 Symbol.hasInstance
#### 10.7.2 Symbol.isConcatSpreadable
#### 10.7.3 Symbol.species
#### 10.7.4 Symbol.match
#### 10.7.5 Symbol.replace
#### 10.7.6 Symbol.search
#### 10.7.7 Symbol.split
#### 10.7.8 Symbol.iterator
#### 10.7.9 Symbol.toPrimitive
#### 10.7.10 Symbol.toStringTag
#### 10.7.11 Symbol.unscopables

### 10.8 Symbol.prototype.description

创建 Symbol 的时候，可以添加一个描述。ES2019 提供了一个实例属性 description ，直接返回 Symbol 的描述。
```js
const sym = Symbol('foo');

sym.description // "foo"
```

## 第十一章 Set 和 Map 数据解构

### 11.1 Set

#### 11.1.1 基本用法

成员的值都是唯一的，没有重复的值。

Set 本身是一个构造函数，用来生成 Set 数据结构。

```js
const s = new Set();

[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3 5 4
```

```js
/**
* Set函数可以接受一个数组（或者具有 iterable 接口的其他数据结构）作为参数，用来初始化。
**/

// 例一
const set = new Set([1, 2, 3, 4, 4]);
[...set]    // 去除数组的重复成员
// [1, 2, 3, 4]

// 例二
const items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
items.size // 5

//去除字符串里面的重复字符
[...new Set('ababbc')].join('')
// "abc"
```

#### 11.1.2 Set 实例的属性和方法

Set 结构的实例有以下属性。
- Set.prototype.constructor：构造函数，默认就是Set函数。
- Set.prototype.size：返回Set实例的成员总数。

Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。

四个操作方法
- add(value)：添加某个值，返回 Set 结构本身。
- delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
- has(value)：返回一个布尔值，表示该值是否为Set的成员。
- clear()：清除所有成员，没有返回值。

#### 11.1.3 遍历操作

Set 结构的实例有四个遍历方法，可以用于遍历成员。
- keys()：返回键名的遍历器
- values()：返回键值的遍历器
- entries()：返回键值对的遍历器
- forEach()：使用回调函数遍历每个成员

需要特别指出的是，Set的遍历顺序就是插入顺序。

keys方法、values方法、entries方法返回的都是遍历器对象（详见《Iterator 对象》一章）。由于 Set 结构没有键名，只有键值（或者说键名和键值是同一个值），所以 keys 方法和 values 方法的行为完全一致。

### 11.2 WeakSet

与 Set 的两个区别：
- WeakSet 的成员**只能是对象**，而不能是其他类型的值。
- 其次，WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中。

ES6 规定 WeakSet 不可遍历。

```js
const ws = new WeakSet();
ws.add(1)
// TypeError: Invalid value used in weak set
ws.add(Symbol())
// TypeError: invalid value used in weak set
const b = [3, 4];
const ws = new WeakSet(b);
// Uncaught TypeError: Invalid value used in weak set(…)

const a = [[1, 2], [3, 4]];
const ws = new WeakSet(a);
// WeakSet {[1, 2], [3, 4]}
```

WeakSet 结构有以下三个方法。（无 forEach）（无 size 属性）
- WeakSet.prototype.add(value)：向 WeakSet 实例添加一个新成员。
- WeakSet.prototype.delete(value)：清除 WeakSet 实例的指定成员。
- WeakSet.prototype.has(value)：返回一个布尔值，表示某个值是否在 WeakSet 实例之中。

### 11.3 Map

#### 11.3.1 含义和基本用法

JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是传统上只能用字符串当作键。这给它的使用带来了很大的限制。

ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，*各种类型的值（包括对象）都可以当作键*。

```js
let map = new Map();

map.set(-0, 123);
map.get(+0) // 123

map.set(true, 1);
map.set('true', 2);
map.get(true) // 1

map.set(undefined, 3);
map.set(null, 4);
map.get(undefined) // 3

map.set(NaN, 123);
map.get(NaN) // 123
```

#### 11.3.2 实例的属性和操作方法

Map 结构的实例有以下属性和操作方法。
- size 属性
- set(key, value)
- get(key)
- has(key)
- delete(key)
- clear()

#### 11.3.3 遍历方法

Map 结构原生提供三个遍历器生成函数和一个遍历方法。
- keys()：返回键名的遍历器。
- values()：返回键值的遍历器。
- entries()：返回所有成员的遍历器。
- forEach()：遍历 Map 的所有成员。

#### 11.3.4 与其他数据结构的互相转换

1. Map 转为数组
```js
//使用扩展运算符
const myMap = new Map()
  .set(true, 7)
  .set({foo: 3}, ['abc']);
[...myMap]    //[[true, 7], [{foo: 3}, ['abc']]]
```

2. 数组 转为 Map
```js
const myMap = new Map(
  [true, 7],
  [{foo: 3}, ['abc']]
);
// Map {
//   true => 7,
//   Object {foo: 3} => ['abc']
// }
```

3. Map 转为对象

如果所有 Map 的键都是字符串，它可以无损地转为对象。如果有非字符串的键名，那么这个键名会被转成字符串，再作为对象的键名。

```js
function strMapToObj(strMap) {
  let obj = Object.create(null);
  for (let [k,v] of strMap) {
    obj[k] = v;
  }
  return obj;
}

const myMap = new Map()
  .set('yes', true)
  .set('no', false);
strMapToObj(myMap)
// { yes: true, no: false }
```

4. 对象转为 Map
```js
function objToStrMap(obj) {
  let strMap = new Map();
  for(let k in Object.keys(obj)) {
    strMap.set(k, obj[k]);
  }
  return strMap;
}

objToStrMap({yes: true, no: false})
// Map {"yes" => true, "no" => false}
```

5. Map 转为 JSON

```js
/**
* Map 转为 JSON 要区分两种情况。一种情况是，Map 的键名都是字符串，这时可以选择转为对象 JSON。
**/
function strMapToJson(strMap) {
  return JSON.stringify(strMapToObj(strMap));
}

let myMap = new Map().set('yes', true).set('no', false);
strMapToJson(myMap)
// '{"yes":true,"no":false}'


/**
* 另一种情况是，Map 的键名有非字符串，这时可以选择转为数组 JSON。
**/
function mapToArrayJson(map) {
  return JSON.stringify([...map]);
}

let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
mapToArrayJson(myMap)
// '[[true,7],[{"foo":3},["abc"]]]'
```

6. JSON 转为 Map

```js
/**
* JSON 转为 Map，正常情况下，所有键名都是字符串。
**/
function jsonToStrMap(jsonStr) {
  return objToStrMap(JSON.parse(jsonStr));
}

jsonToStrMap('{"yes": true, "no": false}')
// Map {'yes' => true, 'no' => false}

/**
* 但是，有一种特殊情况，整个 JSON 就是一个数组，且每个数组成员本身，又是一个有两个成员的数组。这时，它可以一一对应地转为 Map。这往往是 Map 转为数组 JSON 的逆操作。
**/
function jsonToMap(jsonStr) {
  return new Map(JSON.parse(jsonStr));
}

jsonToMap('[[true,7],[{"foo":3},["abc"]]]')
// Map {true => 7, Object {foo: 3} => ['abc']}
```

### 11.4 WeakMap

#### 11.4.1 含义

WeakMap 与 Map 的区别有两点。
- WeakMap只接受对象作为键名（null除外），不接受其他类型的值作为键名。
- WeakMap的键名所指向的对象，不计入垃圾回收机制。（自动清除）
```js
const map = new WeakMap();
map.set(1, 2)
// TypeError: 1 is not an object!
map.set(Symbol(), 2)
// TypeError: Invalid value used as weak map key
map.set(null, 2)
// TypeError: Invalid value used as weak map key
```

WeakMap的**设计目的**在于，有时我们想在某个对象上面存放一些数据，但是这会形成对于这个对象的引用。请看下面的例子。
```js
const e1 = document.getElementById('foo');
const e2 = document.getElementById('bar');
const arr = [
  [e1, 'foo 元素'],   //这就形成了arr对e1和e2的引用。
  [e2, 'bar 元素'],
];
//一旦不再需要这两个对象，我们就必须手动删除这个引用，否则垃圾回收机制就不会释放e1和e2占用的内存。
// 不需要 e1 和 e2 的时候
// 必须手动删除引用
arr [0] = null;
arr [1] = null;
```
#### 11.4.2 WeakMap 的语法

WeakMap 与 Map 在 API 上的区别主要是两个，
- 一是没有遍历操作（即没有keys()、values()和entries()方法），也没有size属性。
- 二是无法清空，即不支持clear方法

#### 11.4.3 WeakMap 示例

#### 11.4.4 WeakMap 的用途

- WeakMap 应用的典型场合就是 DOM 节点作为键名
- WeakMap 的另一个用处是部署私有属性


## 第十二章 Proxy

### 12.1 概述

Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。

```js
//var proxy = new Proxy(target, handler);
//target 参数表示所要拦截的目标对象
//handler 参数也是一个对象，用来定制拦截行为。
var proxy = new Proxy({}, {
  //get 参数 目标对象和所要访问的属性
  get: function(target, property) {
    return 35;
  }
  //可以设置拦截多个操作
  //...
});

//由于拦截函数总是返回 35 ，所以访问任何属性都得到 35
proxy.time // 35
proxy.name // 35
proxy.title // 35
```

下面是 Proxy 支持的拦截操作一览，一共 13 种。

- get(target, propKey, receiver)：拦截对象属性的读取，比如 proxy.foo 和 proxy['foo']。
- set(target, propKey, value, receiver)：拦截对象属性的设置，比如 proxy.foo = v 或 proxy['foo'] = v，返回一个布尔值。
- has(target, propKey)：拦截 propKey in proxy 的操作，返回一个布尔值。
- deleteProperty(target, propKey)：拦截delete proxy[propKey]的操作，返回一个布尔值。
- ownKeys(target)：拦截Object.getOwnPropertyNames(proxy)、Object.getOwnPropertySymbols(proxy)、Object.keys(proxy)、for...in循环，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而Object.keys()的返回结果仅包括目标对象自身的可遍历属性。
- getOwnPropertyDescriptor(target, propKey)：拦截Object.getOwnPropertyDescriptor(proxy, propKey)，返回属性的描述对象。
- defineProperty(target, propKey, propDesc)：拦截Object.defineProperty(proxy, propKey, propDesc)、Object.defineProperties(proxy, propDescs)，返回一个布尔值。
- preventExtensions(target)：拦截Object.preventExtensions(proxy)，返回一个布尔值。
- getPrototypeOf(target)：拦截Object.getPrototypeOf(proxy)，返回一个对象。
- isExtensible(target)：拦截Object.isExtensible(proxy)，返回一个布尔值。
- setPrototypeOf(target, proto)：拦截Object.setPrototypeOf(proxy, proto)，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截。
- apply(target, object, args)：拦截 Proxy 实例作为函数调用的操作，比如proxy(...args)、proxy.call(object, ...args)、proxy.apply(...)。
- construct(target, args)：拦截 Proxy 实例作为构造函数调用的操作，比如new proxy(...args)。

### 12.2 Proxy 实例的方法
#### 12.2.1 get()
#### 12.2.2 set()
#### 12.2.3 apply()
#### 12.2.4 has()
#### 12.2.5 construct()
#### 12.2.6 deleteProperty()
#### 12.2.7 defineProperty()
#### 12.2.8 getOwnPropertyDescriptor()
#### 12.2.9 getPrototypeOf()
#### 12.2.10 isExtensible()
#### 12.2.11 ownKeys()
#### 12.2.12 preventExtensions()
#### 12.2.13 setPrototypeOf()

### 12.3 Proxy.revocable()

Proxy.revocable 方法返回一个可取消的 Proxy 实例。

Proxy.revocable 的一个使用场景是，目标对象不允许直接访问，必须通过代理访问，一旦访问结束，就收回代理权，不允许再次访问。

（revocable 美 [ˈrɛvəkəbəl]  adj.可废止的，可撤回的）

```js
let target = {};
let handler = {};
//返回一个对象，该对象的 proxy 属性是 Proxy 实例， revoke 属性是一个函数，可以取消 Proxy 实例。
let {proxy, revoke} = Proxy.revocable(target, handler);

proxy.foo = 123;
proxy.foo   //123

//当执行revoke函数之后，再访问Proxy实例，就会抛出一个错误。
revoke();
proxy.foo // TypeError: Revoked
```

### 12.4 this 问题

在 Proxy 代理的情况下，目标对象内部的this关键字会指向 Proxy 代理。因此无法保证与目标对象的行为一致。
```js
//例如，原生对象的内部属性，只有通过正确的this才能拿到，所以 Proxy 也无法代理这些原生对象的属性
const target = new Date();
const handler = {};
const proxy = new Proxy(target, handler);

//getDate 方法只能在 Date 对象实例上面拿到，如果 this 不是 Date 对象实例就会报错。
proxy.getDate();
// TypeError: this is not a Date object.

//可以通过绑定 this 到原始对象，解决问题。
const target = new Date('2015-01-01');
const handler = {
  get(target, prop) {
    if (prop === 'getDate') {
      return target.getDate.bind(target);
    }
    return Reflect.get(target, prop);
  }
};
const proxy = new Proxy(target, handler);

proxy.getDate() // 1
```

```js
/**
* 
**/
```