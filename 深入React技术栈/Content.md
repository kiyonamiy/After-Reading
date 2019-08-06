# 第 1 章　初入 React 世界

## 1.1　React 简介

### 1.1.1　专注视图层

和 Angular、Ember 等框架不同，React 并不是完整的 MVC/MVVM 框架，它专注于提供清晰、简洁的 View（视图）层解决方案。

而又与模板引擎不同，React 不仅专注于解决 View 层的问题，又是一个包括 View 和 Controller 的库。对于复杂的应用，可以根据应用场景自行选择业务层框架，并根据需要搭配 Flux、Redux、GraphQL/Relay 来使用。

### 1.1.2　Virtual DOM

DOM 操作非常昂贵。我们都知道在前端开发中，性能消耗最大的就是 DOM 操作，而且这部分代码会让整体项目的代码变得难以维护。

React 把真实 DOM 树转换成 JavaScript 对象树，也就是 Virtual DOM。（传统：直接手动操作 DOM 更新）

它最大的好处其实还在于方便和其他平台集成，比如 react-native 是基于 Virtual DOM 渲染出原生控件，因为 React 组件可以映射为对应的原生控件。

### 1.1.3　函数式编程

## 1.2　JSX 语法

### 1.2.1　JSX 的由来

JSX 与 React 有什么关系呢？简单来讲，React 为方便 View 层组件化，承载了构建 HTML 结构化页面的职责。

**虚拟元素**可以理解为真实元素的对应，它的构建与更新都是在内存中完成的，并不会真正渲染到 DOM 中去。

在 React 中创建的虚拟元素可以分为两类，DOM 元素（DOM element）与组件元素（component element），分别对应着**原生 DOM 元素**与**自定义元素**，而 JSX 与创建元素的过程有着莫大的关联。

因为用 `React.createElement()` 写对象（一层套一层），十分复杂。所以使用了 JSX 语法以类似书写 HTML 一样书写复杂的组件。

```js
const DeleteAccount = () => (
  <div>
    <p>Are you sure?</p>
    <DangerButton>Confirm</DangerButton>
    <Button color="blue">Cancel</Button>
  </div>
);
```
通过 babel 转换：
```js
var DeleteAccount = function DeleteAccount() {
  return React.createElement(
    'div',
    null,
    React.createElement(
      'p',
      null,
      'Are you sure?'
    ),
    React.createElement(
      DangerButton,
      null,
      'Confirm'
    ),
    React.createElement(
      Button,
      { color: 'blue' },
      'Cancel'
    )
  );
};

```

### 1.2.2　JSX 基本语法

**1. XML 基本语法**

- 定义标签时，只允许被一个标签包裹（最外层）。
- 标签一定要闭合。

**2. 元素类型**

- DOM 元素：HTML 标签首字母小写
- 组件元素：HTML 标签首字母**大写**

**3. 元素属性**

在 JSX 中，不论是 DOM 元素还是组件元素，它们都有属性。不同的是，**DOM 元素的属性**是标准规范属性，但有两个例外——class 和 for ，这是因为在 JavaScript 中这两个单词都是关键词。

而组**件元素的属性**是完全自定义的属性，也可以理解为实现组件所需要的参数。

**4. JavaScript 属性表达式**

属性值要使用表达式，只要用 {} 替换 "" 即可

**5. HTML 转义**

React 会将所有要显示到 DOM 的字符串转义，防止 XSS。

所以，如果 JSX 中含有转义后的实体字符，比如 &copy; （©），则最后 DOM 中不会正确显示，因为 React 自动把 &copy; 中的特殊字符转义了

## 1.3　React 组件

### 1.3.1　组件的演变

### 1.3.2　React 组件的构建

组件基本上由 3 个部分组成——属性（props）、状态（state）以及生命周期方法。

![React组件的组成](https://raw.githubusercontent.com/514723273/.md-Pictures/master/React组件的组成.png)

1. React 与 Web Components
2. React 组件的构建方法
    - React.createClass
    - ES6 classes
    - 无状态函数

## 1.4 React 数据流

在 React 中，数据是自顶向下单向流动的，即从父组件到子组件。这条原则让组件之间的关系变得简单且可预测。

state 与 props 是 React 组件中最重要的概念。

如果顶层组件初始化 props，那么 React 会向下遍历整棵组件树，重新尝试渲染所有相关的子组件。

而 state 只关心每个组件自己内部的状态，这些状态只能在组件内改变。把组件看成一个函数，那么它接受了 props 作为参数，内部由 state 作为函数的内部参数，返回一个 Virtual DOM 的实现。

### 1.4.1　state

### 1.4.2　props

**1. 子组件 prop**

在 React 中有一个重要且**内置**的 prop——children ，它代表组件的子组件集合。

**2. 组件 props**

**3. 用 function prop 与父组件通信**

**4. propTypes**

## 1.5　React 生命周期

React 组件的生命周期根据广义定义描述，可以分为挂载、渲染和卸载这几个阶段。当渲染后的组件需要更新时，我们会重新去渲染组件，直至卸载。

因此，我们可以把 React 生命周期分成*两类*：
- 当组件在挂载或卸载时；
-当组件接收新的数据时，即组件更新时。

### 1.5.1　挂载或卸载过程

**1. 组件的挂载**

组件挂载是最基本的过程，这个过程主要做组件状态的初始化。我们推荐以下面的例子为模板写初始化组件：

```js
import React, { Component, PropTypes } from 'react';

class App extends Component {
  static propTypes = {
    // ...
  };

  static defaultProps = {
    // ...
  };

  constructor(props) {
    super(props);

    this.state = {
      // ...
    };
  }

  componentWillMount() {
    // ...
    // 如果我们在 componentWillMount 中执行 setState 方法，会发生什么呢？
    // 组件会更新 state，但组件只渲染一次 。{!!!}
    // 因此，这是无意义的执行，初始化时的 state 都可以放在 this.state 。
  }

  componentDidMount() {
    // ...
    // 如果我们在 componentDidMount 中执行 setState 方法，又会发生什么呢？
    // 组件当然会再次更新，不过在初始化过程就渲染了两次组件，这并不是一件好事。
    // 但实际情况是，有一些场景不得不需要 setState ，比如计算组件的位置或宽高时，就不得不让组件先渲染，更新必要的信息后，再次渲染。
  }

  render() {
    return <div>This is a demo.</div>;
  }
}

```

**2. 组件的卸载**

组件卸载非常简单，**只有** componentWillUnmount 这一个卸载前状态：

```js
import React, { Component, PropTypes } from 'react';

class App extends Component {
  componentWillUnmount() {
    // ...
    // 在 componentWillUnmount 方法中，我们常常会执行一些清理方法，如事件回收或是清除定时器。
  }

  render() {
    return <div>This is a demo.</div>;
  }
}

```

### 1.5.2　数据更新过程

更新过程指的是父组件向下传递 props 或组件自身执行 setState 方法时发生的一系列更新动作。

```js
import React, { Component, PropTypes } from 'react';

class App extends Component {
  componentWillReceiveProps(nextProps) {
    // this.setState({})
  }

  shouldComponentUpdate(nextProps, nextState) {
    // return true;
  }

  componentWillUpdate(nextProps, nextState) {
    // ...
  }

  componentDidUpdate(prevProps, prevState) {
    // ...
  }

  render() {
    return <div>This is a demo.</div>;
  }
}
```
如果组件自身的 state 更新了，那么会依次执行 shouldComponentUpdate 、componentWillUpdate 、**render** 和 componentDidUpdate 。

{!!!} shouldComponentUpdate 是一个特别的方法，它接收需要更新的 props 和 state，让开发者增加必要的条件判断，让其在需要时更新，不需要时不更新。因此，当方法返回 false 的时候，组件不再向下执行生命周期方法。

**1.5.3　整体流程**

{!!!}

![React生命周期整体流程图](https://raw.githubusercontent.com/514723273/.md-Pictures/master/React生命周期整体流程图.png)

## 1.6　React 与 DOM

在 React 组件的开发实现中，我们并不会用到 ReactDOM，只有在顶层组件以及由于 React 模型所限而不得不操作 DOM 的时候，才会使用它。

### 1.6.1　ReactDOM

ReactDOM 中的 API 非常少，只有 findDOMNode 、unmountComponentAtNode 和 render 。下面我们就从 API 的角度来讲讲它们的用法。

**1. findDOMNode**

上一节我们已经讲过组件的生命周期，DOM 真正被添加到 HTML 中的生命周期方法是 componentDidMount 和 componentDidUpdate 方法。

在这两个方法中，我们可以获取真正的 DOM 元素。React 提供的获取 DOM 元素的方法有两种，其中一种就是 ReactDOM 提供的 findDOMNode ：
```js
import React, { Component } from 'react';
import ReactDOM from 'react-dom';

class App extends Component {
  componentDidMount() {
    // this 为当前组件的实例
    const dom = ReactDOM.findDOMNode(this);
  }

  render() {}
}
```

**2. render**

为什么说只有在顶层组件我们才不得不使用 ReactDOM 呢？这是因为要把 React 渲染的 Virtual DOM 渲染到浏览器的 DOM 当中，就要使用 render 方法了：
```js
ReactComponent render(
  ReactElement element,
  DOMElement container,
  [function callback]
)
```

当组件在初次渲染之后再次更新时，React 不会把整个组件重新渲染一次，而会用它高效的 DOM diff 算法做局部的更新。这也是 React 最大的亮点之一！

### 1.6.2　ReactDOM 的不稳定方法

- unstable_renderSubtreeIntoContainer
- unstable_batchedUpdates

### 1.6.3 refs

组件被调用时会新建一个该组件的实例，而 refs 就会指向这个实例。

它可以是一个回调函数，这个回调函数会在组件被挂载后立即执行。`<input type="text" ref={(ref) => this.myTextInput = ref} />`

refs 同样支持字符串。对于 DOM 操作，不仅可以使用 findDOMNode 获得该组件 DOM，还可以使用 refs 获得组件内部的 DOM。
```js
import React, { Component } from 'react';

class App extends Component {
  componentDidMount() {
    // myComp 是 Comp 的一个实例，因此需要用 findDOMNode 转换为相应的 DOM
    const myComp = this.refs.myComp;
    const dom = findDOMNode(myComp);
  }

  render() {
    return (
      <div>
        <Comp ref="myComp" />
      </div>
    );
  }
}
```

### 1.6.4　React 之外的 DOM 操作

## 1.7　组件化实例：Tabs 组件

# 第 2 章　漫谈 React

## 2.1　事件系统

### 2.1.1　合成事件的绑定方式

### 2.1.2　合成事件的实现机制

在 React 底层，主要对合成事件做了两件事：事件委派和自动绑定。

**1. 事件委派**

它并不会把事件处理函数直接绑定到真实的节点上，而是把所有事件绑定到结构的最外层，使用一个统一的事件监听器，这个事件监听器上维持了一个映射来保存所有组件内部的事件监听和处理函数。

**2. 自动绑定**
- bind 方法
- 构造器内声明
- 箭头函数

### 2.1.3　在React中使用原生事件

React 提供了完备的生命周期方法，其中 componentDidMount 会在组件已经完成安装并且在浏览器中存在真实的 DOM 后调用，此时我们就可以完成原生事件的绑定。

```js
import React, { Component } from 'react';

class NativeEventDemo extends Component {
  componentDidMount() {
    this.refs.button.addEventListener('click', e => {
      this.handleClick(e);
    });
  }

  handleClick(e) {
    console.log(e);
  }

  // 在 React 中使用 DOM 原生事件时，一定要在组件卸载时手动移除，否则很可能出现内存泄漏的问题。
  componentWillUnmount() {
    this.refs.button.removeEventListener('click');
  }

  render() {
    return <button ref="button">Test</button>;
  }
}

```

### 2.1.4　合成事件与原生事件混用

### 2.1.5　对比React合成事件与JavaScript原生事件

1. 事件传播与阻止事件传播
2. 事件类型
3. 事件绑定方式
4. 事件对象

## 2.2　表单

### 2.2.1　应用表单组件

1. 文本框
2. 单选按钮与复选框
3. Select 组件

### 2.2.2　受控组件

每当表单的状态发生变化时，都会被写入到组件的 state 中，这种组件在 React 中被称为受控组件（controlled component）。

### 2.2.3　非受控组件

简单地说，如果一个表单组件没有 value props（单选按钮和复选框对应的是 checked prop）时，就可以称为非受控组件。

### 2.2.4　对比受控组件和非受控组件

### 2.2.5　表单组件的几个重要属性

## 2.3　样式处理

## 2.4　组件间通信

### 2.4.1　父组件向子组件通信

### 2.4.2　子组件向父组件通信

- 利用回调函数
- 利用自定义事件机制

### 2.4.3　跨级组件通信

层层传递 props，但此时的代码显得不那么优雅，甚至有些冗余。

在 React 中，我们还可以使用 context 来实现跨级父子组件间的通信

### 2.4.4　没有嵌套关系的组件通信

没有嵌套关系的，那只能通过可以影响全局的一些机制去考虑。

自定义事件机制不失为一种上佳的方法。

## 2.5　组件间抽象

重点讨论两种：mixin 和高阶组件。

### 2.5.1　mixin

看到上述实现，是否会联想到 underscore 库中的 extend 或 lodash 库中的 assign 方法，或者说 ES6中的 Object.assign() 方法？它的作用是什么呢？MDN 上的解释是把任意多个源对象所拥有的自身可枚举属性复制给目标对象，然后返回目标对象

### 2.5.2　高阶组件

higher-order function（高阶函数）在函数式编程中是一个基本的概念，它描述的是这样一种函数：这种函数接受函数作为输入，或是输出一个函数。比如，常用的工具方法 map 、reduce 和 sort 等都是高阶函数。

（一层套一层

实现高阶组件的方法有如下两种：
- 属性代理 
- 反向继承

### 2.5.3　组合式组件开发实践

1. 组件再分离
2. 逻辑再抽象

## 2.6　组件性能优化

### 2.6.1　纯函数

- 给定相同的输入，它总是返回相同的输出
- 过程没有副作用（side effect）（不能改变外部状态）
- 没有额外的状态依赖