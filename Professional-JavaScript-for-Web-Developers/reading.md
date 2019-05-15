# JavaScript高级程序设计

## 第一章 JavaScript简介

### 1.1 JavaScript简史

1. 1995.02（左右）， Netscape 开始着手为 Netscape Navigator2 开发一种名为 LiveScript 的脚本语言。与 Sun 公司建立了开发联盟。为了搭上媒体热炒 Java 的顺风车，改名为 JavaScript 。
    -  JavaScript1.0 大获成功， Netscape 随即在 NetScape Navigator3 中发布 JavaScript1.1 。
2. 1996.08（左右），NetScape Navigator3 发布不久，微软就在其 InterNet Explorer3 加入了名为 JScript 的 JavaScript 实现。
    - 此时有3个不同版本的 JavaScript 版本
        -  NetScape Navigator 中的 JavaScript
        -  Internet Explorer 中的 Jscript
        -  ScriptEase 的 CEnvi
    -  JavaScript 标准化被提上日程。
3. 1997年，以 JavaScript 1.1 为蓝本的建议被提交给了欧洲计算机制造商协会（ ECMA ， European Computer Manufactures Association ）
    -  经过数月的努力完成了 ECMA-262 ---定义一种名为 ECMAScript 的新脚本语言的标准。
4. 1998年，ISO/IEC（国标标准化组织和国际电工委员会）也采用了 ECMAScript 作为标准。
    - 从此后，浏览器开发商就开始致力于将 ECMAScript 作为各自 JavaScript 实现的基础，也在不同程度上取得成功。

### 1.2 JavaScript实现

Javascript三部分组成
- ECMAScript，由ECMA-262定义，提供核心语言功能（语法、类型、语句、关键字、保留字、操作符、对象）
- DOM，文档对象模型，提供访问和操作**网页**的方法和接口（W3C的推荐标准---1级、2级、3级）
- BOM，浏览器对象模型，提供与**浏览器**交互的方法和接口

## 第二章 在 HTML 中使用 JavaScript

### 2.1  script 元素

主要介绍了一下`<script>`标签的使用

#### 2.1.1 标签的位置

一般全部 JavaScript 引用放在 `<body>` 元素内容的后面。

#### 2.1.2 延迟脚本

`defer="defer"`

#### 2.1.3 异步脚本

`async`

#### 2.4 noscript 元素

使用`<noscript>`元素可以指定在不支持脚本的浏览器中显示的替代内容。

## 第三章 基本概念

语法、数据类型、流控制语句、理解函数

### 3.1 语法

标识符、注释、分号结尾、与Java基本相同。


### 3.4 数据类型

JavaScript 变量可以用来保存两种类型的值：基本类型值和引用类型值。

五种*基本*数据类型：Undefined 、 Null 、 Boolean 、 Number 、 String

#### 3.4.1 typeof 操作符

例：
```
var message = "some string";
alert(typeof(message));     // typeof是一个操作符而不是函数，括号不是必须！！
alert(typeof message);      // "string"
```

### 3.5 操作符

1. 一元操作符（ ++, -- ）
2. 位操作符（ ~, &, |, ^, <<, >>> ）
3. 布尔操作符（ !, &&, || ）
4. 乘性操作符（ *, /, % ）
5. 加性操作符（ +, - ）
6. 关系操作符（ <, >, <=, >= ）
7. 相等操作符（ ==, !=, ===, !== ）
8. 条件操作符（ ?: ）
9. 赋值操作符（ =, *=, +=, /=, -=, %=, <<=, >>= ）
10. 逗号操作符（ , ）（可以在一条语句中执行多个操作）

#### 3.5.7 相等操作符

全等（===）只在两个操作数在比较时，不能发生转换。
```
var result1 = ("55" == 55);      //true ，因为转换后相等。
var result2 = ("55" === 55);    //false ，因为不同的数据类型不相等。
```

### 3.6 语句

1. if
2. do-while
3. while
4. for
5. for-in
6. break, continue
7. switch
8. with（作用是将代码的作用域设置到一个特定的对象中。）
9. ~~label~~

### 3.7 函数

#### 3.7.1 理解函数
ECMAScript 函数不介意传递进来多少个参数，即使你定义的函数只接受两个，调用时也不未必一定要传递两个参数。可以传递一个、三个甚至不传递参数。

在函数体内可以通过 **arguments** 对象来访问这个参数数组。

#### 3.7.2 没有重载

在ECMAScript中定义两个名字相同的函数，则该名字只属于后定义的函数。

## 第四章 变量、作用域和内存问题

### 4.1 基本类型和引用类型

#### 4.1.1 动态的属性

对于**引用类型**的值，我们可以为其增删改属性和方法；但是不能给**基础类型**的值添加属性（尽管不会导致任何错误）

#### 4.1.4 检测类型

确定一个值是哪种**基本类型**可以使用 typeof 操作符，而确定一个值是哪种**引用类型**可以使用 instanceof 操作符。
```
result = variable instanceof constructor    //返回 true or false ； typeof 返回基本类型字符串
```

### 4.2 执行环境及作用域

在web浏览器中，全局执行环境被认为是 window 对象，因此所有的全局变量和函数都是作为window对象的属性和方法创建的。

某个环境中的所有代码执行完毕，环境销毁，里面的变量和函数定义随之销毁。

每个函数都有自己的执行环境。

当代码在一个环境中执行时，会创建变量对象的一个作用域链（ scope chain ）。（保证对执行环境有权访问的所有变量和函数的有序访问。）

*标识符解析*是沿着作用域链一级一级地搜索标识符的过程。（搜索不到则搜索上一级）

例：

```
var color = "blue";

function changeColor() {
    //定义
    var anotherColor = "red";
    //定义
    function swapColor() {
        var tempColor = anotherColor;
        anotherColor = color;
        color = tempColor;
        //这里可以访问 color 、 anotherColor 和 tempColor （所有变量）
    }

    //调用
    swapColor();

    //这里可以访问 color 、 anotherColor（不能访问tempColor）
}

//调用
changeColor();

//只能访问 color
```

以上涉及3个执行环境：全局环境、 changeColor() 的局部环境和 swapColor() 的局部环境。

![](https://raw.githubusercontent.com/514723273/.md-Pictures/master/window.png)

#### 4.2.2 没有块级作用域

没有块级作用域（if for 里面定义的，出去之后，依旧存在当前的环境），只有function才定义作用域（环境）

### 4.3 垃圾收集

垃圾收集器会按照固定的时间间隔，周期性地执行---找出不再使用的变量，释放其占用的内存。

#### 4.3.1 标记清除

当变量进入环境（例如在一个函数中声明），将其标记为“进入环境”。
当变量离开环境时，将其标记为“离开环境”。

标记变量的方式不重要，关键在于采取什么策略。

到2008年为止，大部分主流浏览器都使用标记清除式的垃圾收集策略。

#### 4.3.2 引用计数

不太常见。

跟踪记录每个值被引用的次数。

将引用类型值赋值给变量， +1 。

同一个值（地址）又被赋给另一个变量， +1 。

这个变量取得另一个值， -1 。

当这个值的引用次数变为0时，说明没办法再访问这个值了，回收。

**严重问题**：循环引用！

```
function problem() {
    var o1 = new Object();
    var o2 = new Object();

    o1.someOtherObject = o2;    //引用了o2
    o2.anotherObject = o1;      //引用了o1
}
```

为了避免循环引用，最好手工断开引用。
```
o1.someOtherObject = null;
o2.anotherObject = null;
```

#### 4.3.3 性能问题

IE 的垃圾收集器是根据内存分配量运行的，即达到临界值触发回收。（例如256个变量或者64KB字符串）（声名狼藉---垃圾回收频繁）

IE7 做出改变：各项临界值初始与 IE6 相等。如果垃圾回收例程回收的内存分配量低于15%（每次回收的太少了，太频繁了），则变量、字面量和数组元素的临界值就会加倍。如果例程回收了85%的内存分配量，则将各种临界值重回默认值。

#### 4.3.4 管理内存

为什么要管理？最主要的是，分配给Web浏览器的可用内存数量少（相比桌面软件）。内存限制不仅会影响给变量分配内存，同时还会影响调用栈和一个线程中同时执行的语句数量。

**优化内存占用的最佳方式**：执行中的代码只保存必要的数据。一旦数据不再有用，手动设置为null。（解除引用）

### 4.4 小结

- 基本类型值在内存中占据固定大小的空间，因此被保存在**栈**内存中
- 引用类型的值是对象，保存在**堆**内存中
- 全局环境不能直接访问局部环境中的任何数据