# React16.6官方文档

## 第一章 JSX 简介

```js
const element = <h1>Hello, world!</h1>;
```
这种看起来可能有些奇怪的标签语法既不是字符串也不是 HTML ，被称为 JSX 。 一种 JavaScript 的语法扩展。 

### 1.1 在 JSX 中使用表达式

你可以任意地在 JSX 当中使用 JavaScript 表达式（表达式是**解析为值**的任何有效代码单元。），在 JSX 当中的表达式要**包含在大括号里**。

推荐在 JSX 代码的外面*扩上一个小括号*，这样可以防止分号自动插入的 bug。

**if 语句和 for 循环在 JavaScript 中不是表达式**，因此它们不能直接在 JSX 中使用，但是你可以将它们放在周围的代码中。

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
```js
function NumberDescriber(props) {
  let description;
  //if 放在外边，
  if (props.number % 2 == 0) {
    description = <strong>even</strong>;
  } else {
    description = <i>odd</i>;
  }
  // {} 大括号内放变量
  return <div>{props.number} is an {description} number</div>;
}
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

#### 4.4.2 状态更新可能是异步的

React 可以将多个 setState() 调用合并成一个调用来提高性能。

因为 this.props 和 this.state 可能是异步更新的，你不应该依靠它们的值来计算下一个状态。

```js
//Wrong
this.set({
  counter: this.state.counter + this.props.increment,
});

//Correct
//要修复它，使用第二种形式的 setState() 来接受 一个函数 而不是一个对象。
//该函数将接收 先前的状态，此次更新被应用时的 props ：
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```

#### 4.4.3 状态更新合并

当你调用 setState() 时，React 将你提供的对象合并到当前状态。（简言之就是，更新的状态变量使用新值，没更新的状态使用原值，合并到当前状态）

### 4.5 数据自顶向下流动

### 4.6 props state 与 render 函数的关系

当组件的 state 或者 props 发生改变的时候， render 函数就会发生改变。

当父组件的 render 函数被运行时，它的子组件的 render 都将被重新运行。

### 4.7 React 完整的生命周期

生命周期函数指在某一时刻组件**会自动调用执行**的函数

![](https://raw.githubusercontent.com/514723273/.md-Pictures/master/20190604203050.png)

#### 4.7.1 Initialization 初始化

设置props和state，这是在constructor中完成的。

#### 4.7.2 Mounting 挂载

- componentWillMount 在组件**即将**被挂载到页面的时刻自动执行
- render 页面挂载执行
- componentDidMount 挂载结束后执行

#### 4.7.3 Updation 组件更新

1. props 独有（多一个）：
- componentWillReceiveProps 当一个组件从*父组件**接受参数*，如果这个组件第一次存在于父组件中，不会执行；如果这个组件之前已经存在于父组件中，才会执行。

2. props和state更新共有的函数：（就是 state 全部）
- shouldComponentUpdate 组件被更新前，自动执行，返回的是一个bool值。你这个组件需要更新吗？？？返回true需要，**返回false不更新，下面函数都不会执行**。
- componentWillUpdate 组件被更新前，它会被自动执行，但是他是在 shouldComponentUpdate 之后执行。
- render 重新渲染DOM。
- componentDidUpdate 件更新结束后执行。

#### 4.7.4 Unmount 卸载

- componentWillUnmount 当这个组件即将被从页面移除的时候，自动执行。（例如 todolist 删去其中一个 todo 项）

## 第五章 事件处理

React 元素的事件处理和 DOM元素的很相似。但是有一点语法上的不同:
- React事件绑定属性的命名采用驼峰式写法，而不是小写。
- 如果采用 JSX 的语法你需要传入一个函数作为事件处理函数，而不是一个字符串(DOM元素的写法)
- 不能使用返回 false 的方式阻止默认行为。必须明确的使用 preventDefault。

```js
/**
* 一二不同举例：
* onClick 驼峰
* 传入函数
**/
//传统
<button onclick="activateLasers()">
  Activate Lasers
</button>
//React
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

```js
/**
* 三不同举例：
* 
**/
//传统
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
//React
function ActionLink() {

  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );

}
```

 JSX 回调函数中的 this，类的方法默认是不会绑定 this 的。如果忘记 bind() ，当你调用这个函数的时候 this 的值会是 undefined。这并不是 React 的特殊行为；它是函数如何在 JavaScript 中运行的一部分。

通过 bind 方式向监听函数传参，在类组件中定义的监听函数，事件对象 e 要排在所传递参数的后面。

## 第六章 条件渲染

在 React 中，你可以创建不同的组件来封装各种你需要的行为。然后还可以根据应用的状态变化只渲染其中的一部分。（创建不同的组件，根据不同的条件，显示不同的组件）
```js
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {   //根据 isLoggedIn 的值渲染不同的问候语
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}

ReactDOM.render(
  // Try changing to isLoggedIn={true}:
  <Greeting isLoggedIn={false} />,
  document.getElementById('root')
);
```

### 6.1 if 语句和元素变量

*可以使用变量来储存元素。*它可以帮助你有条件的渲染组件的一部分，而输出的其他部分不会更改。
```js
/**
* 两个新组件分别代表注销和登录
**/
function LoginButton(props) {
  return (
    <button onClick={props.onClick}>
      Login
    </button>
  );
}

function LogoutButton(props) {
  return (
    <button onClick={props.onClick}>
      Logout
    </button>
  );
}

class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }

  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }

  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }

  render() {
    const isLoggedIn = this.state.isLoggedIn;

    //元素变量
    let button = null;
    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}

ReactDOM.render(
  <LoginControl />,
  document.getElementById('root')
);
```

### 6.2 与运算符 &&

```js
//之所以能这样做，是因为在 JavaScript 中，true && expression 总是返回 expression，而 false && expression 总是返回 false。
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
);
```

### 6.3 三目运算符

条件渲染的另一种方法是使用 JavaScript 的条件运算符 condition ? true : false。

```js
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn ? (
        <LogoutButton onClick={this.handleLogoutClick} />
      ) : (
        <LoginButton onClick={this.handleLoginClick} />
      )}
    </div>
  );
}
```

### 6.4 阻止组件渲染

在极少数情况下，你可能希望隐藏组件，即使它被其他组件渲染。让 render 方法返回 null 而不是它的渲染结果即可实现。

组件的 render 方法返回 null 并不会影响该组件生命周期方法的回调。例如，componentWillUpdate 和 componentDidUpdate 依然可以被调用。

## 第七章 列表 & Keys

### 7.1 渲染多个组件

可以通过使用 {} 在JSX内构建一个元素集合。

```js
const numbers = [1, 2, 3, 4,5];
const listItems = numbers.map(item => 
  <li>{number}</li>
);
//把整个 listItems 插入到 ul 元素中
ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);
```

### 7.2 基础列表组件

```js
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map(item => 
    {/*如果没有这条语句，会报警告。当创建一个元素时，必须包括一个特殊的 key 属性。*/}
    <li key={item.toString}>   
      {item}
    </li>
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

### 7.3 Keys

Keys可以在DOM中的某些元素被增加或删除的时候，帮助React识别哪些元素发生了变化。因此你应当给数组中的每一个元素赋予一个确定的标识。

一个元素的key最好是这个元素在列表中拥有的一个独一无二的字符串。通常，我们使用来自数据的id作为元素的key。

当元素没有确定的id时，你可以使用他的序列号索引 index 作为 key 。

如果列表可以重新排序，我们不建议使用索引来进行排序，因为这会导致渲染变得很慢。[深度解析 Key 的必要性](https://www.reactjscn.com/docs/reconciliation.html#%E9%80%92%E5%BD%92%E5%AD%90%E8%8A%82%E7%82%B9)

### 7.4 用 Keys 提取组件

元素的key只有在它和它的兄弟节点对比时才有意义。

```js
//错误的示范

/**
* 如果你提取出一个ListItem组件，你应该把key保存在数组中的这个<ListItem />元素上，而不是放在ListItem组件中的<li>元素上。（只放在有列表循环的最外层一项组件）
**/

function ListItem(props) {
  const value = props.value;
  return (
    // 错啦！你不需要在这里指定key:
    <li key={value.toString()}>
      {value}
    </li>
  );
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    //错啦！元素的key应该在这里指定：
    <ListItem value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```
```js
//正确的示范

function ListItem(props) {
  const value = props.value;
  // 对啦！这里不需要指定key:
  return (
    <li>
      {value}
    </li>
  );
}

function NumberList(props) {
  const numbers = props.numbers;
  //也可以直接把 map 嵌到 底下的 {} 大括号中，不需要变量 listItems
  const listItems = numbers.map((item, index) => 
    // 又对啦！key应该在数组的上下文中被指定
    <ListItem key={index} value={item} />
  )
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

## 第八章 表单

HTML表单元素与React中的其他DOM元素有所不同，因为表单元素生来就保留一些内部状态。

（比如 HTML input 输入框 可以直接输入，不需要额外的操作；而 React 需要编写函数，处理 onChange 事件，改变 input 的 value）

大多数情况下，我们都会构造一个处理提交表单并可访问用户输入表单数据的函数。实现这一点的标准方法是使用一种称为“受控组件”的技术。

```js
<form>
  <label>
    Name:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="Submit" />
</form>
```

### 8.1 受控组件

在HTML当中，像`<input>`,`<textarea>`, 和 `<select>` 这类表单元素会维持自身状态，并根据用户输入进行更新。

但在React中，可变的状态通常保存在组件的状态属性中，并且只能用 setState() 方法进行更新。

总之，`<input type="text">`, `<textarea>`, 和 `<select>` 都十分类似 - 他们都通过传入一个value属性来实现对组件的控制。

#### 8.1.1 input 标签 & textarea 标签

在React中，`<textarea>` 会用value属性来代替（和 input 框同款使用）。这样的话，表单中的 `<textarea>` 非常类似于使用单行输入的表单。

```js
/**
* 上个例子中在提交表单时输出name,我们可以写成“受控组件”的形式:
**/

class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: ''
    }
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(e) {
    const value = e.target.value;
    this.setState((prevState, props) => ({
        value: value
      })
    )
  }

  handleSubmit(e) {
    alert('A name was submitted: ' + this.state.value);
    //如果不添加这条语句，会继续执行和HTML的默认行为一样的行为：跳转到一个新页面
    //在这里就是刷新了本页，清空了 input 框内内容
    //添加后，不会跳转，不会刷新
    e.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange}/>
          <textarea value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    )
  }
}
```

#### 8.1.2 select 标签

```html
/**
* 在HTML当中，<select> 会创建一个下拉列表。例如这个 HTML 就创建了一个下拉列表的原型
**/
<select>
  <option value="grapefruit">Grapefruit</option>
  <option value="lime">Lime</option>
  <option selected value="coconut">Coconut</option>   //Coconut选项最初由于 selected 属性是被选中的。
  <option value="mango">Mango</option>
</select>
```

在React中，并不使用之前的selected属性，而在根select标签上用value属性来表示选中项
```js
class FlavorForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: "mango"
    };
    this.handleSubmit = this.handleSubmit.bind(this);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(e) {
    const value = e.target.value;
    this.setState((prevState, props) => ({
      value: value
    }));
  }

  handleSubmit(e) {
    alert('Your favorite flavor is: ' + this.state.value);
    e.preventDefault();
  }

  render() {
    return (
      //form 表单有 属性 onSubmit 而不是 button 上 绑定
      <form onSubmit={this.handleSubmit}>
        <label>
          Pick your favorite La Croix flavor: 
          {/* 这边通过 value 来控制 下拉框 哪个被选中 */}
          <select value={this.state.value}  onChange={this.handleChange}>
            <option value="lime">Lime</option>
            <option value="Coconut">Coconut</option>
            <option value="mango">Mango</option>
          </select>
        </label>
        {/* 只有 type 为 submit 的时候 才能触发 提交 */}
        <input type="submit" value="提交文本显示" />
      </form>
    );
  }
}

```

## 第九章 React 理念

使用 React 创建一个可搜索的产品数据表格，展示我们的思考过程。

### 9.1 第零步：从模拟页面开始（思考）

想象我们已经有一个 JSON 接口和一个设计师给我们的原型图。

![可搜索的产品数据表格原型图](https://raw.githubusercontent.com/514723273/.md-Pictures/master/20190603175652.png)

```js
[
  {category: "Sporting Goods", price: "$49.99", stocked: true, name: "Football"},
  {category: "Sporting Goods", price: "$9.99", stocked: true, name: "Baseball"},
  {category: "Sporting Goods", price: "$29.99", stocked: false, name: "Basketball"},
  {category: "Electronics", price: "$99.99", stocked: true, name: "iPod Touch"},
  {category: "Electronics", price: "$399.99", stocked: false, name: "iPhone 5"},
  {category: "Electronics", price: "$199.99", stocked: true, name: "Nexus 7"}
];
```

### 9.2 第一步：把 UI 划分出组件层级（思考）

第一件你要做的事情是用方框划分出每一个组件(和子组件)并给他们命名。

![原型图划分层级](https://raw.githubusercontent.com/514723273/.md-Pictures/master/20190603180013.png)

1. **FilterableProductTable(橙色)** 包含了整个例子
2. **SearchBar(蓝色)** 接受所有用户输入
3. **ProductTable(绿色)** 根据用户输入过滤并展示数据集合
4. **ProductCategoryRow(绿松石色)** 展示每个分类的标题
5. **ProductRow** 用行来展示每个产品

现在我们已经确定了原型图中的组件，让我们把它们整理成层级结构。这很容易。原型图中的子组件在层级结构中应该作为子节点。

- FilterableProductTable
  - SearchBar
  - ProductTable
    - ProductCategoryRow
    - ProductRow

### 9.3 第二步：用 React 创建一个静态版本（代码）

See the Pen [Thinking In React: Step 2](https://codepen.io/lacker/pen/vXpAgj/) on CodePen.

现在有了组件层级，是时候去实现你的应用了。最简单的方式是先创建一个静态版本：传入数据模型，渲染 UI 但没有任何交互。

需要创建能够复用其他组件的组件，并通过 props 来传递数据。props 是一种从父级向子级传递数据的方法。如果你熟悉 state 的概念， 在创建静态版本的时候不要使用 state。State 只在交互的时候使用，即随时间变化的数据。由于这是静态版本的应用，你不需要使用它。（这些组件只会有 render() 方法）

你可以自顶向下或者自底向上构建应用。在较大的项目中，自底向上会更容易并且在你构建的时候有利于编写测试。

### 9.4 第三步：定义 UI 状态的最小（但完整）表示（思考）

React 使用 state，使你的 UI 交互，能够触发对底层数据模型的更改。

让我们来看看每一条，找出哪一个是 state。每个数据只要考虑三个问题：
- 它是通过 props 从父级传来的吗？如果是，他可能不是 state。
- 它随着时间推移不变吗？如果是，它可能不是 state。
- 你能够根据组件中任何其他的 state 或 props 把它计算出来吗？如果是，它不是 state。

想想我们的实例应用中所有数据。我们有：
- 原产品列表。（不是 state）（被作为 props 传入）
- 用户输入的搜索文本（是 state）（随时间改变并且不能由其他任何值计算出来。）
- 复选框的值（是 state）（随时间改变并且不能由其他任何值计算出来。）
- 产品的筛选列表（不是 state）（它可以通过将原始产品列表与搜索文本和复选框的值组合计算出来。）

最后，我们的 state 有：
- 用户输入的搜索文本
- 复选框的值

### 9.5 第四步：确定你的 State 应该位于哪里（代码）

See the Pen [Thinking In React: Step 4](https://codepen.io/lacker/pen/ORzEkG/) by Kevin Lacker (@lacker) on CodePen.

（第二步完成后，页面只是一个静态的，输入框中输入，勾选框中勾选，没有任何改变）

好的，现在我们确定了应用 state 的最小集合。接下来，我们需要确定哪个组件会改变，或拥有这个 state。

记住：React 中的数据流是单向的，并在组件层次结构中向下传递。

一开始我们可能不是很清楚哪个组件应该拥有哪个 state。在新手理解上这通常是最富有挑战性的部分，所以按照下面的步骤来辨别：

对你应用的每一个 state：
- 确定每一个需要这个 state 来渲染的组件。
- 找到一个公共所有者组件(一个在层级上高于所有其他需要这个 state 的组件的组件)
- 这个公共所有者组件或另一个层级更高的组件应该拥有这个 state。
- 如果你没有找到可以拥有这个 state 的组件，创建一个仅用来保存状态的组件并把它加入比这个公共所有者组件层级更高的地方。

让我们用这个策略分析我们的应用：
- ProductTable 需要根据 state 过滤产品列表，SearchBar 需要展示搜索文本和复选框状态。
- 公共所有者组件是 FilterableProductTable。
- 筛选文本和复选框的值应该放在 FilterableProductTable。

很酷，所以我们决定把 state 放在 FilterableProductTable。
1. 首先，为 FilterableProductTable 的 constructor 添加一个实例属性 this.state = {filterText: '', inStockOnly: false} 来表示我们应用的初始状态。
2. 接下来，把 filterText 和 inStockOnly 作为 prop 传入 ProductTable 和 SearchBar。
3. 最后在 ProductTable 中使用这些 props 来筛选每行产品信息，在 SearchBar 中设置表单域的值。

### 9.6 第五步：添加反向数据流（代码）

See the Pen [Thinking In React: Step 5](https://codepen.io/rohan10/pen/qRqmjd) on CodePen.

到目前为止，我们已经创建了一个可以正确渲染的应用程序，它的数据在层级中通过函数的 props 和 state 向下流动。现在是时候支持其他方式的数据流了：层级结构中最底层的表单组件需要去更新在 FilterableProductTable 中的 state 。

（第四步完成后，在最近公共祖先 FilterableProductTable 上添加了 state ，使子组件 Product 、 SearchBar 可以根据 父组件传递的 props 改变自身显示（商品被筛选了、勾选框打勾），但此时数据只是从上向下，我们还不能控制输入框内的内容，也不能对勾选框进行勾选操作（要改变 父组件的 state））

在当前版本的示例中，**如果你试图键入或选中复选框，你会发现 React 会忽略你的输入**。这是故意的，因为我们把 input 的 value 属性设置为一直等于从 FilterableProductTable 传入的 state。

让我们想想我们想要做什么。我们想确保每当用户更改表单时，我们更新状态来反应用户输入。
- 因为组件应该只更新自己的状态， FilterableProductTable 会将一个回调函数传递给 SearchBar ，每当应该更新状态时，它就会触发。我们可以使用输入上的 onChange 事件来调用它。
- FilterableProductTable 传入的回调函数会调用 setState()，这时应用程序会被更新。

### 9.7 总结代码

```js
/**
* 0. data
**/
let PRODUCTS = [
    {category: 'Sporting Goods', price: '$49.99', stocked: true, name: 'Football'},
    {category: 'Sporting Goods', price: '$9.99', stocked: true, name: 'Baseball'},
    {category: 'Sporting Goods', price: '$29.99', stocked: false, name: 'Basketball'},
    {category: 'Electronics', price: '$99.99', stocked: true, name: 'iPod Touch'},
    {category: 'Electronics', price: '$399.99', stocked: false, name: 'iPhone 5'},
    {category: 'Electronics', price: '$199.99', stocked: true, name: 'Nexus 7'}
];

export default PRODUCTS;
```
```js
/**
* 1. FilterableProductTable
**/
import React, { Component } from 'react';
import SearchBar from './SearchBar';
import ProductTable from './ProductTable';

class FilterableProductTable extends Component {
    constructor(props) {
        super(props);
        /**
         * 所有有关 filterText & inStockOnly 都是 9.5 第四步 的补充代码
         */
        this.state = {
            filterText: '',
            inStockOnly: false
        }

        this.handleFilterTextInput = this.handleFilterTextInput.bind(this);
        this.handleInStockOnlyInput = this.handleInStockOnlyInput.bind(this);
    }
    render() {
        let products = this.props.products;
        let filterText = this.state.filterText;
        let inStockOnly = this.state.inStockOnly;
        return (
            <div>
                <SearchBar 
                    filterText={filterText} 
                    inStockOnly={inStockOnly}
                    handleFilterTextInput={this.handleFilterTextInput}
                    handleInStockOnlyInput={this.handleInStockOnlyInput}
                />
                <ProductTable 
                    products={products} 
                    filterText={filterText} 
                    inStockOnly={inStockOnly}
                />
            </div>
        )
    }

    /**
     * 所有有关 handleFilterTextInput & handleInStockOnlyInput 都是 9.6 第五步 的补充代码
     */
    handleFilterTextInput(filterText) {
        this.setState((prevState, props) => ({
            filterText: filterText
        }))
    }

    handleInStockOnlyInput(inStockOnly) {
        this.setState((prevState, props) => ({
            inStockOnly: inStockOnly
        }))
    }
}

export default FilterableProductTable;
```
```js
/**
* 2. SearchBar
**/
import React, { Component } from 'react';

class SearchBar extends Component {
    constructor(props) {
        super(props);

        this.handleFilterTextInputChange = this.handleFilterTextInputChange.bind(this);
        this.handleInStockOnlyInputChange = this.handleInStockOnlyInputChange.bind(this);
    }
    render() {
        /**
         * 所有有关 filterText & inStockOnly 都是 9.5 第四步 的补充代码
         */
        let inStockOnly = this.props.inStockOnly;
        let filterText = this.props.filterText;
        return (
            <form>
                {/* 只添加了 value 就不能随着输入的变化而变化了（因为值被初始值锁定了） */}
                <input type="text" placeholder="Search..." value={filterText} onChange={this.handleFilterTextInputChange} />
                <p>
                    {/* 注意是 checked 而不是 value */}
                    <input type="checkbox" checked={inStockOnly} onChange={this.handleInStockOnlyInputChange} />
                    {' '}
                    Only show products in stock
                </p>
            </form>
        );
    }

    /**
     * 所有有关 handleFilterTextInput & handleInStockOnlyInput 都是 9.6 第五步 的补充代码
     */
    handleFilterTextInputChange(e) {
        this.props.handleFilterTextInput(e.target.value);
    }

    handleInStockOnlyInputChange(e) {
        //注意是 checked
        this.props.handleInStockOnlyInput(e.target.checked);
    }
}

export default SearchBar;
```
```js
/**
* 3. ProductTable
**/
import React, { Component } from 'react';
import ProductCategoryRow from './ProductCategoryRow';
import ProductRow from './ProductRow';

class ProductTable extends Component {
    render() {
        let products = this.props.products;
        let filterText = this.props.filterText;
        let inStockOnly = this.props.inStockOnly;
        let rows = [];
        let lastCategory = null;
        products.forEach(item => {
            /**
             * 所有有关 filterText & inStockOnly 都是 9.5 第四步 的补充代码
             */
            //空字符串被包含于任何字符串
            if(!item.name.includes(filterText) || (inStockOnly && !item.stocked)) {
                return;
            }

            if(item.category !== lastCategory) {
                rows.push(<ProductCategoryRow key={item.category} category={item.category} />);
                lastCategory = item.category;
            }
            rows.push(<ProductRow key={item.name} product={item}/>);
        })
        return (
            <table>
                <thead>
                    <tr>
                        <th>Name</th>
                        <th>Price</th>
                    </tr>
                </thead>
                <tbody>
                    {rows}
                </tbody>
            </table>
        )
    }
}

export default ProductTable;
```
```js
/**
* 4. ProductCategoryRow
**/
import React, { Component } from 'react';

class ProductCategoryRow extends Component {
    render() {
        return (
            <tr>
                <th colSpan="2">
                    {this.props.category}
                </th>
            </tr>
        );
    }
}

export default ProductCategoryRow;
```
```js
/**
* 5. ProductRow
**/
import React, { Component } from 'react';

class ProductRow extends Component {
    render() {
        const stocked = this.props.product.stocked;
        let name = this.props.product.name;
        const price = this.props.product.price;
        //如果缺货，商品名标记为红色
        if(!stocked) {
            name = <span style={{color: 'red'}}>{name}</span>
        }
        return (
            <tr>
                <td>{name}</td>
                <td>{price}</td>
            </tr>
        )
    }
}

export default ProductRow;
```
```js
/**
* 6. index.js
**/
import React from 'react';
import ReactDOM from 'react-dom';
import FilterableProductTable from './FilterableProductTable';
import productsData from './data';

ReactDOM.render(<FilterableProductTable products={productsData}/>, document.getElementById('root'));

```

## 第十章 使用 PropTypes 进行类型检查

PropTypes 包含一整套验证器，可用于确保你接收的数据是有效的。当传入的 prop 值类型不正确时，JavaScript 控制台将会打印警告。**出于性能原因，propTypes 只在开发模式下进行检查。**

### 10.1 PropTypes

```js
import PropTypes from 'prop-types';

MyComponent.propTypes = {
  // 你可以将属性声明为 JS 原生类型，默认情况下
  // 这些属性都是可选的。
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,

  // 任何可被渲染的元素（包括数字、字符串、元素或数组）
  // (或 Fragment) 也包含这些类型。
  optionalNode: PropTypes.node,

  // 一个 React 元素。
  optionalElement: PropTypes.element,

  // 你也可以声明 prop 为类的实例，这里使用
  // JS 的 instanceof 操作符。
  optionalMessage: PropTypes.instanceOf(Message),

  // 你可以让你的 prop 只能是特定的值，指定它为
  // 枚举类型。
  optionalEnum: PropTypes.oneOf(['News', 'Photos']),

  // 一个对象可以是几种类型中的任意一个类型
  optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),

  // 可以指定一个数组由某一类型的元素组成
  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

  // 可以指定一个对象由某一类型的值组成
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),

  // 可以指定一个对象由特定的类型值组成
  optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number
  }),

  // 你可以在任何 PropTypes 属性后面加上 `isRequired` ，确保
  // 这个 prop 没有被提供时，会打印警告信息。
  requiredFunc: PropTypes.func.isRequired,

  // 任意类型的数据
  requiredAny: PropTypes.any.isRequired,

  // 你可以指定一个自定义验证器。它在验证失败时应返回一个 Error 对象。
  // 请不要使用 `console.warn` 或抛出异常，因为这在 `onOfType` 中不会起作用。
  customProp: function(props, propName, componentName) {
    if (!/matchme/.test(props[propName])) {
      return new Error(
        'Invalid prop `' + propName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  },

  // 你也可以提供一个自定义的 `arrayOf` 或 `objectOf` 验证器。
  // 它应该在验证失败时返回一个 Error 对象。
  // 验证器将验证数组或对象中的每个值。验证器的前两个参数
  // 第一个是数组或对象本身
  // 第二个是他们当前的键。
  customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop `' + propFullName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  })
};
```

### 10.2 默认 Prop 值

您可以通过配置特定的 defaultProps 属性来定义 props 的默认值：
```js
class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

// 指定 props 的默认值：
Greeting.defaultProps = {
  name: 'Stranger'
};

// 渲染出 "Hello, Stranger"：
ReactDOM.render(
  <Greeting />,
  document.getElementById('example')
);
```

## 第十一章 虚拟DOM

### 11.1 三种方式的演变与比较

#### 11.1.1 方式一

1. State 数据
2. JSX 模板
3. 数据 + 模板 结合，生成真实的DOM（页面显示的DOM），来显示
4. state 发生改变
5. 数据 + 模板 结合，生成真实的DOM，替换原始的DOM

缺陷：

第一次生成了一个完整的DOM片段

第二次生成了一个完整的DOM片段

第二次的DOM替换第一次的DOM，非常耗性能

#### 11.1.2 方式二

1. State 数据
2. JSX 模板
3. 数据 + 模板 结合，生成真实的DOM，来显示
4. State 发生改变
5. *数据 + 模板 结合，生成真实的DOM，（并不直接替换原始的DOM，存储在内存）*
6. *新的DOM（DocumentFragment）和原始的DOM做比对，找差异*
7. *找出input框发生的变化*
8. *只用新的DOM中的input元素，替换掉老的DOM中的input元素*

缺陷：

节省了DOM替换的性能，但是损耗了DOM对比的性能，虽然性能有所提升，但是不大。

#### 11.1.3 方式三

1. State 数据
2. JSX 模板
3. 数据 + 模板 结合，生成虚拟DOM（**虚拟DOM就是一个JS对象，用它来描述真实的DOM**），如下
```js
[标签，属性对象，内容]
['div', {id: 'abc'}, ['span', {}, 'hello world']]
```
4. 用虚拟DOM的结构生成真实的DOM，来显示
```js
<div id = 'abc'>
  <span>
    Hello world!
  </span>
</div>
```
5. state 发生变化
6. 数据 + 模板 生成新的虚拟DOM（极大的提升了性能）（此时有两份虚拟DOM）
['div', {id: 'abc'}, ['span', {}, 'bye bye']]
7. 比较原始虚拟DOM和新的虚拟DOM的区别，找到区别是span的内容（极大的提升了性能）
8. 直接操作DOM，改变span中的内容

优点：

节省了真实DOM的创建，节省了真实DOM的对比，取而代之，是创建了JS对象，对比的也是JS对象。
（**JS比较JS对象不怎么消耗性能，但是操作真实的DOM十分消耗性能**）

### 11.2 深入了解虚拟 DOM

例如
```js
return <div>item</div>
```

中间的<div></div>只是一个JSX模板，并不是真实的DOM，也不是虚拟DOM。

JSX -> JS 对象（虚拟 DOM） -> 真实的 DOM

`return React.createElement('div', {}, item);` 这是一个更加底层的实现，效果一样。

也就是说即使没有JSX语法，也能实现虚拟DOM，只是比较麻烦。

再例如
```js
return <div><span>item</span></div>
//等同于
return React.createElement('div', {}, React.createElement('span', {}, item))
```
再深入可得，JSX -> *createElement* -> JS 对象（虚拟 DOM）-> 真实的 DOM

### 11.3 虚拟 DOM 的优点

1. 性能提升了， DOM 的比对变为 JS 对象的比对。
2. 它使得跨端应用得以实现。React Native。

（在浏览器中渲染真实的DOM没有问题，但是IOS、Android是不存在DOM这个概念的，无法被使用！但是转化为虚拟DOM，一个JS对象，可以在各平台被识别，在浏览器渲染真实的DOM，在移动平台渲染各种原生组件。）

### 11.4 虚拟 DOM 中的 Diff 算法

如何比较两个虚拟DOM的内容就涉及到Diff算法。

#### 11.4.1 Diff 的时机

![](https://raw.githubusercontent.com/514723273/.md-Pictures/master/20190604155235.png)

虚拟DOM的比对是在数据发生了变化之后。state改变，（props的改变实则父组件的state的改变），所以都是先调用了`setState`方法。

setState 是异步的，为了提升性能。比如连续调用三次 setState ，比对三次，更新三次，这样比较浪费性能，所以在极短的时间，**被合并为一次 setState**，一次比对虚拟DOM一次更新真实的DOM，省去额外两次。

#### 11.4.2 Diff 同级比较思想

![](https://raw.githubusercontent.com/514723273/.md-Pictures/master/20190604155508.png)

同级比对，如果在节点比较就发现不同的话，下面就不会继续比较了，直接全部重新生成替换所有子节点。（虽然这样看上去很浪费性能，但是算法简单，提升效率。）

#### 11.4.3 Key 值的重要性

![](https://raw.githubusercontent.com/514723273/.md-Pictures/master/20190604160604.png)

例如一个列表（如图）：
- 第一种情况：没有 Key 值，当数据发生改变，生成新的虚拟 DOM （第二排的小圆球），使用 Diff 算法与原虚拟 DOM 对比的时候，因为没有名字，不能很快地将其一一对应。（类似使用两层循环的比较，使其一一对应，十分消耗性能）。
- 第二种情况：当每项都拥有 Key 时，拥有自己的名字，能很快地建立联系，一一对比，找出不同。提高了虚拟DOM的比较性能。

*再提个问题，之前为什么不能用数组 Index 做 Key 值？*

不能保证原始的虚拟DOM和新的虚拟DOM 的 key 值一致了。

比如原始数组元素 [a, b, c] ， Index 分别是 0, 1, 2 。

当删去了 a ， [b, c] 的 Index 就变成了 0, 1 。因为数据变化（ a 删去），所以生成新的虚拟 DOM ，此时生成的 DOM 的 Key 值是按 0, 1 生成的，与原来的虚拟 DOM 中的 1, 2 不一致了，无法直接建立联系了，失去了 Key 的意义。

所以要使用稳定的值作为 Key 值，例如 唯一 id 。

## 第十二章 Refs and the DOM

Refs 提供了一种方式，允许我们访问 (DOM 节点或在 render 方法中创建的) React 元素。

在典型的 React 数据流中，props 是父组件与子组件交互的唯一方式。要修改一个子组件，你需要使用新的 props 来重新渲染它。*但是，在某些情况下，你需要在典型数据流之外强制修改子组件。*

被修改的子组件可能是一个 *React 组件的实例*，也可能是一个 *DOM 元素*。对于这两种情况，React 都提供了解决办法。

### 12.1 何时使用 Refs

下面是几个适合使用 refs 的情况：

- 管理焦点，文本选择或媒体播放。
- 触发强制动画。
- 集成第三方 DOM 库。

勿过度使用 Refs ，优先考虑 state 位置放置是否正确。

### 12.2 Refs 的使用

ref 的值根据节点的类型而有所不同：
- 当 ref 属性用于 HTML 元素时，构造函数中使用 React.createRef() 创建的 ref 接收底层 DOM 元素作为其 current 属性。（ [12.2.1 为 DOM 元素添加 ref](####12.2.1为DOM元素添加ref) ）
- 当 ref 属性用于自定义 class 组件时，ref 对象接收组件的挂载实例作为其 current 属性。（[12.2.2 为 class 组件添加 Ref](####12.2.2为class组件添加Ref)）
- 你不能在函数组件上使用 ref 属性，因为他们没有实例。

#### 12.2.1 为 DOM 元素添加 ref

```js
class CustomTextInput extends React.Component {

  constructor(props) {
    super(props);
    /**
     * Refs 是使用 React.createRef() 创建的，并通过 ref 属性附加到 React 元素。在构造组件时，
     * 通常将 Refs 分配给实例属性，以便可以在整个组件中引用它们。
    **/
   //1. 创建一个 ref 来存储 textInput 的 DOM 元素
    this.textInput = React.createRef();
  }

  render() {
    return (
      <div>
        <input 
          type="text"
          {/*2. 告诉 React 我们想把 <input> ref 关联到构造器里创建的 'textInput' 上*/}
          {/*在这里才完成了 this.textInput 赋值！*/}
          ref={this.textInput}
        />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    )
  }

  focusTextInput() {
    // 3. 直接使用原生 API 使 text 输入框获得焦点
    // 注意：我们通过 "current" 属性 来访问 ** DOM 节点 **
    this.textInput.current.focus();
  }
}
```
React 会在组件挂载时给 current 属性传入 DOM 元素，并在组件卸载时传入 null 值。ref 会在 componentDidMount 或 componentDidUpdate 生命周期钩子触发前更新。

#### 12.2.2 为 class 组件添加 Ref
```js
class AutoFocusTextInput extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  }

  componentDidMount() {
    this.textInput.current.focusTextInput();
  }

  render() {
    return(
      {/*请注意，这仅在 CustomTextInput 声明为 class 时才有效*/}
      <CustomTextInput ref={this.textInput} />
    )
  }
}
```

#### 12.2.3 Refs 与函数组件

```js
function MyFunctionComponent() {
  return <input />;
}

class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  }
  render() {
    // 你不能在函数组件上使用 ref 属性，因为它们没有实例。（也就没有属性？
    return (
      <MyFunctionComponent ref={this.textInput} />
    );
  }
}
```

如果你需要使用 ref，你应该将组件转化为一个 class，就像当你需要使用生命周期钩子或 state 时一样。

### 12.3 回调 Refs

“回调 refs”，这是 React 另一种设置 refs 的方式。它能助你更精细地控制何时 refs 被设置和解除。

```js
//React 将在组件挂载时，会调用 ref 回调函数并传入 DOM 元素，当卸载时调用它并传入 null。在 componentDidMount 或 componentDidUpdate 触发前，React 会保证 refs 一定是最新的。
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);

    this.textInput = null;
    /**
     * 不同于传递 createRef() 创建的 ref 属性，你会传递一个函数。这个函数中接受 React 组件实例或 HTML DOM 元素作为参数，以使它们能在其他地方被存储和访问。
    **/
    this.setTextInputRef = element => {
      this.textInput = element;
    }

    this.focusTextInput = () => {
      // 使用原生 DOM API 使 text 输入框获得焦点
      if(this.textInput) {
        this.textInput.focus;
      }
    }
  }

  componentDidMount() {
    // 组件挂载后，让文本框自动获得焦点
    this.focusTextInput();
  }

  render() {
    return (
      <div>
        <input
          type="text"
          {/*使用 `ref` 的回调函数将 text 输入框 DOM 节点的引用存储到 React 实例上（比如 this.textInput）*/}
          {/*传递的是函数*/}
          ref={this.setTextInputRef}
        />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    )
  }
}
```

```js
//你可以在组件间传递回调形式的 refs，就像你可以传递通过 React.createRef() 创建的对象 refs 一样。
function CustomTextInput(props) {
  return (
    <div>
    {/*传递过来函数*/}
      <input ref={props.inputRef} />
    </div>
  );
}

class Parent extends React.Component {
  render() {
    return (
      <CustomTextInput
        inputRef={el => this.inputElement = el}
      />
    );
  }
}
```