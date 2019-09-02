# JavaScript高级程序设计

## 第 1 章 JavaScript简介

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

## 第 2 章 在 HTML 中使用 JavaScript

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

## 第 3 章 基本概念

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

## 第 4 章 变量、作用域和内存问题

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

## 第 5 章 引用类型

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
3. `splice()`，（剪接、粘接），有很多种用法，主要用途是向数组中的中部插入项。（**改变元数组**，返回被删除的项）
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

## 第 6 章 面向对象的程序设计

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

已经找到了解决这个问题的方法---[寄生组合式继承](https://github.com/514723273/After-Reading/blob/master/JavaScript%E9%AB%98%E7%BA%A7%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/Content.md#636-%E5%AF%84%E7%94%9F%E7%BB%84%E5%90%88%E5%BC%8F%E7%BB%A7%E6%89%BF)。 

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

## 第 7 章 函数表达式

定义函数的方式有两种：函数声明，函数表达式。

1. 函数声明：
```js

functionName();     //函数声明提升，在执行代码之前会先读取函数声明

function functionName(arg0, arg1, arg2) {
    //函数体
}

alert(functionName.name);       //functionName 非标准属性
```

2. 函数表达式（有几种不同的语法形式）最常见：
```js
//匿名函数也叫 拉姆达函数（ Lambda ）
var functionName = function(arg0, arg1, arg2) { //没有函数提升
    //函数体
};
```

### 7.1 递归

与[5.5.4 函数内部属性](https://github.com/514723273/After-Reading/blob/master/JavaScript%E9%AB%98%E7%BA%A7%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/Content.md#554-%E5%87%BD%E6%95%B0%E5%86%85%E9%83%A8%E5%B1%9E%E6%80%A7)内容类似

```js
function factorial(num) {
    if(num <= 1) {
        return 1;
    } else {
        return num * arguments.callee(num - 1);     //是一个指向正在执行的函数的指针
    }
}
```

但在严格模式下，不能通过脚本访问 arguments.callee ，访问这个属性会导致错误。有如下解决办法：
```js
var factorial = (function f(num) {
    if(num <= 1) {
        return 1;
    } else {
        return num * f(num - 1);        //即便把函数复制给了另一个变量，函数的名字 f 仍然有效。
    }
})
```

### 7.2 闭包

过度使用闭包可能会导致内存占用过多，只有在绝对必要时再考虑使用闭包。

分清匿名函数和闭包两个概念。

闭包是指有权访问另一个函数作用域中的变量的*函数*。

创建闭包的常见方式，就是在一个函数内部创建另一个函数。


例如前面提过的 createComparisonFunction() 函数。再看下面的例子：

```js
function compare(value1, value2) {
    return value1 - value2;
}

var result = compare(5, 10);        //在全局作用域中调用了它
```

![作用域链](https://raw.githubusercontent.com/514723273/.md-Pictures/master/%E4%BD%9C%E7%94%A8%E5%9F%9F%E9%93%BE1.png)

后台的每个执行环境都有一个表示变量的对象---变量对象。全局环境的变量对象始终存在，像 compare() 函数这样的局部环境变量对象，则*只在函数执行的过程中存在*。（执行完，立刻销毁）

在创建 compare() 函数时，会创建一个预先包含全局变量对象的作用域链，这个作用域链被保存在内部的[[Scope]]属性中。（注意：创建函数时，预先，*只有*全局变量对象的作用域，函数内部的[[Scope]]）（只创建了作用域链）

当调用 compare 函数时，会为函数创建一个执行环境，然后通过复制函数的 [[Scope]] 属性中的对象构建起执行环境的作用域链。（注意：调用时，）（创建了函数的执行环境）

此后，又有一个活动对象被创建并推入执行环境作用域的前端。（创建了变量对象）

无论什么时候再函数中访问一个变量时，就会从作用域中搜索具有相应名字的变量。

#### 7.2.1 闭包与变量

```js
function createComparisonFunction(propertyName) {

    return function(object1, object2) {
        var value1 = object1[propertyName];
        var value2 = object2[propertyName];

        //return value1 - value2;       字符串比较，返回 NaN
        if(value1 < value2) {
            return -1;
        } else if(value1 > value2) {
            return 1;
        } else {
            return 0;
        }
    }
}

//创建函数
var compareNames = createComparisonFunction("name");

//调用函数
var result = compareNames({name: "AA"}, {name: "BB"});      //-1

//解除对匿名函数的引用（以便释放内存）
compareNames = null;
```

![作用域链2](https://raw.githubusercontent.com/514723273/.md-Pictures/master/%E4%BD%9C%E7%94%A8%E5%9F%9F%E9%93%BE2.png)

createComparisonFunction() 函数在执行完毕后，其活动对象也不会销毁，因为匿名函数的作用域链仍然引用这个活动对象。（环境销毁，活动对象没有销毁）（作用域链链的依旧是对象）（所以才能访问到 name 这个变量）


#### 7.2.1 闭包与变量

副作用：即闭包只能取得包含函数中任何变量的最后一个值（执行到尾，不会再变化了）。

```js
function createFunctions() {
    var result = new Array();

    for(var i = 0; i < 10; i ++) {
        result[i] = function(i) {
            return i;
        };
    }

    return result;
}
```

这个函数会返回一个函数数组，结果是每个函数都返回 10 。因为作用域链上保存的是活动对象，引用的都是同一个 i ，当 createFunctions() 返回后，变量 i 的值是 10 。（创建了 10 个作用域链，但是链接的对象依旧只有三个， 全局， createFunction() ， 本身匿名（ 10 个））

```js
//创建另一个匿名函数强制让闭包的行为符合预期
//可以思考下此时的作用域链
function createFunctions() {
    var result = new Array();

    for(var i = 0; i < 10; i ++) {
        result[i] = function(num) {
            return function() {
                return num;
            };
        } (i);
    }

    return result;
}
```

#### 7.2.2 关于 this 对象

匿名函数的执行环境具有全局性，因此其 this 对象通常指向 window 。

一些关于 this 的细节。

```js
var name = "The Window";

var object = {
    name: "My Object",
    getNameFunc: function() {
        //var that = this;              //可以把外部作用域中的 this 对象保存在一个闭包能够访问到的变量里，就可以让闭包访问该对象了。
        return function() {
            return this.name;
            //return that.name;         //My Object 修改后的输出
        };
    }
}

//返回出来的就是一个普通匿名函数，当前的执行环境为 Window
alert(object.getNameFunc()());      //The Window （在非严格模式下）
```

#### 7.2.3 内存泄漏

如果闭包的作用域中保存着一个 HTML 元素，那么就意味着该元素将无法被销毁。

```js
function assignHandler() {
    var element = document.getElementById("someElement");

    //var id = element.id;  //消除循环引用
    element.onclick = function() {  //闭包创建了一个循环引用，导致无法减少 element 的引用数。
        alert(element.id);
        //alert(id);
    };
    //element = null;   手动释放
}
```

### 7.3 模仿块级作用域

```js
function outputNumbers(count) {
    for(var i = 0; i < count; i ++) {
        alert(i);
    }
    //var i;    //即使这样错误地重新声明同一个变量，也不会改变它地值（对后续地声明视而不见，但若有初始化则执行）
    alert(i);   //是定义在函数的活动对象中，可以在函数内部随处访问它

    // (function() {
    //     for(var i = 0; i < count; i ++) {
    //         alert(i);
    //     }
    // })();
}
```

匿名函数可以模仿块级作用域避免这个问题。
```js
(function() {
    //这里是块级作用域
})();

// function() {
//     //这里是块级作用域
// }();     //出错
//因为 JavaScript 将 function 关键字当作一个函数声明地开始，而函数声明不能跟括号。
//要将函数声明转换成函数表达式（加对括号即可）
```

这种技术经常在全局作用域中被用在函数外部，从而限制向全局作用域中添加过多的变量和函数。

### 7.4 私有变量

```js
function Person(name) {
    //特权方法，有权访问私有变量 name
    this.getName = function() {
        return name;
    };
    //特权方法，有权访问私有变量 name
    this.setName = function(value) {
        name = value;
    };
}

var person = new Person("AA");
alert(person.getName());    //AA    感觉是闭包方面，作用域
person.setName("BB");
alert(person.getName());    //BB

//缺点和构造函数模式一样，每个实例创建同样一组新方法
```

#### 7.4.1 静态私有变量

通过在私有作用域中定义私有变量或函数，同样也可以创建特权方法。

```js
(function() {
    //私有变量和函数由实例共享
    var name = "";      //外面访问不到块级作用域中的内容

    Person = function(value) {  //创建全局变量，没有使用 var 关键字（严格模式报错）
        name = value;
    }

    Person.prototype.getName() {
        return name;
    }
    Person.prototype.setName(value) {
        name = value;
    }
})();

var person1 = new Person("AA");
alert(person1.getName());   //AA
var person2 = new Person("BB");
person2.setName("DD");
alert(person2.getName());   //DD
alert(person1.getName());   //DD
```

#### 7.4.2 模块模式

前面的模式是用于自定义类型创建私有变量和特权方法的。而模块模式则是为**单例**创建私有变量和特权方法。

```js
var application = function() {
    //私有变量和函数
    var components = new Array();

    //初始化
    components.push(new BaseComponent());

    return {
        getComponentCount: function() {
            return components.length;
        },
        registerComponent: function(component) {
            if(typeof component == "object") {
                components.push(component);
            }
        }
    };
} ();
```

#### 7.4.3 增强的模块模式

在返回对象之前加入对其增强的代码。

```js
var application = function() {
    var components = new Array();

    components.push(new BaseComponent());

    var app = new BaseComponent();      //不同之处，app 是 BaseComponent 实例（增强）
    app.getComponentCount = function() {
        return components.length;
    }
    app.registerComponent = function(component) {
        if(typeof component == "object") {
            components.push(component);
        }
    }
    return app;
}();
```

## 第 8 章　BOM （粗略）

### 8.1　window对象

BOM 表示浏览器的一个实例。在浏览器中，window 对象有双重角色，它既是通过JavaScript访问浏览器窗口的一个接口，又是ECMAScript规定的Global 对象。

#### 8.1.1　全局作用域

尝试访问未声明的变量会抛出错误，但是通过查询window 对象，可以知道某个可能未声明的变量是否存在。例如：
```js
//这里会抛出错误，因为oldValue未定义
var newValue = oldValue;

//这里不会抛出错误，因为这是一次属性查询
//newValue的值是undefined
var newValue = window.oldValue;     // 查询对象
```

#### 8.1.2　窗口关系及框架

如果页面中包含框架`frame`，则每个框架都拥**有自己的window 对象**，并且保存在frames 集合中。

在frames 集合中，可以通过数值索引（从0开始，从左至右，从上到下）或者框架名称来访问相应的window 对象。每个window 对象都有一个name 属性，其中包含框架的名称。

```html
<html>
    <head>
        <title>Frameset Example</title>
    </head>
    <frameset rows="160,*">
        <frame src="frame.htm" name="topFrame">
        <frameset cols="50%,50%">
            <frame src="anotherframe.htm" name="leftFrame">
            <frame src="yetanotherframe.htm" name="rightFrame">
        </frameset>
    </frameset>
</html>

<!-- 
    以上代码创建了一个框架集，其中一个框架居上，两个框架居下。
    对这个例子而言，可以通过window.frames[0] 或者window.frames["topFrame"] 来引用上方的框架。
    不过，恐怕你最好使用 top 而非 window 来引用这些框架（例如，通过top.frames[0] ）。
-->
```

![BOM_frame](https://raw.githubusercontent.com/514723273/.md-Pictures/master/BOM_frame.png)

#### 8.1.3　窗口位置

用来确定和修改window 对象位置的属性和方法有很多。

IE、Safari、Opera和Chrome都提供了screenLeft 和screenTop 属性，分别用于表示窗口相对于屏幕左边和上边的位置。

Firefox则在screenX 和screenY 属性中提供相同的窗口位置信息，

Safari和Chrome也同时支持这两个属性。

Opera虽然也支持screenX 和screenY 属性，但与screenLeft 和screenTop 属性并不对应，因此建议大家不要在Opera中使用它们。

```js
var leftPos = (typeof window.screenLeft == "number") ?
                  window.screenLeft : window.screenX;
var topPos = (typeof window.screenTop == "number") ?
                  window.screenTop : window.screenY;
```

#### 8.1.4　窗口大小

跨浏览器确定一个窗口的大小不是一件简单的事。

IE9+、Firefox、Safari、Opera和Chrome均为此提供了4个属性：innerWidth 、innerHeight 、outerWidth 和outerHeight 。

#### 8.1.5　导航和打开窗口

使用window.open() 方法既可以导航到一个特定的URL，也可以打开一个新的浏览器窗口。

这个方法可以接收4个参数：要加载的URL、窗口目标、一个特性字符串以及一个表示新页面是否取代浏览器历史记录中当前加载页面的布尔值。*(通常只须传递第一个参数，最后一个参数只在不打开新窗口的情况下使用。)*

1. 弹出窗口

如果给window.open() 传递的第二个参数并不是一个已经存在的窗口或框架，那么该方法就会根据在第三个参数位置上传入的字符串创建一个新窗口或新标签页。如果没有传入第三个参数，那么就会打开一个带有全部默认设置（工具栏、地址栏和状态栏等）的新浏览器窗口（或者打开一个新标签页——根据浏览器设置）。在不打开新窗口的情况下，会忽略第三个参数。

2. 安全限制

曾经有一段时间，广告商在网上使用弹出窗口达到了肆无忌惮的程度。他们经常把弹出窗口打扮成系统对话框的模样，引诱用户去点击其中的广告。由于看起来像是系统对话框，一般用户很难分辨是真是假。为了解决这个问题，有些浏览器开始在弹出窗口配置方面增加限制。

3. 弹出窗口屏蔽程序

在弹出窗口被屏蔽时，就应该考虑两种可能性：
- 如果是浏览器内置的屏蔽程序阻止的弹出窗口，那么window.open() 很可能会返回 null
- 如果是浏览器扩展或其他程序阻止的弹出窗口，那么window.open() 通常会抛出一个错误。

```js
var blocked = false;

try {
    var wroxWin = window.open("http://www.wrox.com", "_blank");
    if (wroxWin == null){
        blocked = true;
    }
} catch (ex) {
    blocked = true;
}

if (blocked) {
    alert("The popup was blocked!");
}
```

#### 8.1.6 间歇调用和超时调用

- 超时调用：`setTimeout()`
- 间歇调用：`setInterval()`

JavaScript是一个单线程序的解释器，因此一定时间内只能执行一段代码。

为了控制要执行的代码，就有一个JavaScript任务队列。这些任务会按照将它们添加到队列的顺序执行。

setTimeout() 的第二个参数告诉JavaScript再过多长时间把当前任务添加到队列中。

如果队列是空的，那么添加的代码会立即执行；**如果队列不是空的，那么它就要等前面的代码执行完了以后再执行。**（所以一般不准时执行


```js
// 设置超时调用
// 调用setTimeout() 之后，该方法会返回一个数值ID，表示超时调用。这个超时调用ID是计划执行代码的唯一标识符，可以通过它来取消超时调用。
var timeoutId = setTimeout(function() {
    alert("Hello world!");
}, 1000);

// 注意：把它取消
// 只要是在指定的 *时间尚未过去之前调用* clearTimeout() ，就可以完全取消超时调用。前面的代码在设置超时调用之后马上又调用了clearTimeout() ，结果就跟什么也没有发生一样。
clearTimeout(timeoutId);
```

```js
var num = 0;
var max = 10;
var intervalId = null;      // id

function incrementNumber() {
    num++;

    //如果执行次数达到了max设定的值，则取消后续尚未执行的调用
    if (num == max) {
        clearInterval(intervalId);
        alert("Done");
    }
}

// 调用setInterval() 方法同样也会返回一个间歇调用ID，该ID可用于在将来某个时刻取消间歇调用。
intervalId = setInterval(incrementNumber, 500);

```

间歇调用完全可以用超时调用替换，使用超时调用来模拟间歇调用的是一种最佳模式。（最好不要使用间歇调用。）
```js

var num = 0;
var max = 10;

function incrementNumber() {
    num++;

    //如果执行次数未达到max设定的值，则设置另一次超时调用
    if (num < max) {
        setTimeout(incrementNumber, 500);
    } else {
        alert("Done");
    }
}
setTimeout(incrementNumber, 500);
```

#### 8.1.7　系统对话框 

浏览器通过 alert() 、confirm() 和prompt() 方法可以调用系统对话框向用户显示消息。

系统对话框与在浏览器中显示的网页没有关系，也不包含HTML。它们的外观由操作系统及（或）浏览器设置决定，而不是由CSS决定。

此外，通过这几个方法打开的对话框都是同步和模态的。也就是说，显示这些对话框的时候代码会停止执行，而关掉这些对话框后代码又会恢复执行。

- alert() 向用户显示一个系统对话框，其中包含指定的文本和一个OK（“确定”）按钮。
- confirm() 从向用户显示消息的方面来看，这种“确认”对话框很像是一个“警告”对话框。但二者的主要区别在于“确认”对话框除了显示OK按钮外，还会显示一个Cancel（“取消”）按钮
- prompt() 方法生成的，这是一个“提示”框，用于提示用户输入一些文本。提示框中除了显示OK和Cancel按钮之外，还会显示一个文本输入域，以供用户在其中输入内容。prompt() 方法接受两个参数：要显示给用户的文本提示和文本输入域的默认值（可以是一个空字符串）。

还有两个可以通过JavaScript打开的对话框，即“查找”和“打印”。这两个对话框都是异步显示的，能够将控制权立即交还给脚本。

这两个对话框与用户通过浏览器菜单的“查找”和“打印”命令打开的对话框相同。
```js
//显示“打印”对话框
window.print();

//显示“查找”对话框
window.find();
```

### 8.2　location对象

location 是最有用的BOM对象之一，它提供了*与当前窗口中加载的文档有关的信息*，还提供了一些导航功能。

事实上，location 对象是很特别的一个对象，因为它既是window 对象的属性，也是document 对象的属性；换句话说，window.location 和document.location 引用的是**同一个对象。**

location 对象的用处不只表现在它保存着当前文档的信息，还表现在它将URL解析为**独立的片段**，让开发人员可以通过不同的属性访问这些片段。

下表列出了location 对象的所有属性:

|属　性　名|例　　子|说　　明|
| --- | --- | --- |
|hash| "#contents" |返回URL中的hash （#号后跟零或多个字符），如果URL中不包含散列，则返回空字符串|
|host|"www.wrox.com:80" |返回服务器名称和端口号（如果有）|
|hostname|"www.wrox.com" |返回不带端口号的服务器名称|
|href |"http:/www.wrox.com" |返回当前加载页面的完整URL。而location 对象的toString() 方法也返回这个值|
|pathname |"/WileyCDA/" |返回URL中的目录和（或）文件名|
|port |"8080" |返回URL中指定的端口号。如果URL中不包含端口号，则这个属性返回空字符串|
|protocol |"http:" |返回页面使用的协议。通常是http: 或https: |
|search |"?q=javascript" |返回URL的查询字符串。这个字符串以问号开头|

#### 8.2.1　查询字符串参数

写了一个函数解析了例如`?q=javascript&num=10`这样的字符串，把它变成对象，方便使用。

```js
function getQueryStringArgs(){
    //取得查询字符串并去掉开头的问号
    var qs = (location.search.length > 0 ? location.search.substring(1) : ""),

    //保存数据的对象
    args = {},

    //取得每一项
    items = qs.length ? qs.split("&") : [],
    item = null,
        name = null,
        value = null,

        //在for循环中使用
        i = 0,
        len = items.length;

    //逐个将每一项添加到args对象中
    for (i=0; i < len; i++){
        item = items[i].split("=");
        name = decodeURIComponent(item[0]);
        value = decodeURIComponent(item[1]);

        if (name.length) {
            args[name] = value;
        }
    }

    return args;
}
```

#### 8.2.2　位置操作

使用 location 对象可以通过很多方式来改变浏览器的位置。

```js
// 这样，就可以立即打开新URL并在浏览器的历史记录中生成一条记录。
location.assign("http://www.wrox.com");
// 如果是将location.href 或window.location 设置为一个URL值，也会以该值调用assign() 方法。
// 例如，下列两行代码与显式调用assign() 方法的效果完全一样。
window.location = "http://www.wrox.com";
location.href = "http://www.wrox.com";      // 最常用
```

另外，修改location 对象的其他属性也可以改变当前加载的页面。下面的例子展示了通过将hash 、search 、hostname 、pathname 和port 属性设置为新值来改变URL。
（就是把表格里的属性都改变）
```js
//假设初始URL为http://www.wrox.com/WileyCDA/

//将URL修改为"http://www.wrox.com/WileyCDA/#section1"
location.hash = "#section1";

//将URL修改为"http://www.wrox.com/WileyCDA/?q=javascript"
location.search = "?q=javascript";

//将URL修改为"http://www.yahoo.com/WileyCDA/"
location.hostname = "www.yahoo.com";

//将URL修改为"http://www.yahoo.com/mydir/"
location.pathname = "mydir";

//将URL修改为"http://www.yahoo.com:8080/WileyCDA/"
location.port = 8080;

// 每次修改location 的属性（hash 除外），页面都会以新URL重新加载。
```

当通过上述任何一种方式修改URL之后，浏览器的历史记录中就会生成一条新记录，因此用户通过单击“后退”按钮都会导航到前一个页面。要禁用这种行为，可以使用replace() 方法（*不会生成记录）（这个方法只接受一个参数，即要导航到的URL*）。

```js
location.replace("http://www.wrox.com/");
```

与位置有关的最后一个方法是reload() ，作用是重新加载当前显示的页面。如果调用reload() 时不传递任何参数，页面就会以最有效的方式重新加载。

```js
location.reload();        //重新加载（有可能从缓存中加载）
location.reload(true);    //重新加载（从服务器重新加载）
// 后面代码可能不执行。为此，最好将reload() 放在代码的最后一行。
```

### 8.3　navigator 对象

#### 8.3.1 检测插件

检测浏览器中是否安装了特定的插件是一种最常见的检测例程。对于非IE浏览器，可以使用 **plugins 数组**来达到这个目的。该数组中的每一项都包含下列属性。

- name ：插件的名字。
- description ：插件的描述。
- filename ：插件的文件名。
- length ：插件所处理的MIME类型数量。

```js
//检测插件（在IE中无效）
function hasPlugin(name){
    name = name.toLowerCase();
    for (var i=0; i < navigator.plugins.length; i++){
        if (navigator.plugins[i].name.toLowerCase().indexOf(name) > -1){
            return true;
        }
    }

    return false;
}

//检测Flash
alert(hasPlugin("Flash"));

//检测QuickTime
alert(hasPlugin("QuickTime"));
```

#### 8.3.2 注册处理程序

Firefox 2 为 navigator 对象新增了registerContentHandler() 和registerProtocolHandler() 方法。

这两个方法可以让一个站点指明它可以处理特定类型的信息。

### 8.4 screen对象

用处不大，screen 对象基本上只用来表明**客户端的能力**，其中包括浏览器窗口外部的**显示器的信息**，如像素宽度和高度等。

### 8.5 history 对象

history 对象保存着用户上网的历史记录，从窗口被打开的那一刻算起。

```js
// 后退一页
history.go(-1);

// 前进一页
history.go(1);

// 前进两页
history.go(2);

```
```js

// 此时浏览器会跳转到历史记录中包含该字符串的第一个位置——可能后退，也可能前进，具体要看哪个位置最近。

// 跳转到最近的wrox.com页面
history.go("wrox.com");

// 跳转到最近的nczonline.net页面
history.go("nczonline.net");
```

```js
//后退一页
history.back();

//前进一页
history.forward();
```

```js
if (history.length == 0){
    //这应该是用户打开窗口后的第一个页面
}
```

## 第 9 章　客户端检测

### 9.1　能力检测

存在该方法，才使用该方法。

#### 9.1.1 更可靠的能力检测

#### 9.1.2　能力检测，不是浏览器检测

### 9.2 怪癖检测

### 9.3 用户代理检测

（迫不得己的使用方式）用户代理检测通过检测**用户代理字符串**来确定实际使用的浏览器。

#### 9.3.1 用户代理字符串的历史

#### 9.3.2　用户代理字符串检测技术

## 第 10 章 DOM

DOM（文档对象模型）是针对HTML和XML文档的一个API（应用程序编程接口）。

DOM描绘了一个层次化的节点树，允许开发人员添加、移除和修改页面的某一部分。

### 10.1　节点层次

DOM可以将任何HTML或XML文档描绘成一个**由多层节点构成的结构**。

节点分为几种不同的类型，每种类型分别表示文档中不同的信息及（或）标记。

每个节点都拥有**各自的特点、数据和方法**，另外也与其他节点存在某种关系。


`<html>`元素，称之为**文档元素**。每个文档只能有一个文档元素（最外层）。

每一段标记都可以通过树中的一个节点来表示（总共有12种节点类型，这些类型都继承自一个基类型。）：
1. HTML元素通过元素节点表示
2. 特性（attribute）通过特性节点表示
3. 文档类型通过文档类型节点表示
4. 注释则通过注释节点表示


#### 10.1.1 Node 类型

JavaScript中的所有节点类型都**继承**自 Node 类型，因此所有节点类型都共享着相同的基本属性和方法。

每个节点都有一个nodeType 属性，用于表明节点的类型：
1. Node.ELEMENT_NODE
2. Node.ATTRIBUTE_NODE
3. Node.TEXT_NODE
4. Node.CDATA_SECTION_NODE
5. Node.ENTITY_REFERENCE_NODE
6. Node.ENTITY_NODE
7. Node.PROCESSING_INSTRUCTION_NODE
8. Node.COMMENT_NODE
9. Node.DOCUMENT_NODE
10. Node.DOCUMENT_TYPE_NODE
11. Node.DOCUMENT_FRAGMENT_NODE
12. Node.NOTATION_NODE

```js
if (someNode.nodeType == Node.ELEMENT_NODE) {   //在IE中无效
    alert("Node is an element.");
}

if (someNode.nodeType == 1) {    //适用于所有浏览器
    alert("Node is an element.");
}
```

1. nodeName 和 nodeValue 属性

对于元素节点，nodeName 中保存的始终都是元素的标签名，而 nodeValue 的值则始终为 null 。

2. 节点关系

每个节点都有一个childNodes 属性，其中保存着一个NodeList 对象。（它并不是Array 的实例）

NodeList 对象的独特之处在于，它实际上是基于DOM结构**动态**执行查询的结果。

```js
// 访问保存在NodeList 中的节点——可以通过方括号，也可以使用item() 方法。
var firstChild = someNode.childNodes[0];
var secondChild = someNode.childNodes.item(1);

var count = someNode.childNodes.length;

// 使用Array.prototype.slice() 方法可以将其转换为数组。
var arrayOfNodes = Array.prototype.slice.call(someNode.childNodes, 0);
```

父节点的firstChild 和lastChild 属性分别指向其childNodes 列表中的第一个和最后一个节点。

![XXX](https://raw.githubusercontent.com/514723273/.md-Pictures/master/js红宝书-节点关系.png)


父节点的firstChild 和lastChild 属性分别指向其childNodes 列表中的第一个和最后一个节点。

3. 操作节点

最后增 appendChild() ：用于向childNodes 列表的末尾添加一个节点。
```js
// appendChild() ，用于向 childNodes 列表的末尾添加一个节点。
var returnedNode = someNode.appendChild(newNode);
// 添加节点后，childNodes 的新增节点、父节点及以前的最后一个子节点的关系指针都会相应地得到更新。
alert(returnedNode == newNode);     // true
alert(somNode.last == newNode);     // true
```
```js
// 如果传入到appendChild() 中的节点已经是文档的一部分了，那结果就是将该节点从原来的位置转移到新位置。
//someNode 有多个子节点
var returnedNode = someNode.appendChild(someNode.firstChild);
alert(returnedNode == someNode.firstChild);      // false
alert(returnedNode == someNode.lastChild);       // true
```

特定位置增 insertBefore() ：把节点放在childNodes 列表中某个特定的位置上。（这个方法接受两个参数：要插入的节点和作为参照的节点。
```js
//插入后成为最后一个子节点
returnedNode = someNode.insertBefore(newNode, null);
alert(newNode == someNode.lastChild);   //true

//插入后成为第一个子节点
var returnedNode = someNode.insertBefore(newNode, someNode.firstChild);
alert(returnedNode == newNode);         //true
alert(newNode == someNode.firstChild);  //true

//插入到最后一个子节点前面
returnedNode = someNode.insertBefore(newNode, someNode.lastChild);
alert(newNode == someNode.childNodes[someNode.childNodes.length-2]); //true
```

替换 replaceChild() ：接受的两个参数是：要插入的节点和要替换的节点。要替换的节点将由这个方法返回并从文档树中被移除，同时由要插入的节点占据其位置。
```js
//替换第一个子节点
var returnedNode = someNode.replaceChild(newNode, someNode.firstChild);
//替换最后一个子节点
returnedNode = someNode.replaceChild(newNode, someNode.lastChild);
```
删除 removeChild()
```js
//移除第一个子节点
var formerFirstChild = someNode.removeChild(someNode.firstChild);       // 被移除的节点将成为方法的返回值

//移除最后一个子节点
var formerLastChild = someNode.removeChild(someNode.lastChild);
```

4. 其他方法

cloneNode() ：深拷贝和浅拷贝。
```js
var deepList = myList.cloneNode(true);  // true 深拷贝
alert(deepList.childNodes.length);      //3（IE < 9）或7（其他浏览器）

var shallowList = myList.cloneNode(false);      // 浅拷贝，只拷贝元素节点本身，不拷贝子元素
alert(shallowList.childNodes.length);   //0
```

normalize() ：处理文档树中的文本节点。

#### 10.1.2 Document 类型

Document 节点具有下列特征：
- nodeType 的值为9；
- nodeName 的值为"#document" ；
- nodeValue 的值为null ；
- parentNode 的值为null ；
- ownerDocument 的值为null ；
- 其子节点可能是一个DocumentType （最多一个）、Element （最多一个）、ProcessingInstruction 或Comment 。

1. 文档的子节点

```js
//1. 取得对<html>的引用
var html = document.documentElement;
alert(html === document.childNodes[0]);     //true
alert(html === document.firstChild);        //true

//2. 取得对<body>的引用
var body = document.body;

//3. 取得对<!DOCTYPE>的引用
var doctype = document.doctype;     
```

2. 文档信息

```js
//1. 取得文档标题
var originalTitle = document.title;

//2. 设置文档标题
document.title = "New page title";      // 修改当前页面的标题并反映在浏览器的标题栏中。修改title 属性的值不会改变<title> 元素。
```

3. 查找元素

```js
// 1. 取得<div>元素的引用
var div = document.getElementById("myDiv");

// 2. 取得页面中所有的 <img> 元素，并返回一个HTMLCollection 。(为一个“动态”集合，该对象与NodeList 非常类似。) <img src="myimage.gif" name="myImage">
var images = document.getElementByTagName("img");
alert(images.length);          //输出图像的数量
alert(images[0].src);          //输出第一个图像元素的src特性
alert(images.item(0).src);     //输出第一个图像元素的src特性
var myImage = images.namedItem("myImage");  // 使用这个方法可以通过元素的 name 特性取得集合中的项
var myImage = images["myImage"];            // 功能同上     // 在后台，对数值索引就会调用item() ，而对字符串索引就会调用namedItem() 。
var allElements = document.getElementsByTagName("*");       // 取得文档中的所有元素 // 第一项是<html> 元素，第二项是<head> 元素，按顺序出现

// 3. 返回带有给定 name 特性的所有元素。 <input type="radio" value="green" name="color" id="colorGreen">
var radios = document.getElementsByName("color");
```

4. 特殊集合

- `document.anchors` ，包含文档中所有带name 特性的`<a>` 元素；
- `document.forms` ，包含文档中所有的<form> 元素，与document.getElementsByTagName("form") 得到的结果相同；
- `document.images` ，包含文档中所有的<img> 元素，与document.getElementsByTagName("img") 得到的结果相同；
- `document.links` ，包含文档中所有带href 特性的`<a>` 元素。

5. DOM一致性检测

`document.implementation` 检测浏览器实现了DOM的哪些部分

```js
// 传递参数：要检测的DOM功能的名称及版本号。如果浏览器支持给定名称和版本的功能，则该方法返回true
var hasXmlDom = document.implementation.hasFeature("XML", "1.0");
```

6. 文档写入

将输出流写入到网页中的能力。这个能力体现在下列4个方法中：write() 、writeln() 、open() 和close()

```html
<html>
<head>
    <title>document.write() Example</title>
</head>
<body>
    <p>The current date and time is:
    <script type="text/javascript">
        document.write("<strong>" + (new Date()).toString() + "</strong>");
    </script>
    </p>
</body>
</html>

```

### 10.1.3 Element 类型

Element 节点具有以下特征：
- nodeType 的值为1；
- nodeName 的值为元素的标签名；
- nodeValue 的值为null ；
- parentNode 可能是Document 或Element ；
- 其子节点可能是Element 、Text 、Comment 、ProcessingInstruction 、CDATASection 或EntityReference 。


1. HTML元素

- id ，元素在文档中的唯一标识符。
- title ，有关元素的附加说明信息，一般通过工具提示条显示出来。
- lang ，元素内容的语言代码，很少使用。
- dir ，语言的方向，值为"ltr" （left-to-right，从左至右）或"rtl" （right-to-left，从右至左），也很少使用。
- className ，与元素的class 特性对应，即为元素指定的CSS类。没有将这个属性命名为class ，是因为class 是ECMAScript的保留字。


```js
// <div id="myDiv" class="bd" title="Body text" lang="en" dir="ltr"></div>
var div = document.getElementById("myDiv");
alert(div.id);             //"myDiv""
alert(div.className);      //"bd"
alert(div.title);          //"Body text"
alert(div.lang);           //"en"
alert(div.dir);            //"ltr"

div.id = "someOtherId";
div.className = "ft";
div.title = "Some other text";
div.lang = "fr";
div.dir ="rtl";
```

2. 取得特性

操作特性的DOM方法主要有三个，分别是getAttribute() 、setAttribute() 和removeAttribute() 

```js
var div = document.getElementById("myDiv");
alert(div.getAttribute("id"));         //"myDiv"
alert(div.getAttribute("class"));      //"bd"           // 传入"class" 而不是"className" 
alert(div.getAttribute("title"));      //"Body text"
alert(div.getAttribute("lang"));       //"en"
alert(div.getAttribute("dir"));        //"ltr"
```

由于存在差别(onclick style)，在通过JavaScript以编程方式操作DOM时，开发人员**经常不使用**getAttribute() ，而是只**使用对象的属性**。只有在取得自定义特性值的情况下，才会使用getAttribute() 方法。

3. 设置特性

```js
div.setAttribute("id", "someOtherId");
div.setAttribute("class", "ft");
div.setAttribute("title", "Some other text");
div.setAttribute("lang","fr");
div.setAttribute("dir", "rtl");
```

4. attributes 属性

Element 类型是使用attributes 属性的**唯一一个** DOM 节点类型。

attributes 属性中包含一个NamedNodeMap ，与NodeList 类似，也是一个“动态”的集合，拥有如下方法：
- getNamedItem_(name)_ ：返回nodeName 属性等于 name 的节点；
- removeNamedItem_(name)_ ：从列表中移除nodeName 属性等于 name 的节点；
- setNamedItem_(node)_ ：向列表中添加节点，以节点的nodeName 属性为索引；
- item_(pos)_ ：返回位于数字 pos 位置处的节点。

```js
var id = element.attributes.getNamedItem("id").nodeValue;
var id = element.attributes["id"].nodeValue;        // 简写
element.attributes["id"].nodeValue = "someOtherId";     // 设置特性的新值
var oldAttr = element.attributes.removeNamedItem("id");     // 返回表示被删除特性的 Attr 节点
element.attributes.setNamedItem(newAttr);           // 添加一个新特性，为此需要为它传入一个特性节点
```

5. 创建元素

```js
var div = document.createElement("div");        // 创建一个<div> 元素。     // 在使用createElement() 方法创建新元素的同时，也为新元素设置了ownerDocuemnt 属性。
div.id = "myNewDiv";
div.className = "box";
// 把新元素添加到文档树
document.body.appendChild(div);
```

6. 元素的子节点

素的childNodes 属性中包含了它的所有子节点，这些子节点有可能是元素、文本节点、注释或处理指令。


体验浏览器差别（可能现在不存在了）：
```html
<ul id="myList">
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
</ul>
```

如果是IE来解析这些代码，那么<ul> 元素会有3个子节点，分别是3个<li> 元素。

但如果是在其他浏览器中，<ul> 元素都会有7个元素，包括3个<li> 元素和4个文本节点（表示<li> 元素之间的空白符）。

#### 10.1.4 Text 类型

Text 节点具有以下特征：
- nodeType 的值为3；
- nodeName 的值为"#text" ；
- nodeValue 的值为节点所包含的文本；
- parentNode 是一个Element ；
- 不支持（没有）子节点。

使用下列方法可以操作节点中的文本。
- appendData(text ) ：将 text 添加到节点的末尾。
- deleteData(offset , count ) ：从 offset 指定的位置开始删除 count 个字符。
- insertData(offset, text ) ：在 offset 指定的位置插入 text 。
- replaceData(offset, count, text ) ：用 text 替换从 offset 指定的位置开始到 offset + count 为止处的文本。
- splitText(offset ) ：从 offset 指定的位置将当前文本节点分成两个文本节点。
- substringData(offset, count ) ：提取从 offset 指定的位置开始到 offset+count 为止处的字符串。

```js
// <div>Hello World!</div>
var textNode = div.firstChild;     //或者div.childNodes[0]
div.firstChild.nodeValue = "Some other message";

//输出结果是"Some &lt;strong&gt;other&lt;/strong&gt; message"   // 此时的字符串会经过HTML（或XML，取决于文档类型）编码。
div.firstChild.nodeValue = "Some <strong>other</strong> message";
```

1. 创建文本节点

```js
var element = document.createElement("div");
element.className = "message";

var textNode = document.createTextNode("Hello world!");     // document.createTextNode() 
element.appendChild(textNode);

document.body.appendChild(element);
```

2. 规范化文本节点

```js
var element = document.createElement("div");
element.className = "message";

var textNode = document.createTextNode("Hello world!");
element.appendChild(textNode);

var anotherTextNode = document.createTextNode("Yippee!");
element.appendChild(anotherTextNode);

document.body.appendChild(element);

alert(element.childNodes.length);    // 2

// 将相邻文本节点合并
element.normalize();                

alert(element.childNodes.length);    // 1

alert(element.firstChild.nodeValue);    // "Hello world!Yippee!"
```

3. 分割文本节点

```js
var element = document.createElement("div");
element.className = "message";

var textNode = document.createTextNode("Hello world!");
element.appendChild(textNode);

document.body.appendChild(element);

// splitText() 这个方法会将一个文本节点分成两个文本节点，即按照指定的位置分割nodeValue 值。
var newNode = element.firstChild.splitText(5);      

alert(element.firstChild.nodeValue);    // "Hello"

alert(newNode.nodeValue);               //" world!"

alert(element.childNodes.length);       // 2
```

#### 10.1.5　Comment类型

Comment 节点具有下列特征：
- nodeType 的值为8；
- nodeName 的值为"#comment" ；
- nodeValue 的值是注释的内容；
- parentNode 可能是Document 或Element ；
- 不支持（没有）子节点。

```js
// <div id="myDiv"><!--A comment --></div>
var div = document.getElementById("myDiv");
var comment = div.firstChild;
aler(comment.data);     // "A comment"
```

```js
var comment = document.createComment("A comment ");
```

#### 10.1.6　CDATASection类型

CDATASection 类型继承自Text 类型，CDATA区域只会出现在XML文档中，CDATASection 节点具有下列特征：

#### 10.1.7　DocumentType类型

DocumentType 类型在Web浏览器中并不常用，**仅有**Firefox、Safari和Opera支持它。

#### 10.1.8　DocumentFragment类型

假设我们想为这个<ul> 元素添加3个列表项

低效做法：逐个地添加列表项，将会导致浏览器反复渲染（呈现）新信息。

高效做法：使用一个文档片段来保存创建的列表项，然后再一次性将它们添加到文档中。

```js
var fragment = document.createDocumentFragment();
var ul = document.getElementById("myList");
var li = null;

for (var i=0; i < 3; i++){
    li = document.createElement("li");
    li.appendChild(document.createTextNode("Item " + (i+1)));
    fragment.appendChild(li);
}

ul.appendChild(fragment);       // 此时，文档片段的所有子节点都被删除并转移到了<ul> 元素中。
```

#### 10.1.9 Attr 类型

```js
// 使用document.createAttribute() 并传入特性的名称可以创建新的特性节点。
var attr = document.createAttribute("align");
attr.value = "left";

element.setAttributeNode(attr);

alert(element.attributes["align"].value);       //"left"
alert(element.getAttributeNode("align").value); //"left"
alert(element.getAttribute("align"));           //"left"
```

### 10.2 DOM操作技术

#### 10.2.1　动态脚本

```js
var script = document.createElement("script");
script.type = "text/javascript";
script.src = "client.js";
document.body.appendChild(script);
```

#### 10.2.2　动态样式

```js
var link = document.createElement("link");
link.rel = "stylesheet";
link.type = "text/css";
link.href = "style.css";
var head = document.getElementsByTagName("head")[0];

head.appendChild(link);     // 必须将<link> 元素添加到<head> 而不是<body> 元素，才能保证在所有浏览器中的行为一致。
```

#### 10.2.3 操作表格


## 第 11 章 DOM 扩展

### 11.1　选择符API

#### 11.1.1　querySelector() 方法

```js
//取得body元素
var body = document.querySelector("body");

//取得ID为"myDiv"的元素
var myDiv = document.querySelector("#myDiv");

//取得类为"selected"的第一个元素
var selected = document.querySelector(".selected");

//取得类为"button"的第一个图像元素
var img = document.body.querySelector("img.button");
```

#### 11.1.2　querySelectorAll() 方法

```js
//取得某<div>中的所有<em>元素（类似于getElementsByTagName("em")）
var ems = document.getElementById("myDiv").querySelectorAll("em");

//取得类为"selected"的所有元素
var selecteds = document.querySelectorAll(".selected");

//取得所有<p>元素中的所有<strong>元素
var strongs = document.querySelectorAll("p strong");

var i, len, strong;
for (i=0, len=strongs.length; i < len; i++){ 
    strong = strongs[i];   //或者strongs.item(i)
    strong.className = "important";
}
```

#### 11.1.3　matchesSelector()方法

```js
// 在取得某个元素引用的情况下，使用这个方法能够方便地检测它是否会被querySelector() 或querySelectorAll() 方法返回。
if (document.body.matchesSelector("body.page1")){ 
    //true
}
```

#### 11.2　元素遍历

利用这些元素不必担心空白文本节点，从而可以更方便地查找DOM元素了。

- childElementCount ：返回子元素（不包括文本节点和注释）的个数。
- firstElementChild ：指向第一个子元素；firstChild 的元素版。
- lastElementChild ：指向最后一个子元素；lastChild 的元素版。
- previousElementSibling ：指向前一个同辈元素；previousSibling 的元素版。
- nextElementSibling ：指向后一个同辈元素；nextSibling 的元素版。

过去，要跨浏览器遍历某元素的所有子元素，需要像下面这样写代码:
```js
var i, 
    len,
    child = element.firstChild;
while(child != element.lastChild){
    if (child.nodeType == 1){   //检查是不是元素
       processChild(child);
    }
    child = child.nextSibling;
}
```
而使用Element Traversal新增的元素，代码会更简洁:
```js
var i, 
    len,
    child = element.firstElementChild;
while(child != element.lastElementChild){
    processChild(child);   //已知其是元素
    child = child.nextElementSibling;
}
```

### 10.3 HTML5

#### 11.3.1 与类相关的扩充

1. getElementsByClassName() 方法

```js
//取得所有类中包含"username"和"current"的元素，类名的先后顺序无所谓
var allCurrentUsernames = document.getElementsByClassName("username current");

//取得ID为"myDiv"的元素中带有类名"selected"的所有元素
var selected = document.getElementById("myDiv").getElementsByClassName("selected");
```

2. classList 属性

- add(value ) ：将给定的字符串值添加到列表中。如果值已经存在，就不添加了。
- contains(value ) ：表示列表中是否存在给定的值，如果存在则返回true ，否则返回false 。
- remove(value ) ：从列表中删除给定的字符串。
- toggle(value ) ：如果列表中已经存在给定的值，删除它；如果列表中没有给定的值，添加它。

```js
//删除"disabled"类
div.classList.remove("disabled");

//添加"current"类
div.classList.add("current");

//切换"user"类
div.classList.toggle("user");

//确定元素中是否包含既定的类名
if (div.classList.contains("bd") && !div.classList.contains("disabled")){
    //执行操作
}

//迭代类名
for (var i=0, len=div.classList.length; i < len; i++){
    doSomething(div.classList[i]);
}
```

#### 11.3.2　焦点管理

```js
// 这个属性始终会引用DOM中当前获得了焦点的元素。
var button = document.getElementById("myButton");
button.focus();
alert(document.activeElement === button);   //true

// 文档刚刚加载完成时，document.activeElement 中保存的是document.body 元素的引用。文档加载期间，document.activeElement 的值为null 。
```

```js
var button = document.getElementById("myButton");
button.focus();

// 这个方法用于确定文档是否获得了焦点。
alert(document.hasFocus());  //true
```

#### 11.3.3　HTMLDocument 的变化

1. readyState 属性

- loading ，正在加载文档；
- complete ，已经加载完文档。

```js
// 通过它来实现一个指示文档已经加载完成的指示器。
if (document.readyState == "complete"){ 
    //执行操作
}
```

2. 兼容模式

#### 11.3.4　字符集属性

```js
alert(document.charset); //"UTF-16"
document.charset = "UTF-8";
```

```js

// 如果文档没有使用默认的字符集，那charset 和defaultCharset 属性的值可能会不一样
if (document.charset != document.defaultCharset){ 
    alert("Custom character set being used.");
}
```

#### 11.3.5　自定义数据属性

HTML5规定可以为元素添加非标准的属性，但要添加前缀data- ，目的是为元素提供与渲染无关的信息，或者提供语义信息。

添加了自定义属性之后，可以通过元素的dataset 属性来访问自定义属性的值。

```js
// <div id="myDiv" data-appId="12345" data-myname="Nicholas"></div>

var div = document.getElementById("myDiv");

//取得自定义属性的值
var appId = div.dataset.appId;
var myName = div.dataset.myname;

//设置值
div.dataset.appId = 23456;
div.dataset.myname = "Michael";

//有没有"myname"值呢？
if (div.dataset.myname){
    alert("Hello, " + div.dataset.myname);
}
```

#### 11.3.6　插入标记

1. innerHTML 属性

```html
<div id="content"> 
    <p>This is a <strong>paragraph</strong> with a list following it.</p>
    <ul>
        <li>Item 1</li>
        <li>Item 2</li>
        <li>Item 3</li>
    </ul>
</div>
```
对于上面的<div> 元素来说，它的innerHTML 属性会返回`<p>...</p>`字符串。


```js
div.innerHTML = "Hello world!";     // 在写模式下，innerHTML 的值会被解析为DOM子树
```

2. outerHTML 属性

如果在<div> 元素上调用outer HTML，会返回包括<div> 本身字符串。
```js
div.outerHTML = "<p>This is a paragraph.</p>";      // 连 div 都被替换掉了
```

3. insertAdjacentHTML() 方法

它接收两个参数：插入位置和要插入的HTML文本。第一个参数必须是下列值之一：
- "beforebegin" ，在当前元素之前插入一个紧邻的同辈元素；
- "afterbegin" ，在当前元素之下插入一个新的子元素或在第一个子元素之前再插入新的子元素；
- "beforeend" ，在当前元素之下插入一个新的子元素或在最后一个子元素之后再插入新的子元素；
- "afterend" ，在当前元素之后插入一个紧邻的同辈元素。

```js
//作为前一个同辈元素插入
element.insertAdjacentHTML("beforebegin", "<p>Hello world!</p>");

//作为第一个子元素插入
element.insertAdjacentHTML("afterbegin", "<p>Hello world!</p>");

//作为最后一个子元素插入
element.insertAdjacentHTML("beforeend", "<p>Hello world!</p>");

//作为后一个同辈元素插入
element.insertAdjacentHTML("afterend", "<p>Hello world!</p>");
```

4. 内存与性能问题

在使用前述某个属性将该元素从文档树中删除后，元素与**事件**处理程序（或JavaScript对象）之间的绑定关系在内存中**并没有一并删除。**

因此，在使用innerHTML 、outerHTML 属性和insertAdjacentHTML() 方法时，**最好**先手工删除要被替换的元素的所有事件处理程序和JavaScript对象属性（第13章将进一步讨论事件处理程序）。

不过，使用这几个属性——特别是使用innerHTML ，仍然还是可以为我们提供很多便利的。一般来说，在**插入大量新HTML标记**时，使用innerHTML 属性与通过多次DOM操作先创建节点再指定它们之间的关系相比，**效率要高得多**。这是因为在设置innerHTML 或outerHTML 时，就会**创建一个HTML解析器**。这个解析器是在浏览器级别的代码（通常是C++编写的）基础上运行的，因此比执行JavaScript快得多。不可避免地，**创建和销毁HTML解析器也会带来性能损失**，所以最好能够将设置innerHTML 或outerHTML 的次数控制在合理的范围内。

#### 11.3.7　scrollIntoView() 方法

scrollIntoView() 可以在所有HTML元素上调用，通过滚动浏览器窗口或某个容器元素，调用元素就可以出现在视口中。

```
//让元素可见
document.forms[0].scrollIntoView();
```

### 11.4 专有扩展

在发现某项功能缺失时，这些开发商都会一如既往地向DOM中添加**自己的**扩展，以弥补功能上的不足。

#### 11.4.1　文档模式

页面的文档模式决定了可以使用什么功能。

#### 11.4.2 children 属性

只包含元素中同样还是**元素**的子节点。

#### 11.4.3　contains()方法

调用contains() 方法的应该是祖先节点，也就是搜索开始的节点，这个方法接收一个参数，即要检测的后代节点。

```js
alert(document.documentElement.contains(document.body));    //true

var result = document.documentElement.compareDocumentPosition(document.body);
alert(!!(result & 16));
```

#### 11.4.4　插入文本

1. innerText 属性

通过innertText 属性可以操作元素中包含的所有文本内容，包括子文档树中的文本。

2. outerText 属性

除了作用范围扩大到了包含调用它的节点之外，outerText 与innerText 基本上没有多大区别。

#### 11.4.5　滚动

- scrollIntoViewIfNeeded(alignCenter ) ：只在当前元素在视口中不可见的情况下，才滚动浏览器窗口或容器元素，最终让它可见。如果当前元素在视口中可见，这个方法什么也不做。如果将可选的 alignCenter 参数设置为true ，则表示尽量将元素显示在视口中部（垂直方向）。Safari和Chrome实现了这个方法。
- scrollByLines(lineCount ) ：将元素的内容滚动指定的行高， lineCount 值可以是正值，也可以是负值。Safari和Chrome实现了这个方法。
- scrollByPages(pageCount ) ：将元素的内容滚动指定的页面高度，具体高度由元素的高度决定。Safari和Chrome实现了这个方法。

## 第 12 章　DOM2和DOM3

- DOM1级：主要定义的是HTML和XML文档的底层结构。
- DOM2级
    - 核心：在1级核心基础上构建，为节点添加了更多方法和属性。
    - 视图：为文档定义了基于样式信息的不同视图。
    - 事件：说明了如何使用事件与DOM文档交互。
    - 样式：定义了如何以编程方式来访问和改变CSS样式信息。
    - 遍历和范围：引入了遍历DOM文档和选择其特定部分的新接口。
    - HTML：在1级HTML基础上构建，添加了更多属性、方法和新接口。
- DOM3级：增加了“XPath”模块和“加载与保存”（Load and Save）模块。

### 12.1　DOM变化

#### 12.1.1　针对XML命名空间的变化

### 12.2　样式

#### 12.2.1　访问元素的样式

任何支持style 特性的HTML元素在JavaScript中都有一个对应的style 属性。

```js
var myDiv = document.getElementById("myDiv");

//设置背景颜色
myDiv.style.backgroundColor = "red";        // 短线变驼峰

//改变大小
myDiv.style.width = "100px";
myDiv.style.height = "200px";

//指定边框
myDiv.style.border = "1px solid black";

```

1. DOM样式属性和方法

“DOM2级样式”规范还为style 对象定义了一些属性和方法。这些属性和方法在提供元素的style 特性值的同时，也可以修改样式。下面列出了这些属性和方法。

- cssText ：如前所述，通过它能够访问到style 特性中的CSS代码。
- length ：应用给元素的CSS属性的数量。
- parentRule ：表示CSS信息的CSSRule 对象。本节后面将讨论CSSRule 类型。
- getPropertyCSSValue(propertyName ) ：返回包含给定属性值的CSSValue 对象。
- getPropertyPriority(propertyName ) ：如果给定的属性使用了!important 设置，则返回"important" ；否则，返回空字符串。
- getPropertyValue(propertyName ) ：返回给定属性的字符串值。
- item(index ) ：返回给定位置的CSS属性的名称。
- removeProperty(propertyName ) ：从样式中删除给定属性。
- setProperty(propertyName,value,priority ) ：将给定属性设置为相应的值，并加上优先权标志（"important" 或者一个空字符串）。

```js
myDiv.style.cssText = "width: 25px; height: 100px; background-color: green";        // 在写入模式下，赋给cssText 的值会重写整个style 特性的值
alert(myDiv.style.cssText);

for (var i=0, len=myDiv.style.length; i < len; i++){        // length 以便迭代在元素中定义的CSS属性。
    prop = myDiv.style[i];    //或者 myDiv.style.item(i)

    value = myDiv.style.getPropertyValue(prop);

    alert(prop + " : " + value);
}
```

#### 12.2.2　操作样式表

CSSStyleSheet 继承自StyleSheet ，后者可以作为一个基础接口来定义非CSS样式表。从StyleSheet 接口继承而来的属性如下。

- disabled ：表示样式表是否被禁用的布尔值。这个属性是可读/写的，将这个值设置为true 可以禁用样式表。
- href ：如果样式表是通过`<link>` 包含的，则是样式表的URL；否则，是null 。
- media ：当前样式表支持的所有媒体类型的集合。与所有DOM集合一样，这个集合也有一个length 属性和一个item() 方法。也可以使用方括号语法取得集合中特定的项。如果集合是空列表，表示样式表适用于所有媒体。在IE中，media 是一个反映`<link>` 和`<style>` 元素media 特性值的字符串。
- ownerNode ：指向拥有当前样式表的节点的指针，样式表可能是在HTML中通过`<link>` 或`<style/>` 引入的（在XML中可能是通过处理指令引入的）。如果当前样式表是其他样式表通过@import 导入的，则这个属性值为null 。IE不支持这个属性。
- parentStyleSheet ：在当前样式表是通过@import 导入的情况下，这个属性是一个指向导入它的样式表的指针。
- title ：ownerNode 中title 属性的值。
- type ：表示样式表类型的字符串。对CSS样式表而言，这个字符串是"type/css" 。

#### 12.2.3　元素大小

1. 偏移量
2. 客户区大小
3. 滚动大小
4. 确定元素大小

### 12.3 遍历

### 12.4　范围

## 第 13 章　事件

### 13.1 事件流

事件流 描述的是从页面中接收事件的**顺序**。

IE的事件流是事件**冒泡**流，而Netscape Communicator的事件流是事件**捕获**流。（完全相反）

#### 13.1.1 事件冒泡

从最底下往上触发事件。

#### 13.1.2 事件捕获

从最上面往下触发事件。

#### 13.1.3 DOM 事件流

### 13.2 事件处理程序

#### 13.2.1 HTML 事件处理程序

```html
<!-- 首先，这样会创建一个封装着元素属性值的函数。这个函数中有一个局部变量event ，也就是事件对象 -->
<!-- 输出 "click" -->
<input type="button" value="Click Me" onclick="alert(event.type)">

<!-- this 值等于事件的目标元素 -->
<!-- 输出 "Click Me" -->
<input type="button" value="Click Me" onclick="alert(this.value)">

```

#### 13.2.2　DOM0级事件处理程序

通过JavaScript指定事件处理程序的传统方式，就是将一个函数赋值给一个事件处理程序属性。
```js
var btn = document.getElementById("myBtn");
btn.onclick = function(){
    alert("Clicked");
};

btn.onclick = null;     //删除事件处理程序
```

#### 13.2.3　DOM2级事件处理程序

定义了两个方法，用于处理指定和删除事件处理程序的操作：addEventListener() 和removeEventListener() 。

```js
var btn = document.getElementById("myBtn");
var handler = function(){
    alert(this.id);
};

// 两个函数都接受3个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值。
// 最后这个布尔值参数如果是true ，表示在捕获阶段调用事件处理程序；如果是false ，表示在冒泡阶段调用事件处理程序。
btn.addEventListener("click", handler, false);

// 这里为按钮添加了两个事件处理程序。
// 这两个事件处理程序会按照添加它们的顺序触发，因此首先会显示元素的ID，其次会显示"Hello world!" 消息。
btn.addEventListener("click", function(){
    alert("Hello world!");
}, false);

btn.removeEventListener("click", handler, false); //指向同一个函数，删除有效！
```

#### 13.2.4　IE事件处理程序

IE实现了与DOM中类似的两个方法：attachEvent() 和detachEvent() 。
```js
// 这两个方法接受相同的两个参数：事件处理程序名称与事件处理程序函数。
var btn = document.getElementById("myBtn");
btn.attachEvent("onclick", function(){
    alert("Clicked");
});
```

#### 13.2.5　跨浏览器的事件处理程序

在触发DOM上的某个事件时，会产生一个事件对象event ，这个对象中包含着所有与事件有关的信息。

### 13.3 事件对象

#### 13.3.1 DOM 中的事件对象

属性/方法 | 类　　型 | 读/写 | 说　　明
--- | --- | --- | --- 
bubbles | Boolean | 只读 | 表明事件是否冒泡
cancelable | Boolean | 只读| 表明是否可以取消事件的默认行为
currentTarget | Element | 只读| 其事件处理程序当前正在处理事件的那个元素
defaultPrevented | Boolean | 只读| 为true 表示已经调用了preventDefault() （DOM3级事件中新增）
detail | Integer | 只读| 与事件相关的细节信息
eventPhase | Integer | 只读| 调用事件处理程序的阶段：1表示捕获阶段，2表示“处于目标”，3表示冒泡阶段
preventDefault() | Function | 只读| 取消事件的默认行为。如果cancelable 是true ，则可以使用这个方法
stopImmediatePropagation() | Function | 只读| 取消事件的进一步捕获或冒泡，同时阻止任何事件处理程序被调用（DOM3级事件中新增）
stopPropagation() | Function | 只读| 取消事件的进一步捕获或冒泡。如果bubbles 为true ，则可以使用这个方法
target | Element | 只读| 事件的目标
trusted | Boolean | 只读| 为true 表示事件是浏览器生成的。为false 表示事件是由开发人员通过JavaScript创建的（DOM3级事件中新增）
type | String | 只读| 被触发的事件的类型
view | AbstractView | 只读| 与事件关联的抽象视图。等同于发生事件的window 对象

#### 13.3.2 IE中的事件对象

#### 13.3.3　跨浏览器的事件对象

### 13.4 事件类型

“DOM3级事件”规定了以下几类事件：
1. UI（User Interface，用户界面）事件，当用户与页面上的元素交互时触发；
   - DOMActivate ：表示元素已经被用户操作（通过鼠标或键盘）激活。这个事件在DOM3级事件中被废弃，但Firefox 2+和Chrome支持它。考虑到不同浏览器实现的差异，不建议使用这个事件。
   - load ：当页面完全加载后在window 上面触发，当所有框架都加载完毕时在框架集上面触发，当图像加载完毕时在`<img>` 元素上面触发，或者当嵌入的内容加载完毕时在`<object>` 元素上面触发。
   - unload ：当页面完全卸载后在window 上面触发，当所有框架都卸载后在框架集上面触发，或者当嵌入的内容卸载完毕后在`<object>` 元素上面触发。
   - abort ：在用户停止下载过程时，如果嵌入的内容没有加载完，则在`<object>` 元素上面触发。
   - error ：当发生JavaScript错误时在window 上面触发，当无法加载图像时在`<img>` 元素上面触发，当无法加载嵌入内容时在`<object>` 元素上面触发，或者当有一或多个框架无法加载时在框架集上面触发。第17章将继续讨论这个事件。
   - select ：当用户选择文本框（`<input>` 或`<texterea>` ）中的一或多个字符时触发。第14章将继续讨论这个事件。
   - resize ：当窗口或框架的大小变化时在window 或框架上面触发。
   - scroll ：当用户滚动带滚动条的元素中的内容时，在该元素上面触发。`<body>` 元素中包含所加载页面的滚动条。
2. 焦点事件，当元素获得或失去焦点时触发；
   - blur ：在元素失去焦点时触发。这个事件不会冒泡；所有浏览器都支持它。
   - DOMFocusIn ：在元素获得焦点时触发。这个事件与HTML事件focus 等价，但它冒泡。只有Opera支持这个事件。DOM3级事件废弃了DOMFocusIn ，选择了focusin 。
   - DOMFocusOut ：在元素失去焦点时触发。这个事件是HTML事件blur 的通用版本。只有Opera支持这个事件。DOM3级事件废弃了DOMFocusOut ，选择了focusout 。
   - focus ：在元素获得焦点时触发。这个事件不会冒泡；所有浏览器都支持它。
   - focusin ：在元素获得焦点时触发。这个事件与HTML事件focus 等价，但它冒泡。支持这个事件的浏览器有IE5.5+、Safari 5.1+、Opera 11.5+和Chrome。
   - focusout ：在元素失去焦点时触发。这个事件是HTML事件blur 的通用版本。支持这个事件的浏览器有IE5.5+、Safari 5.1+、Opera 11.5+和Chrome。
3. 鼠标事件，当用户通过鼠标在页面上执行操作时触发；
   - click ：在用户单击主鼠标按钮（一般是左边的按钮）或者按下回车键时触发。这一点对确保易访问性很重要，意味着onclick 事件处理程序既可以通过键盘也可以通过鼠标执行。
   - dblclick ：在用户双击主鼠标按钮（一般是左边的按钮）时触发。从技术上说，这个事件并不是DOM2级事件规范中规定的，但鉴于它得到了广泛支持，所以DOM3级事件将其纳入了标准。
   - mousedown ：在用户按下了任意鼠标按钮时触发。不能通过键盘触发这个事件。
   - mouseenter ：在鼠标光标从元素外部首次移动到元素范围之内时触发。这个事件不冒泡，而且在光标移动到后代元素上不会触发。DOM2级事件并没有定义这个事件，但DOM3级事件将它纳入了规范。IE、Firefox 9+和Opera支持这个事件。
   - mouseleave ：在位于元素上方的鼠标光标移动到元素范围之外时触发。这个事件不冒泡，而且在光标移动到后代元素上不会触发。DOM2级事件并没有定义这个事件，但DOM3级事件将它纳入了规范。IE、Firefox 9+和Opera支持这个事件。
   - mousemove ：当鼠标指针在元素内部移动时重复地触发。不能通过键盘触发这个事件。
   - mouseout ：在鼠标指针位于一个元素上方，然后用户将其移入另一个元素时触发。又移入的另一个元素可能位于前一个元素的外部，也可能是这个元素的子元素。不能通过键盘触发这个事件。
   - mouseover ：在鼠标指针位于一个元素外部，然后用户将其首次移入另一个元素边界之内时触发。不能通过键盘触发这个事件。
   - mouseup ：在用户释放鼠标按钮时触发。不能通过键盘触发这个事件。
4. 滚轮事件，当使用鼠标滚轮（或类似设备）时触发；
5. 文本事件，当在文档中输入文本时触发；
    - textInput 。这个事件是对keypress 的补充，用意是在将文本显示给用户之前更容易拦截文本。在文本插入文本框之前会触发textInput 事件。
6. 键盘事件，当用户通过键盘在页面上执行操作时触发；
   - keydown ：当用户按下键盘上的任意键时触发，而且如果按住不放的话，会重复触发此事件。
   - keypress ：当用户按下键盘上的字符键时触发，而且如果按住不放的话，会重复触发此事件。按下Esc键也会触发这个事件。Safari 3.1之前的版本也会在用户按下非字符键时触发keypress 事件。
   - keyup ：当用户释放键盘上的键时触发。
7. 合成事件，当为IME（Input Method Editor，输入法编辑器）输入字符时触发；
   - compositionstart ：在IME的文本复合系统打开时触发，表示要开始输入了。
   - compositionupdate ：在向输入字段中插入新字符时触发。
   - compositionend ：在IME的文本复合系统关闭时触发，表示返回正常键盘输入状态。
8. 变动（mutation）事件，当底层DOM结构发生变化时触发。
   - DOMSubtreeModified ：在DOM结构中发生任何变化时触发。这个事件在其他任何事件触发后都会触发。
   - DOMNodeInserted ：在一个节点作为子节点被插入到另一个节点中时触发。
   - DOMNodeRemoved ：在节点从其父节点中被移除时触发。
   - DOMNodeInsertedIntoDocument ：在一个节点被直接插入文档或通过子树间接插入文档之后触发。这个事件在DOMNodeInserted之后触发。
   - DOMNodeRemovedFromDocument ：在一个节点被直接从文档中移除或通过子树间接从文档中移除之前触发。这个事件在DOMNodeRemoved 之后触发。
   - DOMAttrModified ：在特性被修改之后触发。
   - DOMCharacterDataModified ：在文本节点的值发生变化时触发。
9.  变动名称事件，当元素或属性名变动时触发。此类事件已经被废弃，没有任何浏览器实现它们，因此本章不做介绍。

### 13.5 内存和性能

#### 13.5.1 事件委托

我们可以为整个页面指定一个onclick 事件处理程序，而不必给每个可单击的元素分别添加事件处理程序。
```js
// <ul id="myLinks">
//     <li id="goSomewhere">Go somewhere</li>
//     <li id="doSomething">Do something</li>
//     <li id="sayHi">Say hi</li>
// </ul>
let list = document.getElementById("myLinks");

EventUtil.addHandler(list, "click", function(event) {
    event = EventUitl.getEvent(event);

    let target = EventUtil.getTarget(event);        // 代理的关键，拿到 event.target 就是点击的对象，根据不同的 id 分别处理不同的事件。

    switch(target.id) {
        case "doSomething":
            document.title = "I changed the document's title";
            break;
        case "goSomewhere":
            location.href = "http://www.wrox.com";
            break;
        case "sayHi":
            alert("hi");
            break;
    }
});
```

#### 13.5.2 移除事件处理程序

```js
btn.onclick = null;    //移除事件处理程序
```

### 13.6　模拟事件

#### 13.6.1　DOM中的事件模拟

可以在document 对象上使用createEvent() 方法创建event 对象。这个方法接收一个参数，即表示要创建的事件类型的字符串。
- UIEvents ：一般化的UI事件。鼠标事件和键盘事件都继承自UI事件。DOM3级中是UIEvent 。
- MouseEvents ：一般化的鼠标事件。DOM3级中是MouseEvent 。
- MutationEvents ：一般化的DOM变动事件。DOM3级中是MutationEvent 。
- HTMLEvents ：一般化的HTML事件。没有对应的DOM3级事件（HTML事件被分散到其他类别中）。

```js
// 模拟鼠标事件为例

var btn = document.getElementById("myBtn");

//创建事件对象
var event = document.createEvent("MouseEvents");

//初始化事件对象
event.initMouseEvent("click", true, true, document.defaultView, 0, 0, 0, 0, 0, false, false, false, false, 0, null);

//触发事件
btn.dispatchEvent(event);
```

#### 13.6.2 IE 中的事件模拟

## 第 14 章 表单脚本

### 14.1　表单的基础知识

#### 14.1.1　提交表单

```html
<!-- 通用提交按钮 -->
<input type="submit" value="Submit Form">

<!-- 自定义提交按钮 -->
<button type="submit">Submit Form</button>

<!-- 图像按钮 -->
<input type="image" src="graphic.gif">
```

以这种方式提交表单时，浏览器会在将请求发送给服务器之前触发**submit 事件**。这样，我们就有机会验证表单数据，并据以决定是否允许表单提交。

#### 14.1.2　重置表单
```js
<!-- 通用重置按钮 -->
<input type="reset" value="Reset Form">

<!-- 自定义重置按钮 -->
<button type="reset">Reset Form</button>
```

用户单击重置按钮重置表单时，会触发**reset 事件**。利用这个机会，我们可以在必要时取消重置操作。

#### 14.1.3　表单字段

```js
var form = document.getElementById("form1");

//取得表单中的第一个字段
var field1 = form.elements[0];

//取得名为"textbox1"的字段
var field2 = form.elements["textbox1"];

//取得表单中包含的字段的数量
var fieldCount = form.elements.length;
```

### 14.2　文本框脚本

```html
<input type="text" size="25" maxlength="50" value="initial value">

<textarea rows="25" cols="5">initial value</textarea>
```

#### 14.2.1　选择文本

```js
var textbox = document.forms[0].elements["textbox1"];
textbox.select();       // 用于选择文本框中的所有文本。

extbox.value = "Hello world!"

//选择所有文本
textbox.setSelectionRange(0, textbox.value.length);  //"Hello world!"
//选择前3个字符
textbox.setSelectionRange(0, 3);  //"Hel"
//选择第4到第6个字符
textbox.setSelectionRange(4, 7);  //"o w"
```

1. 选择（select ）事件
2. 取得选择的文本
3. 选择部分文本

#### 14.2.2　过滤输入

1. 屏蔽字符
2. 操作剪贴板

#### 14.2.3　自动切换焦点

#### 14.2.4　HTML5约束验证API

1. 必填字段（required 属性
2. 其他输入类型（"email" 和"url" 是两个得到支持最多的类型
3. 数值范围（min 属性（最小的可能值）、max 属性（最大的可能值）和step 属性（从min 到max 的两个刻度间的差值）。
4. 输入模式（pattern 属性
5. 检测有效性

### 14.3　选择框脚本

### 14.4　表单序列化

### 14.5　富文本编辑

这一技术的本质，就是在页面中嵌入一个包含空HTML页面的iframe 。

## 第 15 章 使用 Canvas 绘图

### 15.1 基本用法

```js
/**
<canvas d="drawing" width=" 200" height="200">
</canvas>
**/

var drawing = document.getElementById("drawing");

//确定浏览器支持<canvas>元素
if (drawing.getContext){
    var context = drawing.getContext("2d");
    // ...

    //取得图像的数据 URI
    var imgURI = drawing.toDataURL("image/png");
    //显示图像
    var image = document.createElement("img");
    image.src = imgURI;
    document.body.appendChild(image);
}
```

### 15.2 2D 上下文

坐标开始于`<canvas>` 元素的左上角，原点坐标是(0,0)。x 值越大表示越靠右，y 值越大表示越靠下。

#### 15.2.1　填充和描边

- fillStyle = "red";
- strokeStyle = "#0000ff";

#### 15.2.2 绘制矩形

- fillRect(x, y, width, height)
- strokeRect(x, y, width, height)
- clearRect(x, y, width, height) 

#### 15.2.3 绘制路径

要绘制路径，首先必须调用`beginPath()`方法，表示要开始绘制新路径。然后，再通过调用下列方法来实际地绘制路径。
- `arc(x, y, radius, startAngle, endAngle, counterclockwise)` ：以(x,y ) 为圆心绘制一条弧线，弧线半径为radius ，起始和结束角度（用弧度表示）分别为startAngle 和endAngle 。最后一个参数表示startAngle 和endAngle 是否按逆时针方向计算，值为false 表示按顺时针方向计算。
- `arcTo(x1, y1, x2, y2, radius)` ：从上一点开始绘制一条弧线，到(x2,y2 ) 为止，并且以给定的半径radius 穿过(x1,y1 ) 。
- `bezierCurveTo(c1x, c1y, c2x, c2y, x, y)` ：从上一点开始绘制一条曲线，到(x,y ) 为止，并且以(c1x,c1y ) 和(c2x,c2y ) 为控制点。
- `lineTo(x, y)` ：从上一点开始绘制一条直线，到(x,y) 为止。
- `moveTo(x, y)` ：将绘图游标移动到(x,y ) ，不画线。
- `quadraticCurveTo(cx, cy, x, y)` ：从上一点开始绘制一条二次曲线，到(x,y) 为止，并且以(cx,cy ) 作为控制点。
- `rect(x, y, width, height)` ：从点(x,y ) 开始绘制一个矩形，宽度和高度分别由width 和height 指定。这个方法绘制的是矩形路径，而不是strokeRect() 和fillRect() 所绘制的独立的形状。

#### 15.2.4 绘制文本

- fillText()
- strokeText() 

这两个方法都可以接收4个参数：要绘制的文本字符串、x 坐标、y 坐标和可选的最大像素宽度。

而且，这两个方法都以下列3个属性为基础：
- font ：表示文本样式、大小及字体，用CSS中指定字体的格式来指定，例如"10px Arial" 。
- textAlign ：表示文本对齐方式。可能的值有"start" 、"end" 、"left" 、"right" 和"center" 。建议使用"start" 和"end" ，不要使用"left" 和"right" ，因为前两者的意思更稳妥，能同时适合从左到右和从右到左显示（阅读）的语言。
- textBaseline ：表示文本的基线。可能的值有"top" 、"hanging" 、"middle" 、"alphabetic" 、"ideographic" 和"bottom" 。

#### 15.2.5　变换

可以通过如下方法来修改变换矩阵：
- rotate (angle ) ：围绕原点旋转图像 angle 弧度。
- scale (scaleX, scaleY ) ：缩放图像，在x 方向乘以 scaleX ，在y 方向乘以 scaleY 。 scaleX 和 scaleY 的默认值都是1.0。
- translate (x, y ) ：将坐标原点移动到(x,y ) 。执行这个变换之后，坐标(0,0)会变成之前由(x,y )表示的点。
- transform (m1_1, m1_2, m2_1, m2_2, dx, dy ) ：直接修改变换矩阵，方式是乘以如下 矩阵。
- setTransform(m1_1, m1_2, m2_1, m2_2, dx, dy ) ：将变换矩阵重置为默认状态，然后再调用 transform () 。

#### 15.2.6　绘制图像

- drawImage()

#### 15.2.7　阴影

2D上下文会根据以下几个属性的值，自动为形状或路径绘制出阴影：

- shadowColor ：用CSS颜色格式表示的阴影颜色，默认为黑色。
- shadowOffsetX ：形状或路径x 轴方向的阴影偏移量，默认为0。
- shadowOffsetY ：形状或路径y 轴方向的阴影偏移量，默认为0。
- shadowBlur ：模糊的像素数，默认0，即不模糊。

这些属性都可以通过context 对象来修改。只要在绘制前为它们设置适当的值，就能自动产生阴影。

#### 15.2.8　渐变

渐变由`CanvasGradient`实例表示，很容易通过2D上下文来创建和修改。

要创建一个新的线性渐变，可以调用`createLinearGradient()`方法。这个方法接收4个参数：起点的x 坐标、起点的y 坐标、终点的x 坐标、终点的y 坐标。

创建了渐变对象后，下一步就是使用addColorStop() 方法来指定色标。这个方法接收两个参数：色标位置和CSS颜色值。色标位置是一个0（开始的颜色）到1（结束的颜色）之间的数字。

```js
var gradient = context.createLinearGradient(30, 30, 70, 70);

gradient.addColorStop(0, "white");
gradient.addColorStop(1, "black");

//绘制红色矩形
context.fillStyle = "#ff0000";
context.fillRect(10, 10, 50, 50);

//绘制渐变矩形
context.fillStyle = gradient;
```

#### 15.2.9　模式

模式其实就是重复的图像，可以用来填充或描边图形。

要创建一个新模式，可以调用createPattern() 方法并传入两个参数：一个HTML `<img>` 元素和一个表示如何重复图像的字符串。其中，第二个参数的值与CSS的background-repeat 属性值相同，包括"repeat" 、"repeat-x" 、"repeat-y" 和"no-repeat" 。

#### 15.2.10　使用图像数据

2D上下文的一个明显的长处就是，可以通过`getImageData()`取得原始图像数据。这个方法接收4个参数：要取得其数据的画面区域的x 和y 坐标以及该区域的像素宽度和高度。

```js
var imageData = context.getImageData(10, 5, 50, 50);    // 对象都有三个属性：width 、height 和data 。

var data = imageData.data,  // 其中data 属性是一个数组,保存着图像中每一个像素的数据。
// 分别表示红、绿、蓝和透明度值。
red = data[0],
green = data[1],
blue = data[2],
alpha = data[3];

```

#### 15.2.11 合成

还有两个会应用到2D上下文中所有绘制操作的属性：`globalAlpha`和`globalCompositionOperation` 。

### 15.3 WebGL

#### 15.3.1 类型化数组

WebGL涉及的复杂计算需要提前知道数值的精度，而标准的JavaScript数值无法满足需要。

类型化数组也是数组，只不过其元素被设置为**特定类型的值**。

```js
// 会在内存中分配20B。
var buffer = new ArrayBuffer(20);
var bytes = buffer.byteLength;
```

1. 视图
2. 类型化视图

#### 15.3.2 WebGL 上下文

1. 常量
2. 方法命名
3. 准备绘图
4. 视口与坐标
5. 缓冲区
6. 错误
7. 着色器
8. 编写着色器
9. 编写着色器程序
10. 为着色器传入值
11. 调试着色器和程序
12. 绘图
13. 纹理
14. 读取像素
    
#### 15.3.3 支持