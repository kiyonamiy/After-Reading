# ECMAScript 6入门

## 第一章 ECMAScript 6 简介（跳）

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