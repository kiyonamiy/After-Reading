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

Undefined 、 Null 、 Boolean 、 Number 、 String 、Object

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

