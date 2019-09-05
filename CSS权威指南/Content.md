## 第 1 章 CSS和文档

### 1.1 Web的衰落

### 1.2 CSS作救星

1. 丰富的样式
2. 易于使用
3. 层叠
4. 缩减文件大小
5. 为将来做准备

### 1.3 元素

#### 1.3.1 替换和非替换元素

- 替换元素：是指用来替换元素内容的部分并非由文档内容直接表示。（img 由文档本身之外的一个图像文件来替换。、input 根据 type）
- 非替换元素：大部分。

#### 1.3.2 元素显示角色

块级（block-level）元素和行内（inline-level）元素

### 1.4 结合CSS和XHTML

`<link rel="stylesheet" type="text/css" href="xxx.css" media="all" />`

#### 1.4.1 属性

- rel代表“关系”（relation），在这里，关系为stylesheet。
- media属性。这里使用的值是all，说明这个样式表要应用于所有表现媒体。all、aural、braille、embossed、handheld、print、projection、screen、tty、tv

#### 1.4.2 候选样式表

`rel="alternate stylesheet"` 然后可以通过浏览器 view->use style 选择其他 title 的 css 。

#### 1.4.3 @import 指令

```html
<style type="text/css">
    @import url(sheet2.css) all;

    @import url(http://example.org/library/layout.css);
    
    h1 {
        color: gray;
    }
</style>
```

## 第 2 章 选择器

### 2.1 基本规则

#### 2.1.1 规则结构

#### 2.1.2 元素选择器

```css
p {color:gray;} 
```

### 2.2 分组

#### 2.2.1 选择器分组*

```css
h2, p {color:gray;}  /* 逗号分隔 */
```

#### 2.2.2 通配选择器

```css
* {color: red;}
```

#### 2.2.3 声明分组*

```css
/*没什么意义，只是基本语法，对标选择器分组
h1 {font: 18px Helvetica;}
h1 {color: purple;}
h1 {background: aqua;}
*/

h1 {
    font: 18px Helvetica; 
    color: purple; 
    background: aqua;
}
```

#### 2.2.4 结合选择器和声明的分组

### 2.3 类选择器和ID选择器

#### 2.3.1 类选择器*

```html
<body>
    <p class="warning">When handling plutonium,care must be taken to avoid the formation of a critical mass.</p>
    <p>With plutonium,<span class="warning">the possibility of implosion is very real, and must be</span></p>
</body>
```

```css
/*
可以结合一个简单选择器
通过这个点号，可以帮助 类选择器 与 它可能结合的其他部分 分隔开
（如类选择器可能结合了元素选择器）。 
*/

*.warning {
}

p.warning {                  /* 点号 */     /* 整个段落是警告（首先p元素再是这个元素的 class 为 warning）时才显示为粗体文本 整个第一段*/
    font-weight: bold;  
}

p .warning {    /*中间隔了空行---后代选择器*/    /*在p元素中的子元素 class 为 warning 的时候 即第二段 warning 部分*/
}
```

#### 2.3.2 多类选择器

```css
/*
需求：
class为warning的所有元素都是粗体，
而class为urgent的所有元素为斜体，
class中同时包含warning和urgent的所有元素还有一个银色的背景。
*/
.warning {font-weight: bold;}

.urgent {font-style: italic;}

.warning.urgent {background: silver;}   /*通过把两个类选择器链接在一起，仅可以选择*同时*（既是又是）包含这些类名的元素（类名的顺序不限）。*/
```

#### 2.3.3 ID 选择器*

```css
*#first-para {font-weight: bold;}

/*与类选择器一样，ID选择器中可以忽略通配选择器。*/
#lead-para {font-weighc:bold;}
```

#### 2.3.4 类选择器还是ID选择器？

实际中，浏览器通常并不检查HTML中ID的唯一性，这意味着如果你在HTML文档中设罝了多个有相同ID属性值的元素，就可能为这些元素应用相同的样式。（不推荐）

不同于类选择器，ID选择器**不能结合使用**，因为ID属性不允许有以空格分隔的词列表（class="warning urgent"）。

### 2.4 属性选择器*

#### 2.4.1 简单属性选择

希望选择有某个属性的元素，而不论该属性的值是什么。

```css
h1[class] {         /* 拥有 class 属性的 h1 ，不论 clas 的属性就匹配*/
    color: silver;
}

a[href][title] {    /* 将同时有 href 和 title 属性的 超链接 的文本置为粗体 */
    font-weight: bold;
}
```

#### 2.4.2 根据具体属性值选择

只选择有特定属性值的元素。

```css
/* <planet type="barren rocky">Mercury</planet> */
planet[type="barren rocky"] {   /* 必须完全字符串匹配 planet[type="barren"]X planet[type="rocky barren"]X  */
    font-weight:bold;
}
```

#### 2.4.3 根据部分属性值选择

如果属性能接受词列表（词之间用空格分隔），可以根据其中的任意一个词进行选择。

```css
/* <p class="urgent warning">test</p> */
p[class~="urgent"] {        /* 这里因为是 class 属性，所以和 .urgent 等价*/
    font-weight:bold;
}

img[title~="Figure"] {
    border: 1px solid gray;
}
```

**CSS3 中新增的子串匹配属性选择器**

类型 | 描述 
--- | ---
[foo^="bar"] | 选择foo属性值以"bar"开头的所有元素 
[foo$="bar"] | 选择foo属性值以"bar"结尾的所有元素 
[foo~="bar"] | 选择foo属性值中包"bar"的所有元素

#### 2.4.4 特定属性选择类型

```css
*[lang|="en"] {     /* 选择lang属性等于en或者以en-开头的所有元素 *//* 感觉和 [lang^="en"] 类似*/
    color:white;
}
```

### 2.5 后代选择器*

```css
h1 strong {             /* 把作为 h1 元素后代的 strong 元素的文本变成灰色。 */ /* 无论嵌套多少层 */
    color: gray;
}
```

#### 2.5.1 选择子元素*

在某些情况下，可能并不想选择一个任意的后代元素，而是希望缩小范围，只选择另一个元素的子元素。

```css
/*可以使用子结合符，即大于号（>）*/
h1>strong {        /* h1 下的所有 strong 子元素，孙元素不匹配 */
    color: red;
}

h1 > strong {  /* 子结合符两边可以有空白符，这是可选的。 */ /* 二者等价 */
}
```

#### 2.5.2 选择相邻兄弟元素*

要选择**紧接**在另一个元素后的元素，而且二者有**相同的父元素**。

可以使用相邻兄弟结合符，一个加号（+）。与子结合符一样，相邻兄弟结合符旁边可以有空白符。

用一个结合符只能选择两个相邻兄弟中的**第二个**元素。

```css
h1 + p {
    margin-top: 0;      /* 去除紧接在一个h1元素后出现的段落的上边距 */
}

/*
<ul>
    <li>111</li>
    <li>222</li>
    <li>333</li>
</ul>
*/
li+li {                 /* 如果有 3 个 li，则第一个 li 不影响，第二个第三个 li 影响 */
    font-weight: bold;
}
/* 明明是直接相邻的兄弟节点才受影响，知道为什么第二个第三个 li 都受影响？*/
/* 因为第二个 li 是第一个 li 直接相邻的兄弟元素；第三个 li 也符合选择器呀，是第二个 li 的直接相邻的 兄弟元素。 */
```

### 2.6 伪类和伪元素

#### 2.6.1 伪类选择器*

```css
a:visited {     /* 冒号（：） */
    color: red;
}
```

1. 链接伪类

伪类名 | 描述 
--- | ---
:link | 指示作为超链接（即有一个href属性）并指向一个未访问地址的所有锚。（有点多余）
:visited | 指示作为已访问地址超链接的所有锚

2. 动态伪类

伪类名 | 描述 
--- | ---
:focus | 指示当前拥有输入焦点的元素，也就是说，可以接受键盘输入或者能以某种方式激活的元素 
:hover | 指示鼠标指针停留在哪个元素上，例如，鼠标指针可能停留在一个超链接上，:hover就会指示这个超链接 
:active | 指示被用户输入激活的元素，例如，鼠标指针停留在一个超链接上时，如果用户点击鼠标，就会激活这个超链接，将指示这个超链接

3. 选择第一个子元素

使用另一个静态伪类：`first-child`来选择元素的第一个**子元素**。

```css
li:first-child {
    text-transform: uppercase;
}
```

4. 根据语言选择

有些情况下，你可能想根据元素的语言来选择，此时可以使用：lang()伪类。从对应的模式来讲，:lang()伪类就像是|=属性选择器。

```css
*:lang(fr) {        /* 把所有法语元素变成斜体 */
    font-style: italic;
}
```

5. 结合伪类

```css
a:visited:hover {   /* 鼠标指针停留在已访问链接上时，链接变成紫红色。 */ /* 不要把互斥的伪类结合在一起使用。 */
    color: maroon;
}
```

#### 2.6.5 伪元素选择器*

CSS2.1中定义了 4 个伪元素：设置首字母样式、设置第一行样式、设置之前和之后元素的样式。

所有伪元素都必须放在出现该伪元素的选择器的最后面。伪元素还有不被允许的属性。



1. 设置首字母样式

```css
p:first-letter {    /* 应用块级元素，不能应用行内元素 */
    color: red;
}
```

2. 设置第一行的样式

```css
p:first-line {      /* 应用块级元素，不能应用行内元素 */
    color: purple;
}
```

3. 设置之前和之后元素的样式

```css
body:after {
    content:" The End.";
}
```

## 第 3 章 结构和层叠

本章将讨论这 3 种机制之间的关联：特殊性、继承和层叠。

### 3.1 特殊性

选择器的特殊性由选择器本身的组件确定。特殊性值表述为4个部分，如：0，0，0，0。一个选择器的具体特殊性如下确定：
- 对于选择器中给定的各个ID属性值，加0，1，0，0。
- 对于选择器中给定的各个类属性值，属性选择或伪类，加0，0，1，0。
- 对于选择器中给定的各个元素和伪元素，加0，0，0，1。
- 结合符和通配符选择器对特殊性没有任何贡献0。

标志为 !important 的声明并没有特殊的特殊性值，不过要与非重要声明分开考虑。

### 3.2 继承

继承值完全没有特殊性（比通配符还弱）。

暂时只看到 color 可以继承。

一般地，大多数框模型属性（包括外边距、内边距、背景和边框）都不能继承。

### 3.3 层叠

如果特殊性相等的两个规则同时应用到同一个元素会怎么样呢？浏览器如何解决这个冲突？

1. 按权重和来源排序
2. 按特殊性排序
3. 按顺序排序

LVHA --- “LoVe-HA!” link、visited、hover、active

## 第 4 章 值和单位

### 4.1 数字

### 4.2 百分数

### 4.3 颜色

- 命名颜色（17个颜色名）
- 用RGB指定颜色
- 十六进制RGB颜色

### 4.4 长度单位

#### 4.4.1 绝对长度单位

1. in 英寸
2. cm 厘米  （1英寸是2.54厘米）
3. mm 毫米
4. pt 点    （1英寸是72点）
5. pc 派卡  （6派卡等于1英寸）

#### 4.4.2 相对长度单位

1. em 
2. ex
3. px

1个“em”定义为一种给定字体的font-size 值。如果一个元素的font-size为14像素，那么对于该元素，1em就等于14像素。另一方面，在设置字体的大小时，em的值会相对于父元素的字体大小改变（`small {font-size: 0.5em;}`）。

与此不同，ex是指所用字体中小写x的高度。因此，如果有两个段落，其中文本的大小为24点，但是各段使用了不同的字体，那么各段相应的ex值可能不同。

#### 4.4.3 em和ex的实际问题

**取em的值，再取其一半**作为ex值。（因为计算困难，所以可以假设1ex=0.5em）

### 4.5 关键字

none、inherit

### 4.6 CSS2 单位

- 角度值：度（deg）、梯度（grad）和弧度（rad
- 时间值：毫秒（ms），秒（s）
- 频率值：赫兹（Hz）或兆赫（MHz）