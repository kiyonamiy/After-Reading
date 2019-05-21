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

### 2.1 script 元素

主要介绍了一下`<script>`标签的使用

#### 2.1.1 标签的位置

一般全部 JavaScript 引用放在 `<body>` 元素内容的后面。

#### 2.1.2 延迟脚本

`defer="defer"`

#### 2.1.3 异步脚本

`async`

### 2.4 noscript 元素

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
```js
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
```js
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
```js
result = variable instanceof constructor    //返回 true or false ； typeof 返回基本类型字符串
```

### 4.2 执行环境及作用域

在web浏览器中，全局执行环境被认为是 window 对象，因此所有的全局变量和函数都是作为window对象的属性和方法创建的。

某个环境中的所有代码执行完毕，环境销毁，里面的变量和函数定义随之销毁。

每个函数都有自己的执行环境。

当代码在一个环境中执行时，会创建变量对象的一个作用域链（ scope chain ）。（保证对执行环境有权访问的所有变量和函数的有序访问。）

*标识符解析*是沿着作用域链一级一级地搜索标识符的过程。（搜索不到则搜索上一级）

例：

```js
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

![作用域示意图](https://raw.githubusercontent.com/514723273/.md-Pictures/master/window.png)

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

```js
function problem() {
    var o1 = new Object();
    var o2 = new Object();

    o1.someOtherObject = o2;    //引用了o2
    o2.anotherObject = o1;      //引用了o1
}
```

为了避免循环引用，最好手工断开引用。
```js
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

## 第五章 引用类型

### 5.1 Object 类型

创建 Object 实例的方法有两种。

第一种：使用 new 操作符后跟 Object 构造函数。
```js
var person = new Object();
person.name = "Kiyonami";
person.age = 20;
```

第二种：使用对象字面量表示法。（更青睐）
```js
var person = {
    name : "Kiyonami",
    age : 20,
    5 : true        //测试用，这里的 数值属性名 会自动转换为 字符串
};
```
```js
var person = {};    //不会调用 Object 构造函数
person.name = "Kiyonami";       //还可以person["name"]，但通常是点表示法
person.age = 20;
```

### 5.2 Array 类型

可以保存任何类型的数据。可以动态调整大小。

同样，创建 Array 实例的方法有两种。

第一种：使用 Array 构造函数。
```js
var colors1 = new Array();

var colors2 = new Array(3);  //创建 length 值为 3 的数组

var colors3 = new Array("Greg");    //创建一个包含 1 项字符串的数组（传递数值和传递其他值的不同）

var color4 = Array(3);  //可以省略 new 操作符。
```

第二种：使用数组字面量表示法。
```js
var colors = ["red", "blue", "green"];

var names = [];

var values = [1, 2,,];  //不要这样！这样会创建一个包含 3 或 4 项的数组（浏览器不同）
```

数组的 length 不是只读的！
```js
var colors = ["red", "blue", "green"];
colors.length = 2;
alert(colors[2]);   //undefined

colors[colors.length] = "black";    //可以这样，在末尾（位置 2 ）添加新项

colors[99] = "brown";
alert(colors.length);   //100，访问其他为 undefined
```

#### 5.2.1 检测数组

```js
if(Array.isArray(value)) {
    //对数组执行某些操作
}
```

#### 5.2.2 转换方法

如前所述，所有对象都具有 toString() （以逗号分隔的字符串）、 valueOf() （返回的还是数组）和toLocaleString() （经常返回与两个方法相同的值）（唯一区别，取得每一项的值，调用的是每一项的 toLocaleString ）方法。

可以使用 join() 方法，使用不同的分隔符来构建返回的字符串。
```js
var colors = ["red", "blue", "green"];
alert(colors.join("||"));   // red||blue||green，只接受一个参数---分隔符的字符串。
```

#### 5.2.3 栈方法

后入先出。`push()`（末端添加）和`pop()`（取出末端项）。

`push()`方法可以接受任意数量的参数。

#### 5.2.4 队列方法

先进先出。`push()`和`shift()`（取出前端项）。

也可以使用`unshift`（前端添加）和`pop()`，从相反的方向模拟队列。

#### 5.2.5 重排序方法

`reverse()`、`sort()`（默认升序，**比较的是字符串！**）

为了实现排序，`sort()`方法会调用每个数组项的 toString() 转型方法，比较得到的字符串，即使是数值。
```js
var values = [0, 1, 5, 10, 15];
values.sort();  //0, 1, 10, 15, 5，按字符串大小排序的
```

因此，`sort()`方法可以接收一个比较函数。
比较函数接收两个参数(a, b)，如果 a 应该位于 b 之*前*（不改变顺序），则返回一个负数。（即升序）
```js
function compare(a, b) {
    return a - b;
}

var values = [0, 1, 5, 10, 15];
values.sort(compare);   //接收一个比较函数
```

#### 5.2.6 操作方法

1. `concat()`，创建一个新数组，并将原数组的所有项添加到末尾。

    ```js
    var colors = ["red", "green", "blue"];  //原数组一直保持不变
    var colors1 = colors.concat();  //["red", "green", "blue"] ，不传递参数时，作用只是复制当前数组并返回副本
    var colors2 = conlors.concat("yellow", ["black", "brown"]); //["red", "green", "blue"， "yellow", "black", "brown"] ，传递一个或多个数组时，则会将每一项都添加到结果数组
    ```

2. `slice()`，选择原数组中的一段（一个或多个项）（切割），并创建一个新的数组存储。
    ```js
    var colors = ["red", "green", "blue"， "yellow", "purple"];
    var colors1 = colors.slice(1);  //["green", "blue"， "yellow", "purple"]
    var colors2 = colors.slice(1, 4);   //["green", "blue"， "yellow"] ，不包括第四项
    ```
3. `splice()`，（剪接、粘接），有很多种用法，主要用途是向数组中的中部插入项。
    - 删除：指定两个参数：要删除的第一项的位置和要删除的项数。例如， `splice(0, 2)`会删除数组的前两项。
    - 插入：提供三个参数：起始位置、0（要删除的项数）和要插入的项（可以多个）。例如，`splice(2, 0, "red", "green")`会从位置 2 开始插入两个字符串。
    - 替换：删除任意项，添加任意项。例如，`splice(2, 1, "red", "green")`，会删除当前数组的第二项，从位置 2 开始插入两个字符串。

    `splice()`方法都会返回一个数组，所有原数组的删除项（0则返回空数组）。

#### 5.2.7 位置方法

`indexOf()`和`lastIndexOf()`，接收两个参数：要查找的项，查找起点位置的索引（可选）。

#### 5.2.8 迭代方法

共五个方法。

每个方法接收两个参数：要在每一项上运行的函数，运行该函数的作用域对象（可选）。

传入的运行函数接收三个参数：数组项的值、该项在数组中的位置和数组对象本身。（item, index, array）

传入的函数会对数组中的每一项运行。（所有）

**五个方法都不会修改数组中的包含的值。**

- `every()`，数组中的每一项都满足条件，才返回true。
- `some()`，数组中只要有一项满足条件，就可以返回true。
- `filter()`，返回满足条件的所有项组成的数组。
- `map()`，将数组所有项进行操作，返回操作后的数组。
- `forEach`，没有返回值，只做遍历操作。

```js
var numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];

//false ，并没有每项都大于2
var everyResult = numbers.every(function(item, index, array) {
    return item > 2;
});     

//true ，有一个大于2的就行
var someResult = numbers.some(function(item, index, array) {
    return item > 2;
});

//[3, 4, 5, 4, 3] ，筛选出所有大于2的
var filterResult = numbers.filter(function(item, index, array) {
    return item > 2;
})

//[2, 4, 6, 8, 10, 8, 6, 4, 2] ，返回对所有项*2的结果
//这也是唯一一个对数组每项进行操作，其他都只是判断（大致意思）
var mapResult = numbers.map(function(item, index, array) {
    return item * 2;
})

//没有返回值
numbers.foreach(function(item, index, array) {
    //执行操作
})
```

#### 5.2.9 缩小方法

`reduce()`和`reduceRight()`，这两个方法都会迭代数组的所有项，构建一个最终返回的值。（后者从后往前，其他完全相同

这两个方法都接收两个参数：一个在每一项上调用的函数，作为缩小基础的初始值（可选）。

传入的函数接收四个参数：前一个值、当前值（项值）、项的索引和数组对象。这个函数返回的任何值都会作为第一个参数自动传给下一项。

第一次迭代发生在数组的第二项。

```js
var values = [1, 2, 3, 4, 5];
var sum = values.reduce(function(prev, cur, index, array) {
    return prev + cur;
}); //15 ，即不断累加的结果
```

### 5.3 Date 类型

```js
var now = new Date();   //自动获取当前日期和时间
```

如果想创建特定的日期时间对象，必须传入该日期的毫秒数。为了简化计算过程，提供两个方法：`Date.parse()` 、 `Date.UTC()`。

1. Date.parse()
    - 接收表示日期的字符串

2. Date.UTC()（可接收7个参数）（省略会取默认值）
    - 年份
    - 月份（ 0-11 ）
    - 日期（ 0-31 ）
    - 小时（ 0-23 ）
    - 分钟
    - 秒
    - 毫秒
3. Date.now()
    - 取得调用该方法的时间
```js
var someDate = new Date(Date.parse("May, 16, 2019"));   //Date.parse 尝试解析日期的字符串，返回毫秒数，若是解析失败，返回 NaN 。

var someDate1 = new Date("May, 16, 2019");  //直接传入字符串，后台会调用 Date.parse() ，二者等价

//GMT 时间 2000 年 1 月 1 日午夜 0 时
var someDate2 = new Date(Date.UTC(2000, 0));

//GMT 时间 2005 年 4 月 5 日下午 5:55:55
var someDate3 = new Date(Date.UTC(2005, 4, 5, 17, 55, 55));

//本地时间 2000 年 1 月 1 日午夜 0 时
var someDate4 = new Date(2000, 0);  //一样可直接传入参数，但是与调用 UTC 不同的一点是，是基于本地时区，而不是GMT来创建。

//取得调用该方法的时间
var start = Date.now();

doSomething();

var stop = Date.now();

//根据二者的时间差，可计算函数调用时间
var result = stop - start;

```

#### 5.3.1 继承的方法

重写了如下方法：

1. toLocaleString()
    - 按照 浏览器设置的递取相适应的格式 返回日期和时间。
2. toString()
    - 通常返回带有时区信息的日期和时间
3. valueOf()
    - 根本不返回字符串，返回日期的毫秒。因此可以方便地比较日期
    ```js
    var date1 = new Date(2019, 0, 1);
    var date2 = new Date(2019, 1, 1);
    
    alert(date1 < date2);   //true ，2019 年 1 月 1 日毫秒值小于 2019 年 2 月 1 日毫秒值
    ```
还介绍了三个日期格式化方法和一堆取部分时间的方法（比如只返回天数、只返回年数、返回毫秒值、设置日期的月份）

### 5.4 RegExp 类型

```js
var expression = /pattern/flags;
```
1. 模式（ pattern ）：字符类、限定符、分组、向前查找、反向引用
    - 元字符：（ ( [ { \ ^ $ | ) ? * + . ] } ）
2. 标志（ flags ）： g （全局（ global ）模式）、 i （不区分大小写（ case-insensitive ）模式）、 m （多行（ multiline ）模式）

```js
//匹配第一个 "bat" 或 "cat“ ，不区分大小写
var pattern1 = /[bc]at/i;                //字面量形式定义
var pattern2 = new RegExp("[bc]at", "i");   //使用 RegExp 构造函数，完全等价（有些情况需要进行双重转义）
```

#### 5.4.1 RegExp 实例属性

global 、 ignoreCase 、 lastIndex 、 multiline 、 source

用处不大，这些信息都包含在模式声明中。

```js
var pattern1 = /\[bc\]at/i;

alert(pattern1.global); //false
alert(pattern1.ignoreCase); //true
alert(pattern1.multiline);  //false
alert(pattern1.lastIndex);  //0 ，表示开始搜索 下一个匹配项 的字符位置
alert(pattern1.source);     //"\[bc\]at"
```

#### 5.4.2 RegExp 实例方法

1. exec()
- 只接收一个参数---应用模式的字符串
- 返回包含第一个匹配项信息的数组（没有匹配项则返回null）
    - 虽然是 Array 实例，包含两个额外的属性： index （匹配项在字符串中的位置）和 input （应用模式的字符串）。
```js
var text = "mom and dad and baby";
var pattern = /mom( and dad( and baby)?)?/gi;     //包含两个捕获组，即使设置为g，每次只会返回一个匹配项（捕获组是对每个匹配项再次进行细分匹配）（g情况，多次执行才会继续查找新匹配项）

var matches = pattern.exec(text);
alert(matches.index);       //0 ，因为整个字符串本身与模式匹配
alert(matches.input);       //"mom and dad and baby"
alert(matches[0]);          //"mom and dad and baby" ，是匹配的字符串
alert(matches[1]);          //" and dad and baby" ，第一个捕获组的内容
alert(matches[2]);          //" and baby" ，第二个捕获组的内容

alert(RegExp.$2);          //RegExp.$1 、 RegExp.$2 、 RegExp.$3 、 RegExp.$4 分别存储第一、第二个...第四个匹配的捕获组
```
2. test()
- 只接受一个字符串参数
- 匹配情况返回 true ；否则返回 false 。在只是想知道目标字符串是否与某个模式匹配，非常方便。

#### 5.4.3 RegExp 构造函数属性

这些属性适用于*作用域中*的所有正则表达式，基于所执行的*最近一次*正则表达式操作而变化。

```js
var text = "this has been a short summer";
var pattern = /(.)hort/g;

if(pattern.test(text)) {
    alert(RegExp.input);        //this has been a short summer
    alert(RegExp.leftContext);  //this has been a 
    alert(RegExp.rightContext); //summer
    alert(RegExp.lastMatch);    //short
    alert(RegExp.lastParen);    //s ，返回最近一次匹配的捕获组
    alert(RegExp.multiline);    //false

    alert(RegExp.$_);           //以上还有短属性名，比较恶心，举例 RegExp.input
    alert(RegExp["$`"]);        // RegExp.leftContext
}
```

### 5.5 Function 类型

每个函数都是 Function 类型的实例，而且*具有属性和方法*（和其他引用类型一样）。
```js
function sum(num1, num2) {
    return num1 + num2;
}

var sum1 = function(num1, num2) {
    return num1 + num2;
}

//极不推荐，二次解析代码
//只是可以更直观理解”函数是对象，函数名是指针“的概念。
var sum2 = new Function("num1", "num2", "return num1 + num2");
```

#### 5.5.1 没有重载

```js
function addSomeNumber(num) {
    return num + 100;
}

function addSomeNumber(num) {
    return num + 200;
}
```
等价如下：
```js
var addSomeNumber = function(num) {
    return num + 100;
}

//在创建第二个函数时，实际上覆盖了引用 第一个函数的变量 addSomeNumber 。
addSomeNumber = function(num) {
    return num + 200;
}
```

#### 5.5.2 函数声明与函数表达式

*函数声明*和*函数表达式*的区别：**解析器会率先读取函数声明，并使其在执行任何代码之前可用**（函数声明提升）；而函数表达式必须等到执行到该行。
```js
//函数声明会被提前，正常运行
alert(sum(10, 10));
function sum(num1, num2) {
    return num1 + num2;
}
```

#### 5.5.3 作为值的函数

1. 可以作为参数传递给另个函数
```js
function callSomeFunction(someFunction, someArgument) {
    return someFunction(someArgument);
}

function add10(num) {
    return num + 10;
}
var result1 = callSomeFunction(add10, 10);  //20

function getGreeting(name) {
    return "Hello, " + name;
}
var result2 = callSomeFunction(getGreeting, "Kiyonami");    //"Hello, Kiyonami"
```

2. 可以从一个函数中返回另一个函数
```js
function createComparisonFunction(propertyName) {   //接收一个字符串，表示属性

    return function(object1, object2) {
        var value1 = object1[propertyName];
        var value2 = object2[property];

        return value1 - value2;
    }
}

var data = [{name: "Zachary", age: 28}, {name: "Kiyonami", age: 29}];

data.sort(createComparisonFunction("name"));
alert(data[0].name);    //Kiyonami

data.sort(createComparisonFunction("age"));
alert(data[0].name);    //Zachary
```

#### 5.5.4 函数内部属性

在函数内部，有两个特殊的对象： arguments 和 this 。

1. arguments

arguments 中还有一个名叫 callee 的属性，是一个指向拥有这个 arguments 对象的*函数*的指针。

例如

```js
//阶乘函数递归（以前写Java差不多都是如下，没有任何问题
function factorial(num) {
    if(num <= 1) {
        return 1;
    } else {
        return num * factorial(num - 1);    //与函数名 factorial 紧紧耦合在了一起（可以使用 arguments.callee 消除这一现象
    }
}
//函数名不变化，没有问题；但是变化呢？
```

使用 arguments.callee 
```js
function factorial(num) {
    if(num <= 1) {
        return 1;
    } else {
        return num * arguments.callee(num - 1); 
    }
}

var trueFactorial = factorial;

factorial = function() {
    return 0;
}

alert(trueFactorial(5));    //5*4*3*2*1=120 ，若是没有使用 callee 则内部会调用 factorial ，返回 0
alert(factorial(5));    //0
```

2. this

this 引用的是函数以执行的环境对象。（在全局时引用的是 window

**只有在调用的时候， this 的值才被确定！（定义时并不确定**

```js
window.color = "red";
var o = { color : "blue" };

function sayColor() {
    alert(this.color);  //调用前， this 的值并不确定
}

sayColor();     //"red"

//o.sayColor = function() {
//    alert(this.color);
//}
o.sayColor = sayColor;      //指向的仍是同一个函数！但是环境变了！
o.sayColor();   //"blue"
```

3. caller

这个属性保存着调用当前函数的*函数*的引用。

```js
function outer() {
    inner();
}

function inner() {
    alert(inner.caller);    //更加解耦 -> alert(arguments.callee.caller);
}

outer();    //会导致警告框中显示 outer() 函数的源代码

inner();    //在全局中调用当前函数，它的值为 null
```

#### 5.5.5 函数属性和方法

每个函数都包含两个属性：length 和 prototype 。

1. length

    表示希望接收的命名参数的个数

2. prototype

    对于引用类型而言， prototype 是保存它们所有实例方法的真正所在。

每个函数都包含两个两个： apply() 和 call() 。

1. apply()

    接收两个参数：在其中运行函数的作用域，一个是参数数组（可以是 Array 的实例，也可以是 arguments 对象）。

    ```js
    function sum(num1, num2) {
        return num1 + num2;
    }

    function callSum1(num1, num2) {
        return sum.apply(this, arguments);
    }

    function callSum2(num1, num2) {
        return sum.apply(this, [num1,  num2]);
    }

    alert(callSum1(10, 10));    //20    ////因为在全局调用，所以传递的是 window 对象
    alert(callSum2(10, 10));    //20
    ```

2. call()

    作用与 apply() 相同，就是参数不同。

    第一个参数依旧是 this ，其余参数都要列举出来传递。

    ```js
    function callSum3(num1, num2) {
        return sum.call(this, num1, num2);
    }

    alert(callSum(10, 10)); //20
    ```

二者选择，取决于哪个函数传递参数方便。

**真正强大的地方**是能够扩充函数赖以运行的作用域。扩充的最大好处，就是对象不需要与方法有任何耦合关系。

```js
window.color = "red";
var o = { color : "blue"};

function sayColor() {
    alert(this.color);
}

sayColor();             //red
sayColor.call(this);    //red
sayColor.call(window);  //red
sayColor.call(o);       //blue      

//之前的写法： 
//o.sayColor() = sayColor;   先将 sayColor() 放到了对象 o 中，将其二者耦合
//o.sayColor();              然后再通过 o 来调用它（永久绑定

//现在就不需要这些多余的步骤了（但是上面的步骤都是一次性的，所以每次调用都要重新 call
```

3. bind()

    这个方法会**创建一个函数**的实例，其 this 值会被绑定。（定义时永久绑定
    
    （前两个函数是在函数调用的时候才调用！临时绑上

    ```js
    window.color = "red";
    var o = { color: "blue" };

    function sayColor() {
        alert(this.color);
    }

    var objectSayColor = sayColor.bind(o);  //this 值定于 o
    objectSayColor();   //blue
    ```

### 5.6 基本包装类

三个特殊的引用类型： Boolean 、 Number 和 String 。

实际上，后台会自动在读取基本类型时，创建对应的包装类对象。（读取完毕后，立即销毁。如果直接new，在离开作用域之前都一直保存在内存中（区别）。

#### 5.6.1 Boolean 类型

Boolean ：建议永远不要使用。
```js
var falseObject = new Boolean(false);
alert(falseObject && true);             //true
alert(typeof falseObject);              //object
alert(falseObject instanceof Boolean);  //true

var falseValue = false;
alert(falseValue && true);              //true
alert(typeof falseValue);               //boolean
alert(falseValue instanceof Boolean);   //false
```

#### 5.6.2 Number 类型

Number ：不建议直接实例化。原因和 Boolean 一样。
- toString() 方法可传递一个参数表示基数，返回几进制表示的字符串
- toFixed() 保留几位小数（四舍五入
```js
var num = 10.005;
alert(num.toFixed(2));      //10.01
```
- toExponential() 返回以指数表示法表示的字符串。
- toPrecision() 会选取合适的格式返回。接收一个参数，即表示所有数字的位数（不包括指数）。
```js
var num = 99;
alert(num.toPrecision(1));      //1e+2
alert(num.toPrecision(2));      //99
alert(num.toPrecision(3));      //99.0
```

#### 5.6.3 String 类型

都有一个属性，length。

1. 字符方法
- charAt() 返回给定位置的单字符串，也可以stringValue[1]
- charCodeAt()  输出的是字符编码

2. 字符串操作方法
- concat() 字符串拼接，但是更多还是用加号（ + ）
- slice() 、 substr() 、 substring() 返回子串

3. 字符串位置方法
- indexOf()
- lastIndexOf()

4. trim() 删除前置以及后缀的所有空格，返回结果

5. 字符串大小写转换方法
- toLowerCase()
- toLocaleLowerCase()
- toUpperCase()
- toLocaleUpperCase()

6. 字符串的模式匹配方法
- match() 只接受一个参数，正则表达式（ RegExp 对象），返回和调用 RegExp 的 exec()方法相同的结果
- search() 参数与 match() 相同，返回第一个匹配项的索引
- replace() 接收两个参数：正则或者字符串，字符串或者函数。
- split() 基于指定的分隔符分割字符串，子串结果放在一个数组中。

7. localeCompare() 比较两个字符串，返回 -1 ， 0 ， 1 （大多数情况）

8. fromCharCode() 静态方法，接收一或多个字符编码，转化一个字符串。

9. HTML 方法

动态格式化 HTML 的需求。

不推荐使用，因为它们创建的标记通常无法表达语义。

anchor(name), big(), bold(), fixed(), fontcolor(color), fontsize(size), italics(), link(url), small(), strike(), sub(), sup()

例如`string.link(url)`结果为 `<a href = "url">string</a>`

### 5.7 单体内置对象

ECMAScript实现提供地、不依赖于宿主环境地对象，在程序执行前就存在了。（不需要显示实例化

例如 Object 、 Array 和 String 。

还定义了两个单体内置对象： Global 和 Math 。

#### 5.7.1 Global 对象

最特别的对象。

不属于任何其他对象的属性和方法， 最终都是它的属性和方法。

事实上，没有全局变量或全局函数；所有在全局作用域中定义的属性和函数，都是 Global 对象的属性。

例如`isNaN()`、`parseFloat()`都是 Global 对象的方法。

1. URI 编码方法

两个 URI 方法都对 URI 进行编码，用特殊的 UTF-8 编码*替换*所有无效的字符（例如空格），从而让浏览器能够接收和理解。

- encodeURI() 主要用于整个 URI （例如， http://wwww.wrox.com/illegal value.htm ）的编码。（只有空格被替换）
- encodeURIComponent() 主要用于对 URI 中的某一段（例如 illegal value.htm ）进行编码。（会使用对应的编码替换所有非字母数字字符）
- decodeURI()
- decodeURIComponent()

2. eval() 方法

大概是最强大的方法。

就像是一个完整的 ECMAScript 解析器，只接受一个参数，即要执行的 ECMAScript 字符串。

（我觉得这方法并没有什么意义，只是将代码写到了字符串里，再通过这个函数执行。

使用必须极为谨慎，防止恶意用户**代码注入**。

```js
eval("function sayHi() { alert('hi'); }");
sayHi();
```

3. Global 对象的属性

undefined 、 NaN 以及 Infinity 都是 Global 对象的属性。

所有原生引用类型的构造函数，例 Object 和 Function ，也都是 Global 对象的属性。

所有： undefined 、 NaN 、 Infinity 、 Object 、 Array 、 Function 、  Boolean 、 String 、 Number 、 Date 、 RegExp 、 Error 、 EvalError 、 RangeError 、 ReferenceError 、 SyntaxError 、 TypeError 、 URIError

ECMAScript5 禁止给 undefined 、 NaN 和 Infinity 赋值。（所以都是对象，只是禁止赋值了

4. window 对象

通过访问 window 对象来访问 Global 对象（作为 window 对象的一部分）。

因此，在全局作用域声明的所有变量和函数，就都成为了 window 对象的属性。

#### 5.7.2 Math 对象

为保存数学公式和信息提供了一共公共位置。

Math 对象提供的计算功能快很多（和我们自己编写相比

1. Math 对象的属性

|属性|说明|
| -- | -- |
|Math.E|自然对数的底数，即常量 e 的值|
|Math.LN10| 10 的自然对数|
|Math.LN2| 2 的自然对数|
|Math.LOG2E| 以 2 为底 e 的对数|
|Math.LOG10E| 以 10 为底 e 的对数|
|Math.PI| pi 的值|
|Math.SQRT1_2| 1/2 的平方根（即 2 平方根的倒数）|
|Math.SQRT2| 2 的平方根|

2. min() 和 max() 方法

```js
//接收任意多个数值参数
var max = Math.max(3, 54, 32, 16);  //54

//运用 apply 传递数组
var min = Math.min.apply(Math, [3, 54, 32, 16]);    //3
```
3. 舍入方法

|方法|说明|
|--|--|
|Math.ceil()|执行向上舍入|
|Math.floor()|执行向下舍入|
|Math.round()|四舍五入|

4. random() 方法

返回介于 0 和 1之间一个随机数，不包括 0 和 1 。

从整数范围内随机取值的标准公式**值 = Math.floor(Math.random() * 可能值的总数 + 第一个可能的值)**  
```js
var num = Math.floor(Math.random() * 9 + 2);   //选一个 2-10 的数值
```

5. 其他方法

- Math.abs(num)
- Math.exp(num)     返回 Math.E 的 num 次幂
- Math.log(num)     返回 num 的自然对数
- Math.pow(num, power)     返回 num 的 power 次幂
- Math.sqrt(num)
- Math.acos(num)
- Math.asin(num)
- Math.atan(num)
- Math.atan2(y, x)
- Math.cos(num)
- Math.sin(num)
- Math.tan(num)

## 第六章 面向对象的程序设计

可以把 ECMAScript 的对象（引用类型）想象成散列表：无非就是一组名值对，其中值可以是数据或函数。

### 6.1 理解对象

```js
var person = {
    name: "Kiyonami", 
    age: 23,
    job: "Software Engineer",

    sayName: function() {
        alert(this.name);
    }
}
```

#### 6.1.1 属性类型

特性（ attribute ）：描述属性（ property ）的各种特征。（只有内部才用）（不能直接访问）

为了表示特性是内部值，该规范把它们放在了两对方括号中，例如`[[Enumerable]]`。

ECMAScript 中有两种属性：数据属性，访问器属性。

1. 数据属性

|特性|说明|
|---|---|
|[[Configurable]]|表示能否通过 delete 删除属性，能否修改属性的特性，或者能否把属性修改为访问器属性。（默认为 true ）|
|[[Enumerable]]|表示能否通过 for-in 循环返回属性。（默认为 true ）|
|[[Writable]]|表示能否修改属性的值。（默认为 true ）|
|[[Value]]|属性的数据值。读写都在这个位置。（默认为 undefined ）|

要修改属性默认的特性，必须使用`Object.defineProperty()`方法。（不常用，助于理解对象）

```js
var person = {};
//接收三个参数：属性所在的对象、属性的名字和一个描述符对象。
//描述符对象的属性必须是四者内。
Object.defineProperty(person, "name", {
    configurable: false,    //不能从对象中删除。此时，再调用 Object.defineProperty() 只能修改 writable ，其他不能修改会导致错误
    writable: false,        //不可修改值
    value: "Kiyoanmi"       //设置值
});

alert(person.name);     //Kiyonami

delete person.name;     //严格模式下报错
alert(person.name);     //Kiyonami

person.name = "Greg";   //严格模式下报错
alert(person.name);     //Kiyonami

```

2. 访问器属性

不包含数据值；包含一对 getter 和 setter 函数（不是必须）。

若只指定 getter 意味着属性是不能写。

|特性|说明|
|---|---|
|[[Configurable]]|表示能否通过 delete 删除属性，能否修改属性的特性，或者能否把属性修改为数据属性。（默认为 true ）|
|[[Enumerable]]|表示能否通过 for-in 循环返回属性。（默认为 true ）|
|[[Get]]|在读取属性时调用的函数。（默认为 undefined ）|
|[[Set]]|在写入属性时调用的函数。（默认为 undefined ）|

```js
var book = {
    _year : 2000,
    edition: 1
}

//新定义一个访问器属性： year
Object.defineProperty(book, "year", {
    get: function() {
        return this._year;
    },
    set: function(newValue) {
        this._year = newValue;
        if(newValue > 2000) {
            this.edition += newValue - 2000;
        }
    }
});

book.year = 2005;
alert(book.year);   //1 + (2005 - 2000) = 6
```

#### 6.1.2 定义多个属性

上面的`Object.defineProperty(object, attribute, {property})`，一次只能定义一个属性的多个特性。

要同时定义多个属性，选择`Object.defineProperties(object, {attribute: {property}})`，一次定义多个属性。（只有参数和同时的区别）

```js
var book = {};

Object.defineProperty(book, {
    //数值属性
    _year: {
        value: 2000
    },
    edition: {
        value: 1
    },
    //访问器属性
    year: {
        get: function() {
            return this._year;
        },
        set: function(newValue) {
            this._year = newValue;
            if(newValue > 2000) {
                edition  += newValue - 2000;
            }
        } 
    }
});
```

#### 6.1.3 读取属性的特性

`Object.getOwnPropertyDescriptor()`方法，可以取得给定属性的描述符。

根据是访问器属性还是数值属性，返回对应的描述符对象。

```js
var descriptor = Object.getOwnPropertyDescriptor(book, "_year");
alert(descriptor.value);    //2000
alert(descriptor.configurable); //false，调用 defineProperty 后，未重新定义 configurable ，则默认为 false
alert(typeof descriptor.get);   //undefined, 因为是数值属性 
```

### 6.2 创建对象

#### 6.2.1 工厂模式

```js
function createPerson(name, age, job) {
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function() {
        alert(this.name);
    };
    return o;
}

var person1 = createPerson("Kiyonami", 23, "Software Engineer");
var person2 = createPerson("Greg", 27, "Doctor");
```

**问题**：工厂模式虽然解决了创建多个相似对象的问题，但却没有解决对象识别的问题（即怎么知道一个对象的类型）。

#### 6.2.2 构造函数模式

```js
function Person(name, age, job) {
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function() {
        alert(this.name);
    };
}

var person1 = new Person("Kiyonami", 23, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");
```

在这个例子中，注意`Person()`函数和`createPerson()`函数的**区别**：
- 没有显式地创建对象（ createPerson -> var o = new Object(); ）
- 直接将属性和方法赋给了 this 对象（ createPerson -> o.name = name; ）
- 没有 return 语句（ createPerson -> return o; ）

要创建 Person 新实例，必须使用 new 操作符。

以这种构造函数地方式会经历以下 4 个步骤：
- 创建一个新对象（ new ）
- 将构造函数地作用域赋值给新对象（因此 this 就指向了这个新对象）
- 执行构造函数中的代码（ 为这个新对象添加属性 ）
- 返回新对象

person1 和 person2 都有一个 constructor 属性，该属性指向 Person 。（原型模式中的 constructor）
```js
alert(person1.constructor == Person);   //true
alert(person2.constructor == Person);   //true
```

对象的 constructor 属性最初是用来标识对象类型的（所以不常使用），但还是使用 instanceof 操作符更靠谱一些。例子中创建的对象既是 Object 实例，也是 Person 实例。
```js
alert(person1 instanceof Person);   //true
alert(person1 instanceof Object);   //true
```

**相较于工厂模式的优点**：可以将它的实例标识为一种特定的类型。

1. 将构造函数当作函数

构造函数与其他函数的**唯一区别**，就在于调用它们的方式不同。（不存在特殊语法）

任何函数，只要通过 new 操作符来调用，那它就可以作为构造函数。
```js
//当作构造函数使用
var person = new Person("Kiyonami", 23, "Software Engineer");
person.sayName();   //Kiyonami

//作为普通函数
Person("Bob", 20, "Doctor");    //直接全局执行该函数，作用域是 Global ，函数中的 this 对应于 Global ，所以是为 Global 绑定了属性和方法
window.sayName();  //Bob    

//在另一个对象的作用域中调用
var o = new Object();
Person.call(o, "Marvel", 22, "Nurse");  //通过 call() 在 o 对象的作用域中调用，和全局一样的过程（觉得通过 new 操作符，与这种函数调用，内部过程极为相似）
o.sayName();    //Marvel
```

2. 构造函数的问题

```js
//不同实例上的同名函数是不相等的（都是另外新建的）
alert(person1.sayName == person2.sayName);  //false
```

问题1：创建两个完成同样任务的 Function 实例没有必要。

使用如下方式解决：把 sayName() 函数的定义转移到构造函数外部。这样一来，由于 sayName 包含的是一个指向函数的指针，因此 对象共享了全局作用域中顶底的同一个 sayName() 函数。
```js
function Person(name, age, job) {
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = sayName;
}

function sayName() {
    alert(this.name);
}

var person1 = new Person("Kiyonami", 23, "Software Engineer");
```

**新问题**：在全局作用域中定义的函数实际上只能被某个对象调用（名不副实），并且如果对象需要定义很多方法，这样就要定义很多全局函数，丝毫没有封装性可言！

#### 6.2.3 原型模式

创建的每个函数都有一个 prototype 属性，这个属性是一个指针，指向一个对象（共享属性和方法）。

prototype 就是通过*调用构造函数而创建的那个对象实例*的原型对象。

使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法。

```js
function Person() {
}

Person.prototype.name = "Kiyonami";
Person.prototype.age = 23;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function() {
    alert(this.name);
}

var person1 = new Person();
person1.sayName();      //Kiyonami

var person2 = new Person();
person2.sayName();      //Kiyonami

alert(person1.sayName == person2.sayName);  //true
```

与构造函数模式不同的是，新对象的这些属性和方法是由所有实例共享的。

1. 理解原型对象

创建一个新函数，自动为其创建一个 prototype 属性，这个属性指向函数的原型对象。

这个原型对象（ prototype ）会自动获得一个 constructor 属性，这个属性包含一个指向 prototype 属性所在函数的指针（就比如， Person.prototype.constructor 指向 Person ）。

当调用构造函数创建一个新实例后，该实例的内部将包含一个指针 [[Prototype]] （内部属性, 不可见），指向构造函数的原型对象（和构造函数的 prototype 指向一样）。

![Prototype](https://raw.githubusercontent.com/514723273/.md-Pictures/master/Prototype.png)

- Person.prototype 指向原型对象
- 实例对象 person1 、 person2 的内部属性 [[Prototype]] 也只指向原型对象（这里发现，它们与构造函数没有直接关系）
- Person.prototype.constructor 指回 Person

虽然所有实现无法访问到 [[Prototype]] ，但可以通过 isPrototypeOf() 和 Object.getPrototypeOf() （新增）方法来确定对象之间是否存在这种关系。
```js
alert(Person.prototype.isPrototypeOf(person1));     //true ，因为内部都有一个指向 Person.prototype 的指针

alert(Object.getPrototypeOf(person1) == Person.prototype);     //true
```

每当代码读取某个对象的某个属性时，都回执行一次搜索，目标是具有给定名字的属性。

如前例，我们调用 person1.sayName() 的时候，会先后执行两次搜索。（实例 person1 、 person1 的原型）

可以通过对象实例访问原型中的值，但不能通过对象实例重写。添加一个与原型对象中重名的属性，则屏蔽原型中的属性（只会阻止我们访问原型中的那个属性，但不会修改）。可以使用 delete 操作符删除实例属性，从而重新访问原型中的属性。
```js
var person1 = new Person();
var person2 = new Person();

person1.name = "Greg";
alert(person1.name);    //"Greg" --- 来自实例
alert(person2.name);    //"Kiyonami" --- 来自原型

delete person1.name;
alert(person1.name);    //"Kiyonami" --- 来自原型
```

hasOwnProperty() 方法（ Object 继承来的）可以检测一个属性是否存在于实例中，存在则返回true。
```js
var person1 = new Person();

alert(person1.hasOwnProperty("name"));      //false     实例没有，原型有

person1.name = "Greg";  
alert(person1.hasOwnProperty("name"));      //true

delete person1.name;
alert(person1.hasOwnProperty("name"));      //false
```

2. 原型与 in 操作符

两种方式使用 in 操作符：单独使用和在 for-in 循环中使用。

in 操作符会在通过对象能够访问给定属性时返回 true （无论实例或原型）。
```js
var person1 = new Person();
alert("name" in person1);   //true
```

所以可以定义一个函数确定属性是否存在于原型（之前一个函数时判断属性是否存在于实例）：
```js
function hasPrototypeProperty(object, attribute) {
    return (attribute in object) && !object.hasOwnProperty(attribute);    //存在该属性，但不存在实例上 => 存在原型上
}
```

for-in 循环时，返回实例属性和原型属性。（能够通过对象访问的、可枚举的属性）

要取得对象上所有可枚举的*实例*属性，可以使用 Object.keys() 方法。
```js
var keys = Object.keys(Person.prototype);
alert(keys);        //name, age, job, sayName

var p1 = new Person();
p1.name = "Bob";
p1.age = 12;
var p1keys = Object.keys(p1);
alert(p1keys);      //name, age
```

要取得所有实例属性（无论是否可枚举），可以使用 Object.getOwnPropertyNames() 方法。
```js
var keys = Object.getOwnPropertyName(Person.prototype);
alert(keys);        //constructor, name, age, job, sayName
```

3. 更简单的原型语法

```js
//不需要每行敲一遍 Person.prototype
function Person() {
}
Person.prototype = {
    //constructor: Person,      可以强行设置回来（但此时 [[Enumerable]] 特性被设置为 true ）（原生 constructor 属性不可枚举）
    name: "Kiyonami",
    age: 23,
    job: "Software Engineer",
    sayName: function() {
        alert(this.name);
    }
}
//将 Person.prototype 设置为一个新对象，constructor 属性不再指向 Person 了（之前的操作都是添加新属性，而这里是重新创建，新对象中没有包含 constructor 属性）。（instanceof 操作符还能返回正常的结果，但通过constructor 已经无法确定对象的类型了）

var person3 = new Person();

alert(person3 instanceof Object);       //true
alert(person3 instanceof Person);       //true
alert(person.constructor == Person);    //false
alert(person.constructor == Object);    //true

Object.defineProperty(Peson.prototype, "constructor", {
    enumerable: false,
    value: Person
})
```

4. 原型的动态性

我们对原型对象所作的任何修改都能立即从实例上反映出来---即使先创建实例后修改原型。
```js
var person4 = new Person();
Person.prototype.sayHi = function() {
    alert("Hi");
}
person4.sayHi();    //"Hi"
```

但是重写整个原型对象，那情况完全不一样了。我们知道，调用构造函数时会为实例添加一个指向最初原型的 [[Prototype]]指针，而把原型修改为另外一个对象就等于*切断*了构造函数和最初原型之间的联系。
**请记住：实例中的指针仅指向原型，而不是构造函数。**
```js
function Person() {
}
var person5 = new Person();

Person.prototype = {
    constructor: Person,
    name: "KK",
    age: 1,
    job: "nurse",
    function sayName() {
        alert(this.name);
    }
}

person5.sayName();      //error
```

![Prototype2](https://raw.githubusercontent.com/514723273/.md-Pictures/master/Prototype2.png)

5. 原生对象的原型

所有原生的引用类型，都是采用这种模式创建的。

例如，在 Array.prototype 中可以找到 sort() 方法，而在 String.prototype 中可以找到 substring() 方法。

不推荐修改原生对象的原型。（比如添加功能函数）（可能导致命名冲突）

6. 原型对象的问题

省略了为构造函数传递初始化参数这一环节，结果所有实例在默认情况下都取得相同的属性值。

**最大问题**是由其共享的本性导致的（对于引用类型值的属性）。
```js
function Person() {
}
Person.prototype = {
    constructor: Person,
    name: "Kiyonami",
    age: 23,
    friends: ["Count", "Bob"],      //不同点，增加引用类型的属性
    sayHi: function() {
        alert(this.name);
    }
}

var person1 = new Person();
var person2 = new Person();

person1.friends.push("Van");
alert(person1.friends);     //Count, Bob, Van
alert(person2.friends);     //Count, Bob, Van   两个人的朋友应该是相互独立的！各管各
alert(person1.friends == person2.friends);  //true
```

#### 6.2.4 组合使用构造函数模式和原型模式

创建自定义类型的**最常见**的方式。

构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性。

优点：最大限度地节省内存；支持向构造函数传递参数。（每个实例都有一根实例属性地副本，同时又共享着对方法地引用）

```js
//（私有的）每个实例都有一份自己地副本（放这）
function Person(name, age, job) {
    this.name = name;
    this.age = age;
    this.job = job;
    this.friends = ["Count", "Bob"];
}

//（共享的）所有方法和共享属性（放这）
Person.prototype = {
    constructor: Person,
    sayName: function() {
        alert(this.name);
    }
}

var person1 = new Person("Kiyonami", 23, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");

person1.friends.push("Van");
alert(person1.friends);     //"Count", "Bob", "Van"
alert(person2.friends);     //"Count", "Bob"
alert(person1.friends == person2.friends);      //false
alert(person1.sayName ==  person2.sayName);     //true
```

#### 6.2.5 动态原型模式

解决一问题：其他 OO 语言经验的开发人员在看到*独立的构造函数和原型*时感到困惑。

```js
function Person(name, age, job) {
    this.name = name;
    this.age = age;
    this.job = job;
    //当第一次调用构造函时，this.sayName 未定义，typeof 为 undefined ，所以会执行 if 语句内的添加原型函数和属性的方法（只要 if 语句判断其中任意一个即可，不需要一大堆 if 语句，都会在一个 if 内添加完所有方法）
    if(typeof this.sayName != "function") {
        Person.prototype.sayName = function() {
            alert(this.name);
        };
        Person.prototype.sayHi = function() {
            alert("Hi");
        }
    }
}
```

使用动态原型模式时，不能重写原型对象，就相当于断开了连接（前一小节解释）。

#### 6.2.6 寄生构造函数模式

通常，前面五种模式不适用的情况下使用。

```js
function Person(name, age, job) {
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function() {
        alert(this.name);
    };
    return o;
}

var person = new Person("Kiyonami", 23, "Software Engineer");
person.sayName();
```

与工厂模式的差别只有：使用了 new 操作符，把函数称之为构造函数。（二者其实一模一样）

（ new 操作符）构造函数在不返回值的情况下，*默认会返回新对象实例*。而通过在构造函数的末尾添加一个 return 语句，可以重写调用构造函数时返回的值。（返回的对象与构造函数或者构造函数的原型没有任何关系）

这个模式只用在特殊情况，其他情况不推荐（理由和工厂模式一致）。
```js
//假设我们想创建一个具有额外方法的特殊数组。

function SpecialArray() {
    //创建数组
    var values = new Array();

    //数组内添加初始项（应该是 push() 不能直接添加一整个数组）
    values.push.apply(values, arguments);

    //添加方法
    values.toPipedString = function() {
        return values.join("|");
    }

    //返回数组
    return values;
}

var colors = new SpecialArray("red", "blue", "green");
alert(colors.toPipedString());      //red|blue|green
```

#### 6.2.7 稳妥构造函数模式

所谓稳妥对象，值得是没有公共属性，而且其方法也不引用 this 的对象。

（所以不常用）稳妥对象最适合在一些安全的环境中（这些环境会禁止使用 this 和 new ），或者在防止数据被其他应用程序改动时使用。

与寄生构造函数类似，但有两点不同：不引用 this ；不使用 new 操作符。

```js
function Person(name, age, job) {
    var o = new Object();

    //可以在这里定义私有变量和函数

    o.sayName = function() {
        alert(name);        //与传入的参数完成绑定
    };

    return o;
}

var person = Person("Kiyonami", 23, "Software Engineer");
person.sayName();
//除了调用 sayName() ， 没有别的方式可以访问其数据成员（通过构造函数传入的参数）。
//创建的对象与构造函数之间也没有什么关系，因此 instanceof 没意义。
```

### 6.3 继承

OO语言都支持两种继承方式：接口继承（只继承方法签名）和实现继承（继承实际的方法）。

ECMAScript 只支持实现继承，而且其实现继承主要是依靠*原型链*来实现的。

#### 6.3.1 原型链

```js
function SuperType() {
    this.property = true;
}
SuperType.prototype.getSuperValue = function() {
    return this.property;
}

function SubType() {
    this.subproperty = false;
}
SubType.prototype = new SuperType();        //相当于重写原型对象
SubType.prototype.getSubValue = function() {    //在重写后的原型对象添加新方法
    return this.subproperty;
}
var instance = new SubType();
alert(instance.getSuperValue());    //true
```

![](https://raw.githubusercontent.com/514723273/.md-Pictures/master/%E5%8E%9F%E5%9E%8B%E9%93%BE.png)

1. 别忘记默认的原型

![](https://raw.githubusercontent.com/514723273/.md-Pictures/master/%E9%BB%98%E8%AE%A4%E7%9A%84%E5%8E%9F%E5%9E%8B%E9%93%BE.png)

2. 确定原型和实例的关系

```js
alert(instance instanceof Object);      //true
alert(instance instanceof SuperType);   //true
alert(instance instanceof SubType);     //true

alert(Object.prototype.isPrototypeOf(instance));
alert(SuperType.prototype.isPrototypeOf(instance));
alert(SubType.prototype.isPrototypeOf(instance));
```

3. 谨慎定义方法

重写超类的某个方法或添加新方法，都一定要放在替换原型的语句之后。（重写原型对象之后）

4. 原型链的问题

最主要的问题来自*引用类型值*的原型。

因为重写子类的原型对象，是通过`new SuperType()`，会把父类的构造函数中的*属性*（本来应该人手一份副本）放入子类原型中，放入原型是一定会被共享的！（就等于往原型里面放了一个前面说的 friends ，子类创建出来的一个实例添加朋友，所有的都会有影响）

#### 6.3.2 借用构造函数

在子类构造函数的内部调用超类构造函数。

```js
function SuperType() {
    this.color = ["red", "blue", "green"];
}

function SubType() {
    //继承了 SuperType
    //如果只是单纯调用 SuperType(); SubType并不会有 color 属性（调用完后，作用域销毁，this.color 销毁，并没有绑定！）。
    //只有通过 call or apply 传入作用域，绑定在 SubType 上
    SuperType.call(this);
}

var instance1 = new SubType();
instance1.color.push("black");
alert(instance1.color);             //red,blue,green,black

var instance2 = new SubType();
alert(instance2.color);             //red,blue,green
```

1. 传递参数

```js
function SuperType(name) {
    this.name = name;
}

//SubType 感受不到
SuperType.prototype.sayName = function() {
    alert(this.name);
}

function SubType() {
    SuperType.call(this, "Kiyonami");

    this.age = 23;      //为了确保 SuperType 构造函数不会重写子类的属性
}

var person = new SubType();
alert(person.age);      //23
//person.sayName();     //error
```

2. 借用构造函数的问题

如果仅仅是借用构造函数，那将无法避免和构造函数模式一样的问题---方法都在构造函数中定义，因此函数复用就无从谈起。

而且定义在超类的原型的方法，对子类是不可见的！


#### 6.3.3 组合继承

也叫伪经典继承。将原型链和借用构造函数的技术组合到一块，发挥二者长处。

```js
function SuperType(name) {
    this.name = name;
    this.friends = ["AA", "BB", "CC"];
}
SuperType.prototype.sayName = function() {
    alert(this.name);
}

function SubType(name, age) {
    SuperType.call(this, name);             //第二次调用 SuperType() 继承所有的属性，这里屏蔽了 new SuperType() 所带来的负影响！（因为创建了同名的属性）
    this.age = age;
}
SubType.prototype = new SuperType();      //第一次调用 SuperType() 继承所有的方法，该子类的原型依旧存在着 name, friends 属性，只不过是被屏蔽了！
//SubType.prototype = new SuperType("YY");      //上一行代码本身也没有传递参数，所以原型对象的 name = undefined 。
SubType.prototype.sayAge = function() {
    alert(this.age);
}

var person = new SubType("Kiyonami", 23);

person.friends.push("DD");
alert(person.friends);  //AA,BB,CC,DD 每人一份副本，互不干扰
person.sayAge();        //23
person.sayName();       //Kiyonami

//这两行测试出，原型依旧存在属性。（原型中存在方法（共享）是应该的，但是存在本应该私有的（会被共享）不好）
delete person.friends;
alert("prototype -> " + person.friends);
```

**不足**：两次调用超类构造函数。

在第一次调用 SuperType 构造函数时， SubType.prototype 会得到两个属性： name 和 colors ；它们都是 SuperType 的实例属性，只不是现在位于 SubType 的**原型**中（原型属性）。

在第二次调用 SuperType 构造函数时，这一次又在新对象上创建了**实例属性** name 和 colors 。这两个属性就屏蔽了原型中的两个同名属性。

已经找到了解决这个问题的方法---寄生组合式继承。（ 6.3.6 ）

#### 6.3.4 原型式继承

借助原型，可以基于已有的对象，创建新对象，同时还不必因此创建自定义类型。

```js
function object(o) {
    function F() {
    }
    F.prototype = o;
    return new F();
}
```

```js
var person = {
    name: "Kiyonami",
    friends: ["AA", "BB", "CC"]
}

var person1 = object(person);
person1.name = "Greg";
person1.friends.push("DD");

var person2 = object(person);
person2.friends.push("EE");

var person3 = Object.create(person, {
    name: {
        value: "Greg"
    }
});

alert(person.friends);      //AA,BB,CC,DD,EE
alert(person1.friends);     //AA,BB,CC,DD,EE ，这样 person.friends 不仅属于 person 所有，而且也会被 person1 和 person2 共享（同时这又是原型链方式的缺点）
```

ECMAScript 新增 Object.create() 方法，规范原型式继承。接收两个参数：对象，额外属性对象（可选）。传入一个参数时，与 object() 方法的行为相同。

在没有必要创建构造函数，只是想让一个对象与另一个对象保持类似的情况下，原型式继承式完全可以胜任的。（但始终会共享）

#### 6.3.5 寄生式继承

寄生式继承与原型式继承紧密相关，思路与寄生构造函数和工厂模式类似。

在主要考虑对象而不是自定义类型和构造函数的情况下，寄生式继承也是一种有用的模式。
```js
function createAnother(original) {
    var clone = object(original);   //不是必须，任何能够返回新对象的函数
    clone.sayHi = function() {      //以某种方式来增强这个对象
        alert("hi");
    }
    return clone;
}
```

#### 6.3.6 寄生组合式继承

普遍认为是最理想的继承范式。

通过借用构造函数来继承属性，通过原型链的混成形式来继承方法。

其背后的基本思路是：不必为了指定子类型的原型而调用超类型的构造函数（第一次调用超类构造函数），我们所需要的只是超类型原型的一个副本。

```js
function inheritPrototype(subType, superType) {
    var prototype = object(superType.prototype);        //创建对象
    prototype.constructor = subType;                    //增强对象
    
    subType.prototype = prototype;                       //指定对象
}
```

```js
function SuperType(name) {
    this.name = name;
    this.friends = ["AA", "BB", "CC"];
}
SuperType.prototype.sayName = function() {
    alert(this.name);
}

function SubType(name, age) {
    Super.call(this, name);                 //这里拿到父类属性
    this.age = age;
}
inheritPrototype(SubType, SuperType);       //这里拿到父类方法，原型里面只存在方法（通常）

SubType.prototype.sayAge = function() {
    alert(this.age);
}

```