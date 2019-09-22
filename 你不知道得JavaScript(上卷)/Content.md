## 第 1 章　作用域是什么

### 1.1 编译原理

传统编译步骤：
1. 分词/词法分析（Tokenizing/Lexing）：这个过程会将由字符组成的字符串分解成（对编程语言来说）有意义的代码块，这些代码块被称为**词法单元**（token）。
2. 解析/语法分析（Parsing）：这个过程是将词法单元流（数组）转换成一个由元素逐级嵌套所组成的代表了程序语法结构的树。这个树被称为“抽象语法树”（Abstract Syntax Tree，**AST**）。
3. 代码生成：将AST转换为**可执行代码**的过程称被称为代码生成。这个过程与语言、目标平台等息息相关。

JavaScript的编译过程不是发生在构建之前的，大部分情况下编译发生在代码**执行前**的几微秒（甚至更短！）的时间内。

### 1.2 结合作用域

作用域是一套**规则**，用于确定在何处以及如何查找变量（标识符）。如果查找的目的是对变量进行**赋值**，那么就会使用**LHS**查询；如果目的是**获取**变量的值，就会使用**RHS**查询（记忆：Retrieve His Source Value）。

JavaScript引擎首先会在代码执行前对其进行编译，在这个过程中，像`var a = 2`这样的声明会被分解成两个独立的步骤：
1. 首先，`var a`在其作用域中声明新变量。这会在最开始的阶段，也就是代码执行前进行。
2. 接下来，`a = 2`会查询（LHS查询）变量a并对其进行赋值。

LHS和RHS查询都会在当前执行作用域中开始，找不到就向上找。

不成功的`RHS`引用会导致抛出ReferenceError异常。不成功的`LHS`引用会导致自动隐式地创建一个全局变量（非严格模式下），该变量使用LHS引用的目标作为标识符，或者抛出ReferenceError异常（严格模式下）。

## 第 2 章　词法作用域

作用域共有两种主要的*工作模型*：
- 词法作用域（最普遍）：就是定义在**词法阶段**（编译器的第一个工作阶段）的作用域。
- 动态作用域（比如Bash脚本、Perl中的一些模式等）

**欺骗词法**作用域会导致性能下降：（JavaScript引擎会在编译阶段进行数项的性能优化。其中有些优化依赖于能够根据代码的词法进行静态分析，并预先确定所有变量和函数的定义位置，才能在执行过程中快速找到标识符。（欺骗词法否定了这些优化和假设））
- eval：`eval("var a = 3;")` 
- with

## 第 3 章　函数作用域和块作用域

### 3.3 函数作用域

*函数声明*和*函数表达式*之间最重要的区别是它们的名称标识符将会绑定在何处。
- 第一个片段中 foo 被绑定在 所在作用域 中，可以直接通过 foo() 来调用它。
- 第二个片段中 foo 被绑定在 函数表达式**自身的函数中** 而不是所在作用域中。换句话说，`(function foo(){ .. })`作为函数表达式意味着foo只能在`..`所代表的位置中被访问，外部作用域则不行。
```js
var a = 2;
/*
    问题
*/
// 首先，必须声明一个具名函数foo()，意味着foo这个名称本身“污染”了所在作用域。
// 其次，必须显式地通过函数名（foo()）调用这个函数才能运行其中的代码。
function foo() {
    var a = 3;
    console.log(a); // 3
}
foo();

console.log(a); // 2
```

```js
var a = 2;
// JavaScript 提供了能够同时解决这两个问题的方案

// 函数会被当作 **函数表达式** 而不是一个标准的 函数声明 来处理。
(function foo() {
    var a = 3;
    console.log(a); // 3
})();

console.log(a);     // 2
```

### 3.4　块作用域

块作用域是一个用来对之前的**最小授权原则**（变量的声明应该距离使用的地方越近越好，并最大限度地本地化）进行扩展的**工具**，将代码从在函数中隐藏信息扩展为在块中隐藏信息。

但可惜，表面上看JavaScript并没有块作用域的相关功能。

let 关键字可以将变量绑定到所在的任意作用域中（通常是`{ .. }`内部）。只要声明是有效的，在声明中的任意位置都可以使用`{ .. }`括号来为let创建一个用于绑定的块。

```js
var foo = true;

if (foo) {
    // 显式块的好处：如果需要对其进行重构，整个块都可以被方便地移动而不会对外部if声明的位置和语义产生任何影响。
    {
        let bar = foo * 2;
        bar = something( bar ); 
        console.log( bar );
    }
}

console.log( bar ); // ReferenceError
```

## 第 4 章　提升

## 第 5 章　作用域闭包

**闭包定义**：当函数可以记住并访问所在的词法作用域时，就产生了闭包，即使函数是在当前词法作用域**之外执行**。

```js
function foo() {
    var a = 2;
    // 函数 bar 可以访问外部作用域中的变量 a
    function bar() {
        console.log(a); // 2
    }

    bar();
}

foo();

/*
    这是闭包吗？
*/
// 技术上来讲，也许是。但根据前面的定义，确切地说并不是。
// 我认为最准确地用来解释bar()对a的引用是 词法作用域的**查找规则**的结果，而这些规则只是闭包的一部分。（但却是非常重要的一部分！）
```

```js
function foo() {
    var a = 2;

    function bar() { 
        console.log( a );
    }

    return bar;     // !!!返回函数
}

var baz = foo();    // !!!闭包阻止了垃圾回收，拥有涵盖 foo() 内部作用域的闭包，使得该作用域能够一直存活。

baz(); //闭包，bar 在其词法作用域*以外*的地方被执行
```

## 第 1 章　关于this

## 第 2 章　this 全面解析

### 2.2 绑定规则

找到调用位置，然后判断需要应用下面**四条**规则中的哪一条

#### 2.2.1 默认绑定

`foo()`是直接使用不带任何修饰的函数引用进行调用的。

```js
var a = 1;

function outer() {
    // 非严格模式
    function foo() { 
        console.log(this.a);    // 函数调用时应用了this的默认绑定，因此this指向全局对象。
    }

    var a = 2;

    foo();
}

outer();    // 1
```

#### 2.2.2 隐式绑定

调用位置是否有上下文对象，或者说是否被某个对象**拥有**或者**包含**。

```js
function foo() { 
    console.log( this.a );
}

var obj2 = { 
    a: 42,
    foo: foo 
};

var obj1 = { 
    a: 2,
    obj2: obj2 
};

obj1.obj2.foo(); // 42

/**
    隐式丢失
*/
var bar = obj2.foo; // 函数别名
var a = "global";
bar();  // "global"
// 还有回调函数丢失this绑定是非常常见的。
```

#### 2.2.3 显式绑定

call、apply、bind

#### 2.2.4 new 绑定

使用new来调用函数，或者说发生构造函数调用时，会自动执行下面的操作。{!!!}
1. 创建（或者说构造）一个全新的对象。
2. 这个新对象会被执行`[[原型]]`连接。
3. 这个新对象会绑定到函数调用的this。
4. 如果函数没有返回其他对象，那么new表达式中的函数调用会自动返回这个新对象。

### 2.3　优先级

**new绑定>显式绑定>隐式绑定>默认绑定**

**判断this**

现在我们可以根据优先级来判断函数在某个调用位置应用的是哪条规则。可以按照下面的顺序来进行判断：
1. 函数是否在new中调用（new绑定）？如果是的话this绑定的是新创建的对象。

    `var bar = new foo()`
1. 函数是否通过call、apply（显式绑定）或者硬绑定调用？如果是的话，this绑定的是指定的对象。

    `var bar = foo.call(obj2)`
3. 函数是否在某个上下文对象中调用（隐式绑定）？如果是的话，this绑定的是那个上下文对象。

    `var bar = obj1.foo()`
4. 如果都不是的话，使用默认绑定。如果在严格模式下，就绑定到undefined，否则绑定到全局对象。

    `var bar = foo()`

### 2.4　绑定例外

#### 2.4.1　被忽略的this

```js
function foo(a,b) {
    console.log( "a:" + a + ", b:" + b );
}

// 把数组“展开”成参数
foo.apply( null, [2, 3] ); // a:2, b:3

// 使用 bind(..) 进行*柯里化*
var bar = foo.bind( null, 2 ); 
bar( 3 ); // a:2, b:3
```

**更安全的 this**：如果使用 null 忽略 this 绑定的话，那默认绑定规则会把this绑定到全局对象，导致不可预计的后果。“DMZ”（demilitarized zone，非军事区）对象---空的非委托的对象`var ø = Object.create( null );`（和 {} 很像，但是比它更空，并不会创建Object.prototype这个委托）

#### 2.4.2　间接引用

#### 2.4.3　软绑定

### 2.5　this词法

无法使用这些规则的特殊函数类型：箭头函数。 

## 第 3 章　对象

对于`Object`、`Array`、`Function和RegExp`（正则表达式）来说，无论使用文字形式还是构造形式，它们都是对象，不是字面量。

`Object.keys(..)`会返回一个数组，包含所有可枚举属性，`Object.getOwnPropertyNames(..)`会返回一个数组，包含所有属性，无论它们是否可枚举。

`in`和`hasOwnProperty(..)`的区别在于是否查找`[[Prototype]]`链，然而，`Object.keys(..)`和`Object.getOwnPropertyNames(..)`都只会查找对象直接包含的属性。

### 3.4 遍历

数组有内置的@@iterator，因此for..of可以直接应用在数组上。

```js
var myArray = [ 1, 2, 3 ];
// 我们使用内置的@@iterator来手动遍历数组，看看它是怎么工作的
var it = myArray[Symbol.iterator]();

it.next(); // { value:1, done:false }
it.next(); // { value:2, done:false }
it.next(); // { value:3, done:false }
it.next(); // { done:true }
```

当然，你可以给任何想遍历的对象定义 @@iterator ，然后使用 for...of 遍历。

```js
var myObject = { 
    a: 2,
    b: 3 
};

Object.defineProperty( myObject, Symbol.iterator, { 
    enumerable: false,
    writable: false,
    configurable: true,
    value: function() { 
        var o = this;
        var idx = 0;
        var ks = Object.keys( o ); 
        return {
            next: function() {
                return {
                    value: o[ks[idx++]], 
                    done: (idx > ks.length)
                }; 
            }
        }; 
    }
} );

// 手动遍历myObject
var it = myObject[Symbol.iterator](); 
it.next(); // { value:2, done:false }
it.next(); // { value:3, done:false }
it.next(); // { value:undefined, done:true }

// 用for..of遍历myObject
for (var v of myObject) { 
    console.log( v );
}
// 2
// 3
```

## 第 4 章　混合对象“类”

### 4.4 混入

## 第 5 章　原型

## 第 6 章　行为委托