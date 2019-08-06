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

### 2.6.2　PureRender

**1. PureRender 本质**

其原理为重新实现了 shouldComponentUpdate 生命周期方法，让当前传入的 props和 state 与之前的作**浅比较**，如果返回 false ，那么组件就不会执行 render 方法。

这里讲到了用 shouldComponentUpdate 来作性能优化的方法。在理想情况下，不考虑 props 和 state 的类型，那么要作到充分比较，只能通过深比较，但是它实在是太昂贵了
```js
shouldComponentUpdate(nextProps, nextState) {
  // 太昂贵了
  return isDeepEqual(this.props, nextProps) &&
    isDeepEqual(this.state, nextState);
}
```
{!!!}
然而，**PureRender 对 object 只作了引用比较，并没有作值比较。**对于实现来说，这是一个取舍问题。PureRender 源代码中只对新旧 props 作了浅比较。以下是 shallowEqual 的示例代码：
```js
function shallowEqual(obj, newObj) {
  if (obj === newObj) {
    return true;
  }

  const objKeys = Object.keys(obj);
  const newObjKeys = Object.keys(newObj);

  if (objKeys.length !== newObjKeys.length) {
    return false;
  }

  // 关键代码，只需关注 props 中每一个是否相等，无需深入判断
  return objKeys.every(key => {
    return newObj[key] === obj[key];
  });
}
```

**2. 运用 PureRender**

**3. 优化 PureRender**
- 直接为 props 设置对象或数组

  我们知道，每次调用 React 组件其实都会**重新创建组件**。就算传入的数组或对象的值没有改变，**它们引用的地址也会发生改变。**
  ```js
  // 这样设置 prop，则每次渲染时 style 都是新对象。
  <Account style={this.props.style || {}} />
  const defaultStyle = {};
  // 对于这样的赋值操作，我们只需要提前赋值成常量，不直接使用字面量即可。
  <Account style={this.props.style || defaultStyle} />
  ```
- 设置 props 方法并通过事件绑定在元素上
- 设置子组件

  对于设置了子组件的 React 组件，在调用 shouldComponentUpdate 时，均返回 true。

### 2.6.3　Immutable

在传递数据时，可以直接使用 Immutable Data 来进一步提升组件的渲染性能。

JavaScript 中的对象一般是可变的（mutable），因为使用了引用赋值，新的对象简单地引用了原始对象，改变新的对象将影响到原始对象。为了解决这个问题，一般的做法是使用浅拷贝（shallowCopy）或深拷贝（deepCopy）来避免被修改，但这样做又造成了 CPU 和内存的浪费。

**1. Immutable Data**

Immutable Data 就是一旦创建，就不能再更改的数据。对 Immutable 对象进行修改、添加或删除操作，都会*返回一个新的 Immutable 对象。*Immutable 实现的原理是**持久化的数据结构** （persistent data structure），也就是使用旧数据创建新数据时，要保证旧数据同时可用且不变。同时为了避免深拷贝把所有节点都复制一遍带来的性能损耗，Immutable 使用了**结构共享**（structural sharing），即如果对象树中一个节点发生变化，只修改这个节点和受它影响的父节点，其他节点则进行共享。

**2. Immutable 的优点**

- 降低了“可变”带来的复杂度：可变数据耦合了 time 和 value 的概念，造成了数据很难被回溯。
  ```js
  function touchAndLog(touchFn) {
    let data = { key: 'value' };
    touchFn(data);
    console.log(data.key);  // 在不查看 touchFn 的代码的情况下，因为不确定方法对 data 做了什么，我们是不可能知道结果是什么。但如果 data 是不可变的呢，你会很肯定地知道打印的结果是 value 。
  }
  ```
- 节省内存：Immutable 使用结构共享尽量复用内存。没有被引用的对象会被垃圾回收。
- 撤销/重做，复制/粘贴，甚至时间旅行这些功能做起来都是小菜一碟 。
- 并发安全 。（然而现在并没有用，因为 JavaScript 还是单线程运行的。
- 拥抱函数式编程 。（只要输入一致，输出必然一致，这样开发的组件更易于调试和组装。

**3. 使用 Immutable 的缺点**

容易与原生对象混淆是使用 Immutable 的过程中遇到的最大的问题。

虽然 Immutable 尽量把 API 设计的原生对象类似，但还是很难区分到底是 Immutable 对象还是原生对象。

**4. Immutable.is**

```js
let map1 = Immutable.Map({a:1, b:1, c:1});
let map2 = Immutable.Map({a:1, b:1, c:1});
map1 === map2; // => false
Immutable.is(map1, map2);  // => true
// Immutable.is 比较的是两个对象的 hashCode 或 valueOf （对于 JavaScript 对象）。
// 这样的算法避免了深度遍历比较，因此性能非常好。
```

**5. Immutable 与 cursor**

**6. Immutable 与 PureRender**

React 做性能优化时最常用的就是 shouldComponentUpdate 方法，**但它默认返回 true** ，即始终会执行 render 方法，然后做 Virtual DOM 比较，并得出是否需要做真实 DOM 的更新，这里往往会带来很多没必要的渲染。

当然，我们也可以在 shouldComponentUpdate 中使用**深拷贝和深比较**来避免无必要的 render ，但深拷贝和深比较一般都是非常昂贵的选择。

Immutable.js 则提供了简洁、高效的判断数据是否变化的方法，只需 === 和 is 比较就能知道是否需要执行 render ，而这个操作几乎零成本，所以可以极大提高性能。

修改后的shouldComponentUpdate 是这样的：

```js
import React, { Component } from 'react';
import { is } from 'immutable';

class App extends Component {
  shouldComponentUpdate(nextProps, nextState) {
    const thisProps = this.props || {};
    const thisState = this.state || {};

    if (Object.keys(thisProps).length !== Object.keys(nextProps).length ||
        Object.keys(thisState).length !== Object.keys(nextState).length) {
      return true;
    }

    for (const key in nextProps) {
      if (nextProps.hasOwnProperty(key) && 
          !is(thisProps[key], nextProps[key])) {
        return true;
      }
    }

    for (const key in nextState) {
      if (nextState.hasOwnProperty(key) &&
         !is(thisState[key], nextState[key])) {
        return true;
      }
    }

    return false;
  }
}

```

使用 Immutable 后，当灰色节点的 state 变化后，不会再渲染树中的所有节点，而是只渲染图右侧灰色的部分：
![Immutable渲染](https://raw.githubusercontent.com/514723273/.md-Pictures/master/Immutable渲染.png)

**7. Immutable 与 setState**

React 建议把 this.state 当作不可变的，因此修改前需要做一个深拷贝。

### 2.6.4　key

### 2.6.5　react-addons-perf

react-addons-perf 是官方提供的插件。通过 Perf.start() 和 Perf.stop() 两个 API 设置开始和结束的状态来作分析。它会把各组件渲染的各个阶段的时间统计出来，然后打印出一张表格。

## 2.7　动画

但是对于大多数情况而言，使用原生 CSS 动画，流程比较烦琐。首先，我们要给 DOM 节点在不同状态下加不同的复杂的 className 。然后在 CSS 中给不同的 className 写不同的样式以及动画逻辑。这里就有必要用 JavaScript 做一些包装，来做一些共同的逻辑，简化动画的开发。

待看

## 2.8　自动化测试

### 2.8.1　Jest

### 2.8.2　Enzyme

### 2.8.3　自动化测试

## 2.9　组件化实例：优化 Tabs 组件

# 第 3 章　解读 React 源码