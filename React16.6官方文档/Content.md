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

本章将会介绍一个自动更新的时钟组件不断改进。

```js
/**
* 尝试一：调用 ReactDOM.render() 方法来改变输出
**/

function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  //函数内部调用 ReactDOM.render() 函数
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}
//每一秒 执行一次 tick 函数
//重新声明元素，重新渲染
setInterval(tick, 1000);
```

```js
/**
* 尝试二：封装时钟
**/
//把时钟提出来，封装成函数
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  )
}
//只执行重新渲染
function tick() {
  ReactDOM.render(
    //不断传入 date 属性。  //不应该传入，而是要组件内部实现
    <Clock date={new Date()}/>,
    document.getElementById('root‘)
  );
}

setInterval(tick, 1000);

//缺陷：Clock设置一个定时器并且每秒更新UI 应该是Clock的实现细节。
//理想情况下，我们写一次 Clock 然后它能更新自身，如下
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
//为了实现这个需求，我们需要为 Clock 组件添加状态
```

状态与属性十分相似，但是状态是**私有**的，完全受控于当前组件。

### 4.1 将函数转换为类

相比较函数，使用类就允许我们使用其它特性，例如局部状态、生命周期钩子。

```js
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        {/**/}
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    )
  }
}
```

### 4.2 为一个类添加局部状态

```js
/**
* 尝试三：使用状态替换属性（ state 替换 props ）
* 我们会通过3个步骤将 date 从属性移动到状态中
**/

class Clock extends React.Component {
  //2. 添加一个类构造函数来初始化状态 this.state
  constructor(props) {
    super(props);
    this.state = {
      date: new Date()
    };
  }
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        {/*1. 在 render() 方法中使用 this.state.date 替代 this.props.date*/}
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

//3. 从 <Clock /> 元素移除 date 属性
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);

//此时 Clock 还不会动态更新，还未完成 使 Clock 设置自己的计时器并每秒更新一次
```

### 4.3 将生命周期方法添加到类中

每当 Clock 组件第一次加载到 DOM 中的时候，我们都想生成定时器，这在React中被称为**挂载**。

同样，每当Clock生成的这个 DOM 被移除的时候，我们也会想要清除定时器，这在React中被称为**卸载**。

```js
/**
* 尝试四：尝试三的改进版，通过生命周期钩子控制组件内的计时器
**/
class Clock extends React.Component {
  constructor(props) {
    super(props);
    //如果 不在 render() 中使用某些东西，它就不应该在状态中
    this.state = {
      date: new Date()
    };
  }

  //当组件输出到 DOM 后会执行 componentDidMount() 钩子，这是一个建立定时器的好地方
  componentDidMount() {
    //注意如何在 this 中保存定时器ID
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  //在 componentWillUnmount()生命周期钩子中卸载计时器
  componentWillMount() {
    clearInterval(this.timerID);
  }

  tick() {
    //只改变数据了，不再重新调用 render 方法了！
    setDate({
      date: new Date()
    });
  }
  //每次 tick 被调用后，render() 都会被重新执行一次更新 DOM
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleString()}.</h2>
      </div>
    );
  }
}
```
快速回顾一下发生了什么以及调用方法的顺序：

1. 当 <Clock /> 被传递给 ReactDOM.render() 时，React 调用 Clock 组件的*构造函数*。 由于 Clock 需要显示当前时间，所以使用包含当前时间的对象来*初始化 this.state*。 我们稍后会更新此状态。

2. React 然后调用 *Clock 组件的 render() 方法*。这是 React 了解屏幕上应该显示什么内容，然后 React 更新 DOM 以匹配 Clock 的渲染输出（ReactDOM.render(<Clock />, document.getElementById('root'))）。

3. 当 Clock 的输出插入到 DOM 中时，React *调用 componentDidMount() 生命周期钩子*。 在其中，Clock 组件要求浏览器设置一个定时器，每秒钟调用一次 tick()。

4. 浏览器每秒钟调用 tick() 方法。 在其中，Clock 组件通过使用包含当前时间的对象调用 setState() 来调度UI更新。 通过调用 setState() ，React 知道状态已经改变，并再次调用 render() 方法来确定屏幕上应当显示什么。 这一次，render() 方法中的 this.state.date 将不同，所以渲染输出将包含更新的时间，并相应地更新DOM。

5. 一旦Clock组件被从DOM中移除，React会调用componentWillUnmount()这个钩子函数，定时器也就会被清除。

### 4.4 正确地使用状态

关于 setState() 这里有三件事情需要知道

#### 4.4.1 不要直接更新状态

构造函数是唯一能够初始化 this.state 的地方。

```js
this.state.comment = 'Hello'; // Wrong

this.setState({comment: 'Hello'});  // Correct
```

#### 4.4.2 状态更新可能是异步地

React 可以将多个 setState() 调用合并成一个调用来提高性能。

因为 this.props 和 this.state 可能是异步更新的，你不应该依靠它们的值来计算下一个状态。

```js
//Wrong
this.set({
  counter: this.state.counter + this.props.increment,
});

//Correct
//要修复它，使用第二种形式的 setState() 来接受 一个函数 而不是一个对象。
//该函数将接收 先前的状态，此次更新被应用时的props ：
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```

#### 4.4.3 状态更新合并

当你调用 setState() 时，React 将你提供的对象合并到当前状态。（简言之就是，更新的状态变量使用新值，没更新的状态使用原值，合并到当前状态）

### 4.5 数据自顶向下流动