# React16.6官方文档

## 第一章 JSX 简介

```js
const element = <h1>Hello, world!</h1>;
```
这种看起来可能有些奇怪的标签语法既不是字符串也不是 HTML ，被称为 JSX 。 一种 JavaScript 的语法扩展。 

### 1.1 在 JSX 中使用表达式

你可以任意地在 JSX 当中使用 JavaScript 表达式（表达式是**解析为值**的任何有效代码单元。），在 JSX 当中的表达式要**包含在大括号里**。

推荐在 JSX 代码的外面*扩上一个小括号*，这样可以防止分号自动插入的 bug。

```js
const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const element = (
  <h1>
    Hello, {formatName(user)}!      {/*表达式*/}
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

### 1.2 JSX 本身其实也是一种表达式

在编译之后，JSX 其实会被转化为普通的 JavaScript 对象。

这也就意味着，你其实可以在 if 或者 for 语句里使用 JSX ，将它赋值给变量，当作参数传入，作为返回值都可以。

### 1.3 JSX 属性

警告：React DOM 使用 camelCase 小驼峰命名 来*定义属性的名称*，而不是使用 HTML 的属性名称。

*例如，class 变成了 className，而 tabindex 则对应着 tabIndex。*

```js
/**
* 可以使用 引号 来定义以 字符串 为值的属性
**/
const element = <div tabIndex="0"></div>;
/**
* 可以使用 大括号 来定义以 JavaScript 表达式 为值的属性
**/
const element = <img src={user.avatarUrl}></img>;   //就不要在外边再套引号了
```

### 1.4 JSX 防注入攻击

```js
/**
* 可以放心地在 JSX 当中使用用户输入
**/
const title = response.potentiallyMaliciousInput;
// 直接使用是安全的：
const element = <h1>{title}</h1>;
```

React DOM 在渲染之前默认会 过滤 所有传入的值。所有的内容在渲染之前都被**转换成了字符串**。可以有效地防止 XSS (跨站脚本) 攻击。

### 1.5 JSX 代表 Objects

Babel 转译器会把 JSX 转换成一个名为 React.createElement() 的方法调用。

```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
//等价于
const element = React.createElement(
  'h1',                     //标签
  {className: 'greeting'},  //属性
  'Hello, world!'           //值
);

//React.createElement 返回一个类似下面例子的对象
// 注意: 以下示例是简化过的（不代表在 React 源码中是这样）
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world'
  }
};
```

React 通过读取这些对象来构建 DOM 并保持数据内容一致。

## 第二章 元素渲染

元素是构成 React 应用的最小单位。用来描述你在屏幕上看到的内容。

（元素与组件定义不同，元素事实上是构成组件的一个部分）

```js
const element = <h1>Hello, world</h1>;
```

与浏览器的 DOM 元素不同，React 当中的元素事实上是普通的对象，React DOM 可以确保 浏览器 DOM 的数据内容与 React 元素保持一致。

### 2.1 将元素渲染到 DOM 中

- 首先我们在一个 HTML 页面中添加一个`id="root"`的`<div>`（一般只会定义一个根节点）
- 要将React元素渲染到根DOM节点中，我们通过把它们都传递给`ReactDOM.render()`的方法来将其渲染到页面上
```js
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

### 2.2 React 只会更新必要的部分

React 元素都是immutable 不可变的。当元素被创建之后，你是无法改变其内容或属性的。

```js
/**
* 根据我们现阶段了解的有关 React 知识，更新界面的唯一办法
* 创建一个新的元素，然后将它传入 ReactDOM.render() 方法
**/
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>     {/*不断获取当前时间*/}
    </div>
  );
  ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);         {/*每秒钟调用一次 ReactDOM.render().*/}
```

React DOM 首先会比较元素内容先后的不同，而在渲染过程中只会更新改变了的部分。

![更新改变了的部分](https://raw.githubusercontent.com/514723273/.md-Pictures/master/20190531104328.png)


## 第三章 组件 & Props

组件从概念上看就像是*函数*，它可以接收任意的输入值（称之为“props”），并返回一个需要在页面上展示的React元素。

### 3.1 函数定义/类定义组件

```js
/**
* 定义一个组件最简单的方式是使用JavaScript函数
**/
//称这种类型的组件为 函数定义组件
function Welcome(props) {
  return (
    <h1>Hello, {props.name}</h1>;
  )
}
```
```js
/**
* 也可以使用 ES6 class 来定义一个组件
**/
class Welcome extends React.Component {
  render() {
    return (
          <h1>Hello, {this.props.name}</h1>;
    )
  }
}
```

### 3.2 组件渲染
```js
//React元素 是 DOM标签
const element = <div />;
/**
* 当React遇到的元素是 用户自定义的组件
* 它会将JSX属性作为单个对象传递给该组件，这个对象称之为“props”
**/
const element = <Welcome name="Sara" />;
```
```js
/**
* 传值渲染的过程
**/
//2.
function Welcome(props) {
  //3.
  return (
    <h1>Hello, {this.props.name}</h1>;
  );
}

//1. 3返回到这
const element = <Welcome name="Sara" />;

//上面为一整个步骤，创建组件 element

//4. 最后进行挂载渲染
ReactDOM.render(
  element, 
  document.getElementById('root')
);

//1. 我们对<Welcome name="Sara"/>元素调用了ReactDOM.render()方法。
//2. React将{name: 'Sara'}作为props传入并调用Welcome组件。
//3. Welcome组件将<h1>Hello, Sara</h1>元素作为结果返回。

//4. React DOM将DOM更新为<h1>Hello, Sara</h1>。
```

### 3.3 组合组件

注意：组件的返回值只能有一个根元素。这也是我们要用一个 <div> 来包裹所有其他元素的原因。

尽可能地抽离出可复用组件，再组合使用。

### 3.4 Props 的只读性

**所有的React组件必须像纯函数那样使用它们的props。**
```js
/**
* 纯函数
* 它没有改变它自己的输入值，当传入的值相同时，总是会返回相同的结果。
**/
function sum(a, b) {
  return a + b;
}

/**
* 非纯函数
* 会改变它自身的输入值
**/
function withdraw(account, amount) {
  account.total -= amount;    //改变了输入值 account
}
```

## 第四章 State & 生命周期
