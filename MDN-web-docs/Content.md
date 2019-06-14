# [MDN Web docs](https://developer.mozilla.org/zh-CN/docs/Learn)

## 第一章 CSS 介绍

### 1.1 如何将你的 CSS 应用到你的 HTML 上

#### 1.1.1 外部样式表

外部样式表是指：当你将你的 CSS 保存在一个独立的扩展名为 .css 的文件中，并从HTML的 `<link>` 元素中引用它。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My CSS experiment</title>
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
    <h1>Hello World!</h1>
    <p>This is my first CSS example</p>
  </body>
</html>
```
以及下面的 CSS 文件
```css
h1 {
  color: blue;
  background-color: yellow;
  border: 1px solid black;
}

p {
  color: red;
}
```

#### 1.1.2 内部样式表

内部样式表是指不使用外部 CSS 文件，而是将你的 CSS 放置在 `<style>` 元素中，该元素包含在 HTML head 内。

它不如外部样式表高效 —— 在网站中，CSS 将需要在每个页面重复，并且需要更新时要更改的多个位置。
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My CSS experiment</title>
    <style>
      h1 {
        color: blue;
        background-color: yellow;
        border: 1px solid black;
      }

      p {
        color: red;
      }
    </style>
  </head>
  <body>
    <h1>Hello World!</h1>
    <p>This is my first CSS example</p>
  </body>
</html>
```

#### 1.1.3 内联样式

内联样式是仅影响一个元素的CSS声明，被 style 属性包括着。

除非有必要，否则不要这么做！这很难维护（你可能不得不在每份文档里更新多次同样的信息），并且它还混合了 CSS 表示的样式信息和 HTML 的结构信息，使 CSS 难以阅读和理解。保持不同类型代码的分离和纯净使处理该代码的任何人工作更为容易。
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My CSS experiment</title>
  </head>
  <body>
    <h1 style="color: blue;background-color: yellow;border: 1px solid black;">Hello World!</h1>
    <p style="color:red;">This is my first CSS example</p>
  </body>
</html>
```

### 1.2 CSS 语法

#### 1.2.1 CSS 声明

![css_syntax_-_declaration](https://raw.githubusercontent.com/514723273/.md-Pictures/master/css_syntax_-_declaration.png)

从最基本的层次来看，CSS是由两块内容组合而成的：
- **属性（Property）**：一些人类可理解的标识符，这些标识符指出你想修改哪一些样式，例如：字体，宽度，背景颜色等。
- **属性值（Value）**：每个指定的属性都需要给定一个值，这个值表示你想把那些样式特征修改成什么样，例如，你想把字体，宽度或背景颜色改成什么。

CSS 有超过[300 个不同的属性](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Reference)以及几乎无穷无尽的属性值。（属性和属性值不能任意组合：每个属性都有一个已经定义好的*可用属性值范围*。）

#### 1.2.2 CSS 声明块

![css syntax - declarations blockn](https://raw.githubusercontent.com/514723273/.md-Pictures/master/css%20syntax%20-%20declarations%20block.png)

声明按块分组，每一组声明都用一对大括号包裹，用 ({) 开始，用 (}) 结束。（声明块里的每一个声明必须用半角分号（;）分隔）

#### 1.2.3 CSS 选择器和规则

![css syntax - ruleset](https://raw.githubusercontent.com/514723273/.md-Pictures/master/css%20syntax%20-%20ruleset.png)

每个*声明块*前加上**选择器（selector）**来告知声明块哪些元素是它们需要应用的。（选择器也可以很复杂，比如光标悬停时候的样式）

一个元素可以被多个选择器所匹配，因此，一个给定的属性可能被多个规则设置多次。 **CSS 定义了哪个规则比其它规则更具优先级，则更具优先级的规则必定被应用**：这被称为**层叠算法（cascade algorithm）**。

#### 1.2.4 CSS 语句

*CSS 规则*是样式表的主要组成块 —— 是你在 CSS 中最常见的一种。

`@-规则(At-rules)`在CSS中被用来传递元数据、条件信息或其它描述性信息。
- @charset 和 @import （元数据）
- @media 或 @document （条件信息，又被称为嵌套语句。)（有在特定条件匹配时才会应用到文档上。）
    - @media 只有在运行浏览器的设备匹配其表达条件时才会应用该@-规则的内容；
    - @supports 只有浏览器确实支持被测功能时才会应用该@-规则的内容；
    - @document 只有当前页面匹配一些条件时才会应用该@-规则的内容。
- @font-face （描述性信息）
```css
@import 'custom.css';
```
```css
/* 下面的嵌套语句只有在页面宽度超过801像素时才会应用。*/
@media (min-width: 801px) {
  body {
    margin: 0 auto;
    width: 800px;
  }
}
```

#### 1.2.5 简写

一些属性比如 `font`，`background`，`padding`，`border`，和 `margin` 被称为简写属性 —— 这是由于它们允许你在一行设置多个属性，从而节省时间并使代码更整洁。

```css
/* 在padding和margin这样的简写属性中，值赋值的顺序是top、right、bottom、left。 
   它们还有其他简写方式，例如给padding两个值，则第一个值表示top/bottom，第二个值表示left/right */
padding: 10px 5px 15px 15px 5px;

/*等价于*/
padding-top: 10px;
padding-right: 15px;
padding-bottom: 15px;
padding-left: 5px;
```

```css
background: red url(bg-graphic.png) 10px 10px repeat-x fixed;

/*等价于*/
background-color: red;
background-image: url(bg-graphic.png);
background-position: 10px 10px;
background-repeat: repeat-x;
background-scroll: fixed;
```

### 1.3 选择器

- **简单选择器（Simple selectors）**：通过元素类型、class 或 id 匹配一个或多个元素。
- **属性选择器（Attribute selectors）**：通过 属性 / 属性值 匹配一个或多个元素。
- **伪类（Pseudo-classes）**：匹配处于确定状态的一个或多个元素，比如被鼠标指针悬停的元素，或当前被选中或未选中的复选框，或元素是DOM树中一父节点的第一个子节点。
- **伪元素（Pseudo-elements）**:匹配处于相关的确定位置的一个或多个元素，例如每个段落的第一个字，或者某个元素之前生成的内容。 
- **组合器（Combinators）**：这里不仅仅是选择器本身，还有以有效的方式组合两个或更多的选择器用于非常特定的选择的方法。例如，你可以只选择divs的直系子节点的段落，或者直接跟在headings后面的段落。
- **多重选择器（Multiple selectors）**：这些也不是单独的选择器；这个思路是将以逗号分隔开的多个选择器放在一个CSS规则下面， 以将一组声明应用于由这些选择器选择的所有元素。

#### 1.3.1 简单选择器

- 元素选择器
- 类选择器
- ID 选择器
- ~~通用选择器~~:通用选择（*）是最终的王牌。它允许选择在一个页面中的所有元素。由于给每个元素应用同样的规则几乎没有什么实际价值，**大多数情况不会使用**！

##### 1.3.1.1 元素选择器

基于 HTML 元素的类型。

```css
p {
    boder: 1px solid;
}
```

###### 1.3.1.2 类选择器

类选择器由一个点“.”以及类后面的类名组成。

类名是在HTML class文档元素属性中没有空格的任何值。由你自己选择一个名字。（同样值得一提的是，文档中的多个元素可以具有相同的类名，而单个元素可以有多个类名(*以空格分开多个类名的形式书写*)。）

```html
<ul>
    <li class="first done">Create an HTML document</li>
    <li class="second done">Create a CSS style sheet</li>
    <li class="third">Link them all together</li>
</ul>
```
```css
.first {
    font-weight: bold;
}
.done {
    text-decoration: line-through;
}
```

###### 1.3.1.3 ID 选择器

ID选择器是由一个哈希/磅符号 (#)，后面跟着给定元素的ID名称组成的。

任何元素都可以使用id属性设置**唯一**的ID名称。

```html
<p id="polite"> — "Good morning."</p>
<p id="rude"> — "Go away!"</p>
```
```css
#polite {
    font-family: cursive;
}

#rude {
    font-family: monospace;
    text-transform: uppercase;
}
```

#### 1.3.2 属性选择器

属性选择器是一种特殊类型的选择器，它根据元素的 属性 和属性值来匹配元素。它们的通用语法由方括号 ([]) 组成，其中包含属性名称，后跟可选条件以匹配属性的值。

属性选择器可以根据其匹配属性值的方式分为两类： 
- 存在和值属性选择器
- 子串值属性选择器

##### 1.3.2.1 存在和值属性选择器

- [attr]：该选择器选择包含 attr 属性的所有元素，不论 attr 的值为何。
- [attr=val]：该选择器仅选择 attr 属性被赋值为 val 的所有元素。（必须正好相等，不多不少）
- [attr~=val]：该选择器仅选择具有 attr 属性的元素，而且要求 val 值是 attr 值包含的被空格分隔的取值列表里中的一个。（该属性可以有很多个值，只要其中一个为 val 即可）

```html
我的食谱配料: <i lang="fr-FR">Poulet basquaise</i>
<ul>
  <li data-quantity="1kg" data-vegetable>Tomatoes</li>
  <li data-quantity="3" data-vegetable>Onions</li>
  <li data-quantity="3" data-vegetable="spicy liquid">Garlic</li>           <!--spicy 有效，liquid 无效-->
  <li data-quantity="700g" data-vegetable="not spicy like chili">Red pepper</li>        <!-- spicy -->
  <li data-quantity="2kg" data-meat>Chicken</li>
  <li data-quantity="optional 150g" data-meat>Bacon bits</li>
  <li data-quantity="optional 10ml" data-vegetable="liquid">Olive oil</li>  <!-- liquid -->
  <li data-quantity="25cl" data-vegetable="liquid">White wine</li>          <!-- liquid -->
</ul>
```
```css
/* 所有具有"data-vegetable"属性的元素将被应用绿色的文本颜色 */
[data-vegetable] {
  color: green
}

/* 所有具有"data-vegetable"属性且属性值刚好为"liquid"的元素将被应用金色的背景颜色 */
[data-vegetable="liquid"] {
  background-color: goldenrod;
}

/* 所有具有"data-vegetable"属性且属性值**包含**"spicy"的元素，
即使元素的属性中还包含其他属性值，都会被应用红色的文本颜色 */
[data-vegetable~="spicy"] {
  color: red;
}
```

##### 1.3.2.2 子串值属性选择器

这种情况的属性选择器也被称为“伪正则选择器”，因为它们提供类似 regular expression 的灵活匹配方式（但请注意，这些选择器并不是真正的正则表达式）

- [attr|=val] : 选择attr属性的值是 val 或值以 val- 开头的元素（注意，这里的 “-” 不是一个错误，这是用来处理语言编码的）。
- [attr^=val] : 选择attr属性的值以 val 开头（包括 val）的元素。
- [attr$=val] : 选择attr属性的值以 val 结尾（包括 val）的元素。
- [attr*=val] : 选择attr属性的值中包含子字符串 val 的元素（一个子字符串就是一个字符串的一部分而已，例如，”cat“ 是 字符串 ”caterpillar“ 的子字符串）。

接上面 html 的例子：
```css
/* 语言选择的经典用法 */
[lang|="fr"] {
  font-weight: bold;
}

/* 
    具有"data-vegetable"属性含有值"not spicy"的所有元素,都变回绿色
*/
[data-vegetable*="not spicy"] {
  color: green;
}

/* 
   具有"data-quantity"属性其值以"kg"结尾的所有元素*/
[data-quantity$="kg"] {
  font-weight: bold;
}

/* 
   具有属性"data-quantity"其值以"optional"开头的所有元素 
*/
[data-quantity^="optional"] {
  opacity: 0.5;
}
```

#### 1.3.3 伪类和伪元素

##### 1.3.3.1 伪类

一个 CSS  伪类（pseudo-class） 是一个以冒号(:)作为前缀，被添加到一个选择器末尾的关键字，当你希望样式在特定状态下才被呈现到指定的元素时，你可以往元素的选择器后面加上对应的伪类（pseudo-class）。

- :active
- :any
- :checked
- :default
- :dir()
- :disabled
- :empty
- :enabled
- :first
- :first-child
- :first-of-type
- :fullscreen
- :focus
- :hover
- :indeterminate
- :in-range
- :invalid
- :lang()
- :last-child
- :last-of-type
- :left
- :link
- :not()
- :nth-child()
- :nth-last-child()
- :nth-last-of-type()
- :nth-of-type()
- :only-child
- :only-of-type
- :optional
- :out-of-range
- :read-only
- :read-write
- :required
- :right
- :root
- :scope
- :target
- :valid
- :visited

```html
<a href="https://developer.mozilla.org/" target="_blank">Mozilla Developer Network</a>
```

```css
/* 这些样式将在任何情况下应用于我们
的链接 */

a {
  color: blue;
  font-weight: bold;
}

/* 我们想让被访问过的链接和未被访问
的链接看起来一样 */

a:visited {
  color: blue;
}

/* 当光标悬停于链接，键盘激活或锁定
链接时，我们让链接呈现高亮 */

a:hover,
a:active,
a:focus {
  color: darkred;
  text-decoration: none;
}
```

##### 1.3.3.2 伪元素

伪元素前缀是两个冒号 (::) ， 同样是添加到选择器后面去选择某个元素的某个部分。

- ::after
- ::before
- ::first-letter
- ::first-line
- ::selection
- ::backdrop

```html
<ul>
  <li><a href="https://developer.mozilla.org/en-US/docs/Glossary/CSS">CSS</a> defined in the MDN glossary.</li>
  <li><a href="https://developer.mozilla.org/en-US/docs/Glossary/HTML">HTML</a> defined in the MDN glossary.</li>
</ul>
```

```css
/* 所有含有"href"属性并且值以"http"开始的元素，
将会在其内容后增加一个箭头（去表明它是外部链接）
*/

[href^=http]::after {
  content: '⤴';
}
```

#### 1.3.4 组合器和多个选择器

##### 1.3.4.1 组合器和选择器组

| 名称           | 组合器 | 选择器                                                                                           |
| -------------- | ------ | ------------------------------------------------------------------------------------------------ |
| 选择器组       | A, B   | 匹配满足A（和/或）B的任意元素                                                                    |
| 后代选择器     | A B    | 匹配B元素，满足条件：B是A的后代结点（B是A的子节点，或者A的子节点的子节点）                       |
| 子选择器       | A>B    | 匹配B元素，满足条件：B是A的直接子节点                                                            |
| 相邻兄弟选择器 | A+B    | 匹配B元素，满足条件：B是A的下一个兄弟节点（AB有相同的父结点，并且B紧跟在A的后面）                |
| 通用兄弟选择器 | A~B    | 匹配B元素，满足条件：B是A之后的任意一个兄弟节点（AB有相同的父节点，B在A之后，但不一定是紧挨着A） |

```html
<table lang="en-US" class="with-currency">
  <thead>
    <tr>
      <th scope="col">Product</th>
      <th scope="col">Qty.</th>
      <th scope="col">Price</th>
    </tr>
  </thead>
  <tfoot>
    <tr>
      <th colspan="2" scope="row">Total:</th>
      <td>148.55</td>
    </tr>
  </tfoot>
  <tbody>
    <tr>
      <td>Lawnchair</td>
      <td>1</td>
      <td>137.00</td>
    </tr>
    <tr>
      <td>Marshmallow rice bar</td>
      <td>2</td>
      <td>1.10</td>
    </tr>
    <tr>
      <td>Book</td>
      <td>1</td>
      <td>10.45</td>
    </tr>
  </tbody>
</table>
```

```css
/* 基本的table样式 */
table {
  font: 1em sans-serif;
  border-collapse: collapse;
  border-spacing: 0;
}

/* 所有在table里的td以及th，这里的逗号不是一个组合器，
它只是允许你把几个选择器对应到相同的CSS规则上.*/
table td, table th {
  border : 1px solid black;
  padding: 0.5em 0.5em 0.4em;
}

/* 所有table里的thead里的所有th */
table thead th {
  color: white;
  background: black;
}

/* 所有table里的tbody里的所有td（第一个除外），每个td都是由它上边的td选择 */
table tbody td + td {
  text-align: center;
}

/*table里所有的tbody里的td当中的最后一个 */
table tbody td:last-child {
  text-align: right
}

/* 所有table里的tfoot里的th */
table tfoot th {
  text-align: right;
  border-top-width: 5px;
  border-left: none;
  border-bottom: none;
}

/* 在table当中，所有的th之后的td */
table th + td {
  text-align: right;
  border-top-width: 5px;
  color: white;
  background: black;
}

/* 定位在“with-currency”类中拥有属性lang并且这个属性值为en-US的元素中的，最后td(:last-child)节点的前面（::before）*/
.with-currency[lang="en-US"] td:last-child::before {
  content: '$';
}

/* 定位在“with-currency”类中拥有属性lang并且这个属性值为fr的元素中的，最后td(:last-child)节点的后面（::after） */
.with-currency[lang="fr"] td:last-child::after {
  content: ' €';
}
```

##### 1.3.4.2 应用同一规则的选择器组

通过相互间用逗号分隔的多个选择器所形成的组，可以一次性将同一规则同时应用到多组选定元素。

```css
h1, h2, h3, h4, h5, h6 {
  font-family: helvetica, 'sans serif';
}
```

### 1.4 CSS 数值与单位

#### 1.4.1 数值

##### 1.4.1.1 长度和尺寸

像素 (px) 是一种**绝对单位**（absolute units）， 因为无论其他相关的设置怎么变化，像素指定的值是不会变化的。其他的绝对单位如下：
- mm, cm, in: 毫米（Millimeters），厘米（centimeters），英寸（inches）
- pt, pc: 点（Points (1/72 of an inch)）， 十二点活字（ picas (12 points.)）

 也有相对单位，他们是相对于当前元素的字号（ font-size ）或者视口（viewport ）尺寸。

- em:1em与当前元素的字体大小相同（更具体地说，一个大写字母M的宽度）。CSS样式被应用之前，浏览器给网页设置的默认基础字体大小是16像素，这意味着对一个元素来说1em的计算值默认为16像素。但是要小心—em单位是会继承父元素的字体大小，所以如果在父元素上设置了不同的字体大小，em的像素值就会变得复杂。现在不要过于担心这个问题，我们将在后面的文章和模块中更详细地介绍继承和字体大小设置。em是Web开发中最常用的相对单位。
- ex, ch: 分别是小写x的高度和数字0的宽度。这些并不像em那样被普遍使用或很好地被支持。
rem: REM（root em）和em以同样的方式工作，但它总是等于默认基础字体大小的尺寸；继承的字体大小将不起作用，所以这听起来像一个比em更好的选择，虽然在旧版本的IE上不被支持（查看关于跨浏览器支持 Debugging CSS.)
- vw, vh: 分别是视口宽度的1/100和视口高度的1/100，其次，它不像rem那样被广泛支持。

```html
<div>
  <div class="boxes">Fixed width layout with pixels</div>
  <div class="boxes">Liquid layout with percentages</div>
</div>
```
```css
/* 
在这里，我们给两个div设置了一些 margin, height, font-size, border and color 。 然后我们给第一个div和第二个div不同的 background-color，所以我们可以很容易地区分它们。 我们还将第一个div的 width 设置为650px，将第二个div的宽度设置为75％。 这样做的效果是，第一个div始终具有相同的宽度，即使视口大小被调整（当视口变得比屏幕更窄时，它将从屏幕上消失），而第二个div的宽度随着视口（viewport ）的变化而变化，使其始终保持其父元素宽度的75％。 在这个例子中，div的父元素是<body>元素，默认情况下宽度是视口宽度的100％。
 */
div .boxes {
  margin: 10px;
  font-size: 200%;
  color: white;
  height: 150px;
  border: 2px solid black;
}

.boxes:nth-child(1) {
  background-color: red;
  width: 650px;
}

.boxes:nth-child(2) {
  background-color: blue;
  width: 75%;
}
```

##### 1.4.1.2 无单位的值

在CSS中，你有时会遇到一些无单位的数值——这并不总是意味着错误，在某些情况下，使用无单位的数值是完全允许的。例如，如果你想让一个元素完全去除外边框和内边框，你可以只使用无单位的0——因为0就是0，不管单位是什么！

另一个例子是 line-height，设置元素中每行文本的高度。你可以使用单位设置特定的行的高度，但使用一个无单位的值往往更容易，它就像一个简单的乘法因子

```css
margin: 0;


/* 这里font-size的值为16px; 行高为font-size值的1.5倍，也就是24px。*/
p {
  line-height: 1.5;
}
```

##### 1.4.1.3 动画的数值

```css
@keyframes rotate {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}

p {
  color: red;
  width: 100px;
  font-size: 40px;
  transform-origin: center;
}

p:hover {
  animation-name: rotate;
  animation-duration: 0.6s;
  animation-timing-function: linear;
  animation-iteration-count: 5;
}
```

#### 1.4.2 百分比

大部分使用特定数值指定的内容同样可以使用百分比来指定。

反观那些宽度被设置为某个固定单位值（如px或em）的盒子，它们总是保持固定的尺寸，即使它们父容器的宽度发生变化。

```html
<div>
  <div class="boxes">Fixed width layout with pixels</div>
  <div class="boxes">Liquid layout with percentages</div>
</div>
```

```css
div .boxes {
  margin: 10px;
  font-size: 200%;
  color: white;
  height: 150px;
  border: 2px solid black;
}

.boxes:nth-child(1) {
  background-color: red;
  width: 650px;
}

.boxes:nth-child(2) {
  background-color: blue;
  width: 75%;
}
```

这两种不同的框布局类型通常被称为**动态（流体）布局**（跟随浏览器视口大小的变化）和**固定宽度布局**（不管怎样都保持不变），两种布局方式有着不同的应用场景：

可以使用动态布局来确保标准文档始终适合于屏幕，并且可以在不同大小的移动设备屏幕上表现良好。
一个固定宽度的布局可以用来保持在线地图的大小相同；浏览器视口可以在地图上滚动，只在一个时间内查看一定数量的地图。您可以立即看到的量取决于您的视口有多大。

#### 1.4.3 颜色

现代计算机中可用的标准颜色系统是24位，通过不同的红、绿、蓝通道，每个通道有256种不同的值，从而显示出大约1670万种不同的颜色。  (256 x 256 x 256 = 16,777,216.)

##### 1.4.3.1 关键字

特定字符串代表颜色值。（最古老）

```css
p {
  background-color: red;
}
```

##### 1.4.3.2 十六进制值

个颜色包括一个哈希/磅符号（#）和其后面紧跟的六个十六进制数，其中每个十六进制数可以是0和F之间的一个值（一共16个），0123456789abcdef。每对十六进制数代表一个通道（红色、绿色或者蓝色）允许我们指定256个可用值。 (16 x 16 = 256.)  

```css
/* equivalent to the red keyword */
p:nth-child(1) {
  background-color: #ff0000;
}

/* equivalent to the blue keyword */
p:nth-child(2) {
  background-color: #0000ff;
}

/* has no exact keyword equivalent */
p:nth-child(3) {
  background-color: #e0b0ff;
}
```

##### 1.4.3.3 RGB

一个RGB值是一个函数——rgb() ——给定的三个参数，分别表示红色，绿色和蓝色通道的颜色值，这与十六进制值的工作方式大致相同。

区别在于，RGB中每个通道不是由两个十六进制数字表示的，而是由0到255之间的十进制数表示的。

RGB值的支持度与十六进制不相上下，在你的工作中你可能不会遇到不支持它们的浏览器。RGB值可以说比十六进制值更直观，更容易理解。
```css
/* equivalent to the red keyword */
p:nth-child(1) {
  background-color: rgb(255,0,0);
}

/* equivalent to the blue keyword */
p:nth-child(2) {
  background-color: rgb(0,0,255);
}

/* has no exact keyword equivalent */
p:nth-child(3) {
  background-color: rgb(224,176,255);
}
```

##### 1.4.3.4 HSL

hsl()函数接受三个表示色调、饱和度以及明度的参数，使用与上述三种不同的方式来区分大约1670万种颜色：
- 色调：颜色的底色调。这个值在0到360之间，表示色轮的角度。
- 饱和度：饱和度是多少？这需要一个从0-100%的值，其中0是没有颜色（它将显示为灰色），100%是全彩色饱和度。
- 明度：颜色有多亮或明亮？这需要一个从0-100%的值，其中0是无光（它会出现全黑的），100%是充满光的（它会出现全白）。

```css
/* equivalent to the red keyword */
p:nth-child(1) {
  background-color: hsl(0,100%,50%);
}

/* equivalent to the blue keyword */
p:nth-child(2) {
  background-color: hsl(240,100%,50%);
}

/* has no exact keyword equivalent */
p:nth-child(3) {
  background-color: hsl(276,100%,85%);
}
```

##### 1.4.3.5 RGBA 和 HSLA

RGB和HSL都有相应的模式——RGBA和HSLA——不仅允许您设置想要显示的颜色,*还有此颜色的透明度*（ transparency ）。它们和与之相应的函数采用同样的参数,再加上第四个范围在0-1的值——设置透明度,或者说alpha通道。0是完全透明的,1是完全不透明的。
```css
p {
  height: 50px;
  width: 350px;
}

/* Transparent red */
p:nth-child(1) {
  background-color: rgba(255,0,0,0.5);
  position: relative;
  top: 30px;
  left: 50px;
}

/* Transparent blue */
p:nth-child(2) {
  background-color: hsla(240,100%,50%,0.5);
}
```

##### 1.4.3.6 不透明度

通过CSS——opacity 属性。与设置某个特定颜色的透明度相比，这会设置所有选定元素以及它们的孩子节点的不透明度。

```css
/* Red with RGBA */
p:nth-child(1) {
  background-color: rgba(255,0,0,0.5);
}

/* Red with opacity */
p:nth-child(2) {
  background-color: rgb(255,0,0);
  opacity: 0.5;
}
```

#### 1.4.4 函数

每当你看到一个名字后跟着括号,括号里包含用逗号分隔的一个或多个值,那么你所使用的就是一个函数。
```css
/* calculate the new position of an element after it has been rotated by 45 degress */
transform: rotate(45deg);
/* calculate the new position of an element after it has been moved across 50px and down 60px */
transform: translate(50px, 60px);
/* calculate the computed value of 90% of the current width minus 15px */
width: calc(90% - 15px);
/* fetch an image from the network to be used as a background image */
background-image: url('myimage.png');
```

### 1.5 层叠和继承

但在实际的工作中，我们可能还有些疑惑，当有多个选择器匹配到同一个元素上时，哪个选择器的 CSS 规则最终会应用到元素上？

#### 1.5.1 层叠

CSS 是 Cascading Style Sheets 的缩写，这暗示层叠（cascade）的概念是很重要的。

什么选择器在层叠中胜出取决于三个因素（这些都是按重量级顺序排列的——前面的的一种会否决后一种）：
1. 重要性（Importance）
2. 专用性（Specificity）
3. 源代码次序（Source order）

##### 1.5.1.1 重要性

在CSS中，有一个特别的语法可以让一条规则总是优先于其他规则：**!important**。（就只有这个可以影响吧）

但是！我们建议你**千万不要**使用它，除非你绝对必须使用它。

```css
/* 元素选择器 < 类选择器 < id选择器 */
/* 但是 border 运用的 是类选择器 border: none ，因为!important
/* <p class="better" id="winning">  </p> */

#winning {
  background-color: red;
  border: 1px solid black;
}

.better {
  background-color: gray;
  border: none !important;
}

p {
  background-color: blue;
  color: white;
  padding: 5px;
}
```

##### 1.5.1.2 专用性

**专用性**基本上是衡量选择器的具体程度的一种方法——它能匹配多少元素。
- 类选择器具有更高的专用性，所以将战胜元素选择器。
- ID选择器有甚至更高的专用性, 所以将战胜类选择器. 
- 战胜ID选择器的唯一方法是使用 !important。

一个选择器具有的专用性的量是用四种不同的值（或组件）来衡量的，它们可以被认为是千位，百位，十位和个位——在四个列中的四个简单数字：
- 千位：如果声明是在style 属性中该列加1分（这样的声明没有选择器，所以它们的专用性总是1000。）否则为0。
- 百位：在整个选择器中每包含一个ID选择器就在该列中加1分。
- 十位：在整个选择器中每包含一个类选择器、属性选择器、或者伪类就在该列中加1分。
- 个位：在整个选择器中每包含一个元素选择器或伪元素就在该列中加1分。

| 选择器 | 千位 | 百位 | 十位 | 个位 | 合计值 |
| ------ | ---- | ---- | ---- | ---- | ------ |
| h1     | 0    | 0    | 0    | 1    | 0001   |
| #indentifier     | 0    |  1    | 0     |  0   |  0100   |
| h1 + p::first-letter | 0     | 0     |  0    |  3  |  0003   |
| li > a[href*="zh-CN"] > .inline-warning  | 0    |  0    | 2     |  2    |   0022   |
| 没有选择器, 规则在一个元素的 `<style>` 属性里     | 1    |  0    | 0     |  0      |  1000   |

##### 1.5.1.3 源代码次序

如果多个相互竞争的选择器具有相同的重要性和专用性，那么第三个因素将帮助决定哪一个规则获胜——后面的规则将战胜先前的规则。
```css
p {
  color: blue;
}

/* This rule will win over the first one */
p {
  color: red;
}
```

#### 1.5.2 继承

##### 1.5.2.1 控制继承

CSS为处理继承提供了四种特殊的通用属性值：

- **inherit** ： 该值将应用到选定元素的属性值设置为与其父元素一样。
- **initial** ：该值将应用到选定元素的属性值设置为与浏览器默认样式表中该元素设置的值一样。如果浏览器默认样式表中没有设置值，并且该属性是自然继承的，那么该属性值就被设置为 inherit。
- **unset** ：该值将属性重置为其自然值，即如果属性是自然继承的，那么它就表现得像 inherit，否则就是表现得像 initial。
- **revert** ：如果当前的节点没有应用任何样式，则将该属性恢复到它所拥有的值。换句话说，属性值被设置成自定义样式所定义的属性（如果被设置）， 否则属性值被设置成用户代理的默认样式。

```html
<ul>
  <li>Default <a href="#">link</a> color</li>
  <li class="inherit">Inherit the <a href="#">link</a> color</li>
  <li class="initial">Reset the <a href="#">link</a> color</li>
  <li class="unset">Unset the <a href="#">link</a> color</li>
</ul>
```

```css
/*我们首先设置<body> 的color为绿色。*/
body {
  color: green;
}

/*第二个规则设置一个类 inherit 的元素内的链接，并从父类继承它的颜色。在这种情况下, 意思是说链接继承了父元素<li>的颜色，默认情况下<li>的颜色来自于它的父元素 <ul> , 最后<ul> 继承自 <body>元素，而<body>的color 根据第一条规则设置成了绿色。*/
.inherit a {
  color: inherit;
}

/*第三个规则选择了在元素上使用类 initial 的任意链接然后设置他们的颜色为 initial 。通常， initial 的值被浏览器设置成了黑色，因此该链接被设置成了黑色。*/
.initial a {
  color: initial;
}

/*最后一个规则选择了在元素上使用类 unset 的所有链接然后设置它们的颜色为 unset  ——即我们不设置值。因为color属性是一个自然继承的属性，它实际上就像把值设置成 inherit 一样。结果是，该链接被设置成了与body一样的颜色——绿色。*/
.unset a {
  color: unset;
}
```

### 1.6 盒模型

#### 1.6.1 盒属性

![box_property](https://raw.githubusercontent.com/514723273/.md-Pictures/master/box_property.png)

##### 1.6.1.1 width 和 height

width 和 height 设置内容框（content box）的宽度和高度。

内容框是框内容显示的区域——包括框内的文本内容，以及表示嵌套子元素的其它框。

##### 1.6.1.2 padding

padding 表示一个 CSS 框的内边距 ——这一层位于内容框的外边缘与边界的内边缘之间。

该层的大小可以通过简写属性padding 一次设置所有四个边，或用 padding-top、padding-right、padding-bottom 和 padding-left 属性一次设置一个边。

##### 1.6.1.3 border

CSS 框的边界（border）是一个分隔层，位于内边距的外边缘以及外边距的内边缘之间。

- **border-top, border-right, border-bottom, border-lef**t: 分别设置某一边的边界厚度／风格／颜色。
- **border-width, border-style, border-color**: 分别仅设置边界的厚度／风格／颜色，并应用到全部四边边界。
- 你也可以单独设置某一个边的三个不同属性，如 border-top-width, border-top-style, border-top-color，等。 

##### 1.6.1.4 margin

外边距（margin）代表 CSS 框周围的外部区域，称为外边距，它在布局中推开其它 CSS 框。

#### 1.6.2 高级的框操作

##### 1.6.2.1 溢流

当你使用绝对的值设置了一个框的大小（如，固定像素的宽/高），允许的大小可能不适合放置内容，这种情况下内容会从盒子溢流。

我们使用overflow属性来控制这种情况的发生。它有一些可能的值，但是最常用的是：

- **auto**: 当内容过多，溢流的内容被隐藏，然后出现滚动条来让我们滚动查看所有的内容。
- **hidden**: 当内容过多，溢流的内容被隐藏。
- **visible**: 当内容过多，溢流的内容被显示在盒子的外边（这个是默认的行为）

```css
p {
  width  : 400px;
  height : 2.5em;
  padding: 1em 1em 1em 1em;
  border : 1px solid black;
}

.autoscroll { overflow: auto;    }
.clipped    { overflow: hidden;  }
.default    { overflow: visible; }
```

##### 1.6.2.2 背景裁剪

```css
div {
  width  : 60px;
  height : 60px;
  border : 20px solid rgba(0, 0, 0, 0.5);
  padding: 20px;
  margin : 20px 0;

  background-size    : 140px;
  background-position: center;
  background-image   : url('https://mdn.mozillademos.org/files/11947/ff-logo.png');
  background-color   : gold;
}

.default     { background-clip: border-box;  }
.padding-box { background-clip: padding-box; }
.content-box { background-clip: content-box; }
```

##### 1.6.2.3 轮廓

最后，还有重要的一点， 一个框的 outline 是一个看起来像是边界但又不属于框模型的东西。它的行为和边界差不多，但是并不改变框的尺寸（更准确的说，轮廓被勾画于在框边界之外，外边距区域之内）

#### 1.6.3 CSS 框类型

我们可以通过display属性来设定元素的框类型。display属性有很多的属性值。

我们将关注三个最常见的类型：block, inline, and inline-block。
- 块框（ block box）是定义为堆放在其他框上的框（例如：其内容会独占一行），而且可以设置它的宽高，之前所有对于框模型的应用适用于块框 （ block box）
- 行内框（ inline box）与块框是相反的，它随着文档的文字流动（例如：它将会和周围的文字和其他行内元素出现在同一行，而且它的内容会像一段中的文字一样随着文字部分的流动而打乱），对行内盒设置宽高无效，设置padding, margin 和 border都会更新周围文字的位置，但是对于周围的的块框（ block box）不会有影响。
- 行内块状框（inline-block box） 像是上述两种的集合：它不会重新另起一行但会像行内框（ inline box）一样随着周围文字而流动，而且他能够设置宽高，并且像块框一样保持了其块特性的完整性，它不会在段落行中断开。（在下面的示例中，行内块状框会放在第二行文本上，因为第一行没有足够的空间，并且不会突破两行。然而，如果没有足够的空间，行内框会在多条线上断裂，而它会失去一个框的形状。）

## 第二章 样式化文本

## 第三章 样式化区块

## 第四章 CSS 排版概述