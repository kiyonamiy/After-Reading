# React16.4开发简书项目

## 第一章 React 开发环境准备

### 1.1 基本步骤

1. [Node 官网](https://nodejs.org/en/download/) 下载node，其中会顺带下载好npm
2. [React 官方文档](https://reactjs.org/docs/getting-started.html) 查阅资料
3. `npm install -g create-react-app` 下载React脚手架工具
4. `create-react-app todolist` 利用脚手架工具，新建一个React项目
5. `cd todolist`
6. `npm start` 开启React初始项目

### 1.2 工具推荐

1. React Chrome 插件[React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?utm_source=chrome-ntp-icon)（开发环境下查看组件变化）
2. Redux Chrome 插件[Redux DevTools](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?utm_source=chrome-ntp-icon)
3. 代理服务器[Charles](https://www.charlesproxy.com/)（本地 mock 数据）

### 1.3 常用模块

1. 安装 [react-transition-group](https://github.com/reactjs/react-transition-group) `npm install react-transition-group --save`（动画）
2. 安装 [axios](https://github.com/axios/axios)  `npm install axios`（Ajax 请求）
3. 安装 [Ant Design of React](https://ant.design/index-cn) `npm install antd --save`
4. 安装 [Redux](https://redux.js.org/) `npm install --save redux`
5. 安装 [Mock.js](https://github.com/nuysoft/Mock) `npm install mockjs` （Mock 数据测试）
5. 安装 [Redux Thunk](https://github.com/reduxjs/redux-thunk) `npm install redux-thunk`
6. 安装 [Redux Saga](https://github.com/redux-saga/redux-saga) `npm install --save redux-saga`
7. 安装 [React Redux](https://github.com/reduxjs/react-redux) `npm install --save react-redux`
8. 安装 [styled-components](https://github.com/styled-components/styled-components) `npm install styled-components`（使 CSS 模块化）
9. 安装 [immutable](https://github.com/immutable-js/immutable-js) `npm install immutable`（封装 state ，确保不改变）
### 1.4 个别工具使用说明

#### 1.4.1 Mock.js 基础使用

```js
// mock/index.js
import Mock from 'mockjs';

import getListData from './get/api_get_list';
import getTestData from './get/api_get_test';

export default Mock.mock('/api/get/list', 'get', getListData)
.mock('/api/get/test', 'get', getTestData);

// mock/get/api_get_test
export default {
    name: 'Kiyonami',
    age: 20
}

//最后在 src 下的 index.js 引入即可！（一处引用就可以！）
import './mock';
```

#### 1.4.2 Redux Thunk 配置

参照 [Exercise-Project/todos/src/store/index.js](https://github.com/514723273/Exercise-Project/blob/master/todos/src/store/index.js) 和 [Redux DevTools Extension](https://github.com/zalmoxisus/redux-devtools-extension)

#### 1.4.3 iconfont 网站使用

1. 登录账号，新建项目
2. 搜索想要的图标，点击购物车，添加至项目
3. 下载到本地
4. 解压，选择五个文件放入项目
  - iconfont.css
  - iconfont.eot
  - iconfont.svg
  - iconfont.ttf
  - iconfont.woff

#### 1.4.4 ReduxDevTools 插件在代码中的配置

```js
import { createStore, /*applyMiddleware,*/ compose } from 'redux';
import reducer from './reducer';

// import createSagaMiddleware from 'redux-saga';
// import sagas from './sagas';

// const sagaMiddleware = createSagaMiddleware();

const composeEnhancers =
  typeof window === 'object' &&
  window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ?   
    window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({}) : compose;

const enhancer = composeEnhancers(
    // applyMiddleware(sagaMiddleware),
);

//第二个参数是为了配置浏览器 redux 扩展
const store = createStore(reducer, enhancer);
//Before running a Saga, you must mount the Saga middleware on the Store using applyMiddleware
//sagaMiddleware.run(sagas);

export default store;
```

### 1.5 问题

#### 1.5.1 npm start 启动报错

[中途出现的问题及解决方案](https://github.com/facebook/create-react-app/issues/6985)
```
create-react-app my-app
cd my-app
npm install react-scripts@2.1.8     //降级为 react scripts@2.1.8 解决了这个问题，而不需要处理环境变量
npm start
```

#### 1.5.2 create-react-app 慢

虽然平常使用cnpm来代替npm,但也只是使用新的指令而已，所以需要整体换源。

`npm config set registry https://registry.npm.taobao.org`

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

## 第四章 Redux 入门

### 4.1 Redux 概念简述

![](https://raw.githubusercontent.com/514723273/.md-Pictures/master/20190609202153.png)

Redux就是把组件数据放进一个公共区域进行存储。

组件改变数据就不需要传递了，改变store里面的数据，其他组件就会感知到数据的改变，再去取就能取到新数据。所以无论层次多深，流程都是统一的。

### 4.2 store 自己的理解

![store](https://raw.githubusercontent.com/514723273/.md-Pictures/master/store.png)

按步骤分析：

#### 4.2.1 新建 reducer 函数

参数为 (state, action)

#### 4.2.2 store = createStore(reducer)

 **将 reducer 置于内部，并且会马上执行一次 reducer**，此时会在内部存储一份 state 。
 
 此时执行 reducer 传入的参数分别为`state = defaultState, action = {type: "@@INIT"}`（此时是没有传入 action ，这个 action 为内部传入；defaultState 为默认值）

 （所以在组件的 constructor 中，第一次 this.state = store.getState() ，是将组件内的 state 指针指向 store 内部的 state）

#### 4.2.3 新建 action 对象

需要有 type 属性

#### 4.2.4 store.dispatch(action)

日常执行 reducer 函数，传递的参数为 store 内部的 state 和 此时作为参数的 action 。更新内部的 state 。

#### 4.2.5 store.subscribe(func)

注册 store 监听，当内部的 state 发生改变时，调用 func 。

#### 4.2.5 this.setState(store.getState())

一般是注册 store 监听时，组件传入的函数。`store.subscribe(() => {this.setState(store.getState())});`

将组件的 state 指针重新指向 store 内的新的 state 。（因为每次 store 内 state 变化是新拷贝一份旧的 state ，在拷贝后的做修改并返回）

### 4.3 UI 组件和容器组件

可以拆分为 UI 组件和容器组件：
- *UI 组件*负责组件渲染，内部一般只有一个 render 函数，使用无状态组件（函数组件）（性能高）来表示。
- *容器组件*负责数据交互，引用对应的容器组件将其渲染，将 UI 组件需要的内容通过属性传递（ UI 组件通过 props 接收）（引入 store ，通过 dispatch 传递参数，在 reducer 中逻辑处理。）

## 第五章 Redux Thunk and Redux Saga

### 5.1 Redux Thunk 中间件

如果遇到异步请求或者非常复杂的逻辑，我们希望把它移到其他地方进行统一处理，否则会使一个组件看起来过于臃肿。

action 中返回的可以不是一个对象了，可以是一个函数。

具体使用可以参考 [Exercise-Project/todos](Exercise-Project/todos/src/store/actionCreators.js)

### 5.2 什么是中间件

![Redux Data Flow](https://raw.githubusercontent.com/514723273/.md-Pictures/master/20190610074946.png)

中间是指 action 和 store 的中间。中间件是对 dispatch 做了一次升级，可以对传入的参数进行判断，例如是对象直接传入 store ，是函数先执行，再传入。
```js
// actionCreators.js
/**
 * redux thunk
 */
//使用 redux-thunk 后， action 可以是函数
export const getInitList = () => {
    //返回函数 （虽然可以再简写 不用 return 用小括号
    //虽然没有 store ，但是这个函数会接收到 dispatch 方法
    return (dispatch) => {
        axios.get('/api/get/list').then((res) => {
            const action = getInitListAction(res.data);   //返回的函数继续执行，还是会创建 action 传入 store
            dispatch(action);
        });
    }
}
```

### 5.3 Redux Saga 中间件

Redux Saga 和 Redux Thunk 用途一致，但流程不同：
- redux saga 和 redux thunk 的不同是，thunk 是通过传入的 action 是函数还是对象自动分配；
- 而 saga 不仅可以将 action 传入 reducer 也可以传入 saga。，我们先创建一个新的 action 对象，传入到 saga 中，再执行 InitList 的 action 。

```js
// saga.js
import { takeEvery, put } from 'redux-saga/effects';
import { GET_INIT_LIST } from './actionTypes';
import { getInitListAction } from './actionCreators';
import axios from 'axios';

function* getInitList() {
    try {
        const res = yield axios.get('/api/get/list');
        const action = getInitListAction(res.data); //此时又使用 初始化list的action
        yield put(action);
    } catch(e) {
        console.log('/api/get/list 网络请求失败');
    }

}

// 必须是 generator 函数
// 不仅可以在 reducer 接收到 action ，saga 也可以
function* saga() {
    yield takeEvery(GET_INIT_LIST, getInitList);
}

export default saga;
```

## 第六章 简书 Header 组件开发

### 6.1 styled-components 管理 css 文件

CSS 是一处引用，全局生效（`import 'index.css'`）。然而我们并不希望如此，会产生耦合，使不同文件的样式相互影响。

使用 styled-components 。使 css 模块化，单个组件引入一个样式使用，不会被其他文件的样式影响。

```js
//src/style.js

//视频中的injectGlobal被弃用了，换成createGlobalStyle
import { createGlobalStyle } from 'styled-components';

export const GlobalStyle = createGlobalStyle`
  body{
    margin: 0;
    background:green;
  }
`
```
```js
//然后在组件中引用自己需要的样式，比如在App组件中
import React, { Fragment } from 'react';
import { GlobalStyle } from './style'
function App() {
  return (
    <Fragment>
      <GlobalStyle />
    </Fragment>
  );
}
export default App;
```

### 6.2 创建 HeaderWrapper, Logo, Nav 组件

```js
//src/components/header/style.js

/* 就是一个 div 标签，带了样式 */
export const HeaderWrapper = styled.div`
    position: relative;
    height: 56px;
    border-bottom: 1px solid #f0f0f0;   /* Header 底下的一条细线 */
`;

export const Logo = styled.a.attrs({    /*设置属性，使其指向首页 */
    href: '/'
})`
    position: absolute;
    top: 0;
    left: 0;
    display: block;
    width: 100px;
    height: 56px;
    background: url(${logoPic});    /*多行文本嵌入变量的语法 \$\{\} */
    background-size: contain;       /* 图片太大或太小时，使其正好放入位置 */
`;

export const Nav = styled.div`
    width: 960px;
    height: 100%;               /* 若是不写，则是自适应，根据标签内容大小显示；若标签内无内容，则 height=0 不显示 */
    padding-right: 70px;
    box-sizing: border-box;     /*使其右边空出 70px, 防止和右边的登录写文章按钮重叠 */
    margin: 0 auto;             /* topbottom leftright 0上下填满 auto水平居中 */
`;
```

### 6.2 创建 NavItem, NavSearch, Button

```js
//src/components/header/style.js

export const NavItem = styled.div`
    line-height: 56px;  /*设置行高，不会改变字体大小，文字相对于line-height上下居中。此时line-height和height高度一致，所以相对于父元素上下居中*/
    padding: 0 15px;
    font-size: 17px;
    color: #333;
    padding-right: 70px;
    box-sizing: border-box;

    /* 如果 NavItem 组件的 className=left 或 className=right 的时候，执行对应的样式 */
    &.left {
        float: left;
    }
    &.right {
        float: right;
        color: #969696;
    }
    &.active {
        color: #ea6f5a;
    }
`;

export const NavSearch = styled.input.attrs({
    placeholder: '搜索'
})`
    width: 160px;               /* 这个属性定义元素!!!内容区!!!的宽度，在内容区外面可以增加内边距、边框和外边距。 */
    height: 38px;
    padding: 0 30px 0 20px;
    margin-top: 9px;            /* 整体高度=56px ， height=38px ， 为了其居中显示， (56-38) / 2 = 9px */
    margin-left: 20px;
    border: none;
    box-sizing: border-box;     /* 为元素指定的任何内边距和边框都将在!!!已设定的宽度和高度内!!!进行绘制。此时宽度是 160px，内边距左右 20px，所以内容宽度变为120px。如果有多的部分被隐藏*/
    outline: none;              /*（轮廓）是绘制于元素周围的一条线，位于 边框边缘 的外围，可起到突出元素的作用。不会占据空间。可设置 color style width */
    border-radius: 19px;        /* 圆角，高度的一半 */
    background: #eee;
    font-size: 14px;
    color: #666;

    &::placeholder {
        color: #999;
    }
`;

export const Addition = styled.div`
    position: absolute;
    right: 0;
    top: 0;
`;

export const Button = styled.div`
    float: right;
    margin-top: 9px;
    margin-right: 20px;
    padding: 0 20px;
    line-height: 38px;
    border-radius:  19px;
    border: 1px solid #ec6149;
    font-size: 14px;

    &.reg {
        color: #ec6149;
    }
    &.writting {
        color: #fff;
        background: #ec6149;
    }
`;
```

### 6.3 使用 iconfont 嵌入头部图标

使用 iconfont 搜索放大镜、羽毛笔、Aa 三个图标，下载解压，使用其中五个文件，放入 statics 文件夹下。

将其中的 CSS 文件修改为如下 JS 文件，再全局引入。

```js
import { createGlobalStyle } from 'styled-components';

const IconfontGlobalStyle = createGlobalStyle`
@font-face {
  font-family: "iconfont";
  src: url('./iconfont.eot?t=1560589255735'); /* IE9 */
  src: url('./iconfont.eot?t=1560589255735#iefix') format('embedded-opentype'), /* IE6-IE8 */
  url('data:application/x-font-woff2;charset=utf-8;base64,d09GMgABAAAAAAPEAAsAAAAAB8wAAAN2AAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAHEIGVgCDHAqDDIJvATYCJAMQCwoABCAFhG0HQhvbBhEVnGPIfha4eyeMONqRRgUY+tHt9oQgTEvQ90XwQHl+bzLTbn7yZS+Bk9UuoBOhk9zVqTufIAPUByve8j2epHbS68r1nvgYHkIDcACOaHL3LLG1xZhp85Lu/f+Pe8ctS2A+oFzmGOcfA4y3dYGNPUC6AAskQU9FupaeiMdqCMAglGREtRr1mqGjsDsJQPTr3bMzekGJ6sgU6AisDSOVk5iLBV2bpl1ixpzo++IjmdBBwyKxezbqUb0b5W983+TKsY6hXaBm5nJ6QBwHEkgGFIhejcYuyCSYjMTQ6ytyAR0dDZaqSHmT65jYVxHqzl8eSDQECCt2ujMA2RSS8MbXY2aiFMjRIMoF+wOkKwB0pAwHtmLjg7m1QiJ03cd0dnYLdTNaODcz+pqt2ywMNZb7t+p80JwBhjCOGdOviHEtW46LZi9uvmszY7eYM04K0fXOnRZ377a8fbv5rVsdXH6zmJ6/0Z07o6f7tlg4btsmn+b2JYtt3s1XfuxqX518qFEj2+6W9lvNbItzNt0OaPQyt+75284d61Hzeny1691dHbbqnT6lDZtrlV16l95Vf2rosNP5acJSkTowoUVvl/Zm3fLCVx1aDrsY4PMnMOZTTChXx9jlmdPS7tCI5kttWaytx2jr1mngjeHoUGprORq9ke9Lw2XK71/iHzl26p9vkXb+NVSvN2HikW/nHn2qVrf9zXYOycivN2dGcLjyt47QenXodq3tCP9hcbvTqsUkMAgAx3JlREw+kMVh8qAc+9/9hqJWdfZ0dCv+ppsKgPuJt87C7qAYJuIPYA1m/VPVVBSAkji2SilNQKdYDXehSM91RQMMDMABqydPlhD76Okg0PHJQKMKISDRiUYpXDJYMMkGKzplYJBEzfEmGS9iiVBOQKI3A4G7D2i4chQk7r4ohfuBBV9/sOKeJRi0iNeMJrGpD1UFjWIF9gW144FcjR7izCP6W2ckz4kIV5Sp9kAaJeVIiQPKEn2mu89UCUi4h4Kth13HMAo36DQKqmMex9T0pMhxPztQlUCGQhVgvYCawwYU5FaHyuePkHfTMaQDUae+QmJS1w5SkWQAqhQOgxCPcs7kzssoRQARrAcKpkedE2RgbG7WQI6KhAn+US6mbDRUGG0v7r/hPMDAPlwJTUihhAWtwuzrGz/UVKNsR996w7aezQAAAA==') format('woff2'),
  url('./iconfont.woff?t=1560589255735') format('woff'),
  url('./iconfont.ttf?t=1560589255735') format('truetype'), /* chrome, firefox, opera, Safari, Android, iOS 4.2+ */
  url('./iconfont.svg?t=1560589255735#iconfont') format('svg'); /* iOS 4.1- */
}

.iconfont {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
`;

export default IconfontGlobalStyle;
```

使用如下：

```html
<span className="iconfont">&#xe62d;</span>
```

### 6.4 搜索框动画效果实现

为搜索框重新布局，用 SearchWrapper 包裹 NavSearch 和 icon ，使放大镜能放在输入框内（本来二者分离，未重叠）。

```js
//src/components/header/style.js

export const SearchWrapper = styled.div`
    position: relative;
    float: left;

    /* 设置 SearchWrapper 底下的类为 iconfont */
    .iconfont {
        position: absolute;     /* !!!具体原理是 position: absolute; 的元素会相对于第一个设置了 position: relative;!!! */
        bottom: 5px;
        right: 5px;
        width: 30px;
        line-height: 30px;
        border-radius: 15px;
        text-align: center;     /* 规定元素中的文本的水平对齐方式 */
    }
`;
```

编写两个函数，handleNavSearchFocused 和 handleNavSearchBlur 通过控制 this.state.focused 的值，来展开和收缩搜索框。（此时没有动效，瞬间完成切换，比较生硬）

```js
<NavSearch 
    className={this.state.focused ? 'focused' : ''}
    onFocus={this.handleNavSearchFocus}
    onBlur={this.handleNavSearchBlur}
/>
<span 
    className={this.state.focused ? 'focused iconfont' : 'iconfont'}
>&#xe62d;</span>
```

```js
handleNavSearchFocus() {
    this.setState({
        focused: true
    })
}

handleNavSearchBlur() {
    this.setState({
        focused: false
    })
}
```

为展开和收缩搜索框添加动画，用 CSSTransition 包裹 NavSearch ，设置出入场开关和动画时长。（不包裹 icon ，因为 icon 的布局决定它能随着 NavSearch 改变而改变，且CSSTransition 只能包裹一个元素）

```js
<SearchWrapper>
    <CSSTransition
        in={this.state.focused}     /** 控制入场退场的开关 */
        timeout={200}               /** 动画时长(但好像还是内部的时长控制) */
        classNames="slide"
    >
        <NavSearch 
            className={this.state.focused ? 'focused' : ''}
            onFocus={this.handleNavSearchFocus}
            onBlur={this.handleNavSearchBlur}
        />
    </CSSTransition>
    <span 
        className={this.state.focused ? 'focused iconfont' : 'iconfont'}
    >&#xe62d;</span>
</SearchWrapper>
```

最后在 NavSearch 中 添加入场动画出场动画的 css 具体实现（各阶段的表现）（CSSTransition 干的事就是在各个阶段给其添加 className）
```css
.slide-enter {          /* 入场动画过程 */
    //width: 160px;
    transition: all .2s ease-out;   /* 与外层 timeout 保持一致 */
}

.slide-enter-active {   /* 入场动画结束后的显示效果（好像不写也行，因为有 className 在控制最终显示效果） */
    width: 240px;
}

.slide-exit {
    transition: all .2s ease-out;
}
```

### 6.5 引入 redux 和 react-redux

新建 src/store/reducer.js

```js
const defaultState = {};
export default (state = defaultState, action) => {
    return state;
}
```

新建 src/store/index.js。引入 reducer 创建 store

```js
import { createStore } from 'redux';
import reducer from './reducer';

const store = createStore(reducer);

export default store;
```

在App.js 引入 Provider，设置为最外层元素，传入属性 store={store}。Provider 会将 store 提供给了它的子组件。

```js
function App() {
  return (
    <Provider store={store}>
      <GlobalStyle />
      <IconfontGlobalStyle />
      <Header />
    </Provider>
  );
}
```

在组件中做连接 connect。`export default connect(mapStateToProps, mapDispatchToProps)(Header);` 为该组件添加属性和方法。

```js
const mapStateToProps = state => ({
    focused: state.focused,

});

const mapDispatchToProps = dispatch => ({
    handleNavSearchFocus() {
        const action = getNavSearchFocusAction();
        dispatch(action);
    },
    handleNavSearchBlur() {
        const action = getNavSearchBlurAction();
        dispatch(action);
    }
});

export default connect(mapStateToProps, mapDispatchToProps)(Header);
```

最后发现 Header 可以改为 无状态组件。
```js
const Header = props => { 
    return(
        //.......
    ); 
}
```

### 6.6 使用 combineReducers 完成对数据的拆分管理

1. 将 src/store/reducer.js 关于 header 的逻辑 移入新建的 src/common/header/store/reducer.js

2. 新建 src/common/header/store/index.js ，目的是为了 src/store/reducer.js 引入的路径减少。
```js
import reducer from './reducer';

export { reducer };
```

3. 再在 src/store/reducer.js 中 将所有的小的 reducer 汇总，生成一个最终的 reducer
```js
import { combineReducers } from 'redux';
import { reducer as headerReducer }  from '../common/header/store';

export default combineReducers({
    header: headerReducer
});
```
4. 此时因为 state 结构发生了变化，变成了
```js
{
     header: {
           focused: false
     }
}
```
所以修改mapStateToProps函数
```js
const mapStateToProps = state => ({
    focused: state.header.focused,      //增加一层 .header

});
```

### 6.7 Header 整体

![jianshu_header](https://raw.githubusercontent.com/514723273/.md-Pictures/master/jianshu_header.png)


此时简书的导航栏已经完成，以上描述了大体的步骤，（还使用了 immutable 对 action 的处理未详细介绍）具体细节可以参考：[Exercise-Project/jianshu/src/common/header](https://github.com/514723273/Exercise-Project/tree/master/jianshu/src/common/header)

目录结构如下：
- store
    - **index.js**---引入store以下所有三个文件，都在这个文件进行导出。外部都只需要统一引入这一个文件即可。
    - **actionCreators.js**---保存了所有的获得 Action 的方法
    - **constants.js**---保存了所有的常量
    - **reducer.js**---保存了对 Header 数据 state 的所有处理
- **index.js**---引用了所有styles.js的单品，*（只）通过嵌套*组合成一个Header组件
- **style.js**---保存了所有单一组件，都经过 css 定位和美化

```js
// src/common/header/store/index.js
import reducer from './reducer';
import * as actionCreators from './actionCreators';
import * as constants from './constants';

export { reducer, actionCreators, constants };
```

