# React16.4开发简书项目

## 第一章 React 开发环境准备

### 1.1 基本步骤

1. [Node 官网](https://nodejs.org/en/download/) 下载node，其中会顺带下载好npm
2. [React 官方文档](https://reactjs.org/docs/getting-started.html) 查阅资料
3. `npm install -g create-react-app` 下载React脚手架工具
4. `create-react-app todolist` 利用脚手架工具，新建一个React项目
5. `cd todolist`
6. `npm start` 开启React初始项目

[中途出现的问题及解决方案](https://github.com/facebook/create-react-app/issues/6985)
```
create-react-app my-app
cd my-app
npm install react-scripts@2.1.8     //降级为 react scripts@2.1.8 解决了这个问题，而不需要处理环境变量
npm start
```

### 1.2 工具推荐

1. React Chrome 插件[React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?utm_source=chrome-ntp-icon)（开发环境下查看组件变化）
2. Redux Chrome 插件[Redux DevTools](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?utm_source=chrome-ntp-icon)
3. 代理服务器[Charles](https://www.charlesproxy.com/)（本地 mock 数据）

### 1.3 常用模块

1. 安装 [react-transition-group](https://github.com/reactjs/react-transition-group) `npm install react-transition-group --save`（动画）
2. 安装 [axios](https://github.com/axios/axios)  `npm install axios`（Ajax 请求）
3. 安装 [Ant Design of React](https://ant.design/index-cn) `npm install antd --save`
4. 安装 [Redux](https://redux.js.org/) `npm install --save redux`


## 第二章 React 生命周期函数

生命周期函数指在某一时刻组件**会自动调用执行**的函数

![](https://raw.githubusercontent.com/514723273/.md-Pictures/master/20190604203050.png)

### 2.1 完整生命周期

#### 2.1.1 Initialization 初始化

设置props和state，这是在constructor中完成的。

#### 2.1.2 Mounting 挂载

- componentWillMount 在组件**即将**被挂载到页面的时刻自动执行
- render 页面挂载执行
- componentDidMount 挂载结束后执行

#### 2.1.3 Updation 组件更新

1. props 独有（多一个）：
- componentWillReceiveProps 当一个组件从*父组件**接受参数*，如果这个组件第一次存在于父组件中，不会执行；如果这个组件之前已经存在于父组件中，才会执行。

2. props和state更新共有的函数：（就是 state 全部）
- shouldComponentUpdate 组件被更新前，自动执行，返回的是一个bool值。你这个组件需要更新吗？？？返回true需要，**返回false不更新，下面函数都不会执行**。
- componentWillUpdate 组件被更新前，它会被自动执行，但是他是在 shouldComponentUpdate 之后执行。
- render 重新渲染DOM。
- componentDidUpdate 件更新结束后执行。

#### 2.1.4 Unmount 卸载

- componentWillUnmount 当这个组件即将被从页面移除的时候，自动执行。（例如 todolist 删去其中一个 todo 项）

### 2.2 React生命周期函数的使用场景

#### 2.2.1 阻止无意义的渲染

在TodoList中，每次输入input框，父组件render，下面的TodoItem也会跟着被render一次。（参考 Diff 算法同级比较思想，找到一个节点不同，底下所有的子节点都替换）但是**TodoItem没有做任何改变，却要被无意义的渲染，这是浪费性能**。

所以使用`shouldComponentUpdate(nextProps, nextState) {}`函数来阻止这无意义的渲染。接受两个参数 nextProps 和 nextState 表示我接下来 props 和 state 会变成什么样。
```js
//通过控制判断内容是否被更改，来判断是否需要重新渲染。
shouldComponentUpdate(nextProps, nextState) {
	if(nextProps.content !== this.props.content) {
		return true;
	} else {
		return false;
	}
}
```

#### 2.2.2 Ajax 数据请求位置

Ajax请求该放在哪？

如果放在 render 函数内最前面，总是会在输入的时候，不停地调用 render ，无意义地向服务器请求数据，其实只要请求一次就好了。

在生命周期中，执行一次的可以放在 componentDidMount() （最合适！），挂载结束后获取。

```js
//用axios来发送ajax请求。
componentDidMount() {
	axios.get('/api/todolist')
	.then(res => {
    //在拿到返回数据结果后，再 setState 初始化列表，又会重新 render 一遍列表
    this.setState(() => ({
      list: [...res.data]
    }));
  })
	.catch(()=>{alert('error')})
}
```

## 第三章 虚拟 DOM

### 3.1 三种方式的演变与比较

#### 3.1.1 方式一

1. State 数据
2. JSX 模板
3. 数据 + 模板 结合，生成真实的DOM（页面显示的DOM），来显示
4. state 发生改变
5. 数据 + 模板 结合，生成真实的DOM，替换原始的DOM

缺陷：

第一次生成了一个完整的DOM片段

第二次生成了一个完整的DOM片段

第二次的DOM替换第一次的DOM，非常耗性能

#### 3.1.2 方式二

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

#### 3.1.3 方式三

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

### 3.2 深入了解虚拟 DOM

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

### 3.3 虚拟 DOM 的优点

1. 性能提升了， DOM 的比对变为 JS 对象的比对。
2. 它使得跨端应用得以实现。React Native。

（在浏览器中渲染真实的DOM没有问题，但是IOS、Android是不存在DOM这个概念的，无法被使用！但是转化为虚拟DOM，一个JS对象，可以在各平台被识别，在浏览器渲染真实的DOM，在移动平台渲染各种原生组件。）

### 3.4 虚拟 DOM 中的 Diff 算法

如何比较两个虚拟DOM的内容就涉及到Diff算法。

#### 3.4.1 Diff 的时机

![](https://raw.githubusercontent.com/514723273/.md-Pictures/master/20190605143116.png)

虚拟DOM的比对是在数据发生了变化之后。state改变，（props的改变实则父组件的state的改变），所以都是先调用了`setState`方法。

setState 是异步的，为了提升性能。比如连续调用三次 setState ，比对三次，更新三次，这样比较浪费性能，所以在极短的时间，**被合并为一次 setState**，一次比对虚拟DOM一次更新真实的DOM，省去额外两次。

#### 3.4.2 Diff 同级比较思想

![](https://raw.githubusercontent.com/514723273/.md-Pictures/master/20190604155508.png)

同级比对，如果在节点比较就发现不同的话，下面就不会继续比较了，直接全部重新生成替换所有子节点。（虽然这样看上去很浪费性能，但是算法简单，提升效率。）

#### 3.4.3 Key 值的重要性

![](https://raw.githubusercontent.com/514723273/.md-Pictures/master/20190604160604.png)

例如一个列表（如图）：
- 第一种情况：没有 Key 值，当数据发生改变，生成新的虚拟 DOM （第二排的小圆球），使用 Diff 算法与原虚拟 DOM 对比的时候，因为没有名字，不能很快地将其一一对应。（类似使用两层循环的比较，使其一一对应，十分消耗性能）。
- 第二种情况：当每项都拥有 Key 时，拥有自己的名字，能很快地建立联系，一一对比，找出不同。提高了虚拟DOM的比较性能。

*再提个问题，之前为什么不能用数组 Index 做 Key 值？*

不能保证原始的虚拟DOM和新的虚拟DOM 的 key 值一致了。

比如原始数组元素 [a, b, c] ， Index 分别是 0, 1, 2 。

当删去了 a ， [b, c] 的 Index 就变成了 0, 1 。因为数据变化（ a 删去），所以生成新的虚拟 DOM ，此时生成的 DOM 的 Key 值是按 0, 1 生成的，与原来的虚拟 DOM 中的 1, 2 不一致了，无法直接建立联系了，失去了 Key 的意义。

所以要使用稳定的值作为 Key 值，例如 唯一 id 。

## 第五章 Redux 入门

### 5.1 Redux 概念简述

![](https://raw.githubusercontent.com/514723273/.md-Pictures/master/20190608145440.png)

Redux就是把组件数据放进一个公共区域进行存储。

组件改变数据就不需要传递了，改变store里面的数据，其他组件就会感知到数据的改变，再去取就能取到新数据。所以无论层次多深，流程都是统一的。