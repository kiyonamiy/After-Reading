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

## 第五章 引用类型

### 5.1 Object 类型

创建 Object 实例的方法有两种。

第一种：使用 new 操作符后跟 Object 构造函数。
```
var person = new Object();
person.name = "Kiyonami";
person.age = 20;
```

第二种：使用对象字面量表示法。（更青睐）
```
var person = {
    name : "Kiyonami",
    age : 20,
    5 : true        //测试用，这里的 数值属性名 会自动转换为 字符串
};
```
```
var person = {};    //不会调用 Object 构造函数
person.name = "Kiyonami";       //还可以person["name"]，但通常是点表示法
person.age = 20;
```

### 5.2 Array 类型

可以保存任何类型的数据。可以动态调整大小。

同样，创建 Array 实例的方法有两种。

第一种：使用 Array 构造函数。
```
var colors1 = new Array();

var colors2 = new Array(3);  //创建 length 值为 3 的数组

var colors3 = new Array("Greg");    //创建一个包含 1 项字符串的数组（传递数值和传递其他值的不同）

var color4 = Array(3);  //可以省略 new 操作符。
```

第二种：使用数组字面量表示法。
```
var colors = ["red", "blue", "green"];

var names = [];

var values = [1, 2,,];  //不要这样！这样会创建一个包含 3 或 4 项的数组（浏览器不同）
```

数组的 length 不是只读的！
```
var colors = ["red", "blue", "green"];
colors.length = 2;
alert(colors[2]);   //undefined

colors[colors.length] = "black";    //可以这样，在末尾（位置 2 ）添加新项

colors[99] = "brown";
alert(colors.length);   //100，访问其他为 undefined
```

#### 5.2.1 检测数组

```
if(Array.isArray(value)) {
    //对数组执行某些操作
}
```

#### 5.2.2 转换方法

如前所述，所有对象都具有 toString() （以逗号分隔的字符串）、 valueOf() （返回的还是数组）和toLocaleString() （经常返回与两个方法相同的值）（唯一区别，取得每一项的值，调用的是每一项的 toLocaleString ）方法。

可以使用 join() 方法，使用不同的分隔符来构建返回的字符串。
```
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
```
var values = [0, 1, 5, 10, 15];
values.sort();  //0, 1, 10, 15, 5，按字符串大小排序的
```

因此，`sort()`方法可以接收一个比较函数。
比较函数接收两个参数(a, b)，如果 a 应该位于 b 之*前*（不改变顺序），则返回一个负数。（即升序）
```
function compare(a, b) {
    return a - b;
}

var values = [0, 1, 5, 10, 15];
values.sort(compare);   //接收一个比较函数
```

#### 5.2.6 操作方法

1. `concat()`，创建一个新数组，并将原数组的所有项添加到末尾。

    ```
    var colors = ["red", "green", "blue"];  //原数组一直保持不变
    var colors1 = colors.concat();  //["red", "green", "blue"] ，不传递参数时，作用只是复制当前数组并返回副本
    var colors2 = conlors.concat("yellow", ["black", "brown"]); //["red", "green", "blue"， "yellow", "black", "brown"] ，传递一个或多个数组时，则会将每一项都添加到结果数组
    ```

2. `slice()`，选择原数组中的一段（一个或多个项）（切割），并创建一个新的数组存储。
    ```
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

```
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

```
var values = [1, 2, 3, 4, 5];
var sum = values.reduce(function(prev, cur, index, array) {
    return prev + cur;
}); //15 ，即不断累加的结果
```

### 5.3 Date 类型

```
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
```
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
    ```
    var date1 = new Date(2019, 0, 1);
    var date2 = new Date(2019, 1, 1);
    
    alert(date1 < date2);   //true ，2019 年 1 月 1 日毫秒值小于 2019 年 2 月 1 日毫秒值
    ```
还介绍了三个日期格式化方法和一堆取部分时间的方法（比如只返回天数、只返回年数、返回毫秒值、设置日期的月份）

### 5.4 RegExp 类型

```
var expression = /pattern/flags;
```
1. 模式（ pattern ）：字符类、限定符、分组、向前查找、反向引用
    - 元字符：（ ( [ { \ ^ $ | ) ? * + . ] } ）
2. 标志（ flags ）： g （全局（ global ）模式）、 i （不区分大小写（ case-insensitive ）模式）、 m （多行（ multiline ）模式）

```
//匹配第一个 "bat" 或 "cat“ ，不区分大小写
var pattern1 = /[bc]at/i;                //字面量形式定义
var pattern2 = new RegExp("[bc]at", "i");   //使用 RegExp 构造函数，完全等价（有些情况需要进行双重转义）
```

#### 5.4.1 RegExp 实例属性

global 、 ignoreCase 、 lastIndex 、 multiline 、 source

用处不大，这些信息都包含在模式声明中。

```
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
```
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

```
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
```
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

```
function addSomeNumber(num) {
    return num + 100;
}

function addSomeNumber(num) {
    return num + 200;
}
```
等价如下：
```
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
```
//函数声明会被提前，正常运行
alert(sum(10, 10));
function sum(num1, num2) {
    return num1 + num2;
}
```

#### 5.5.3 作为值的函数

1. 可以作为参数传递给另个函数
```
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
```
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

```
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
```
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

```
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

```
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

    ```
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

    ```
    function callSum3(num1, num2) {
        return sum.call(this, num1, num2);
    }

    alert(callSum(10, 10)); //20
    ```

二者选择，取决于哪个函数传递参数方便。

**真正强大的地方**是能够扩充函数赖以运行的作用域。扩充的最大好处，就是对象不需要与方法有任何耦合关系。

```
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

    ```
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
```
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
```
var num = 10.005;
alert(num.toFixed(2));      //10.01
```
- toExponential() 返回以指数表示法表示的字符串。
- toPrecision() 会选取合适的格式返回。接收一个参数，即表示所有数字的位数（不包括指数）。
```
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

```
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

```
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
```
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

```
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

```
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

```
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

```
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

```
var descriptor = Object.getOwnPropertyDescriptor(book, "_year");
alert(descriptor.value);    //2000
alert(descriptor.configurable); //false，调用 defineProperty 后，未重新定义 configurable ，则默认为 false
alert(typeof descriptor.get);   //undefined, 因为是数值属性 
```