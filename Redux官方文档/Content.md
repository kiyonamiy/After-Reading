# Redux 官方文档

## 第一章 三大原则

### 1.1 单一数据源

整个应用的 state 被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 store 中。

### 1.2 State 是只读的

唯一改变 state 的方法就是触发 action，action 是一个用于描述已发生事件的普通对象。

这样确保了视图和网络请求都不能直接修改 state，相反它们只能表达想要修改的意图。因为所有的修改都被集中化处理，且严格按照一个接一个的顺序执行，因此不用担心竞态条件（race condition）的出现。

### 1.3 使用纯函数来执行修改

为了描述 action 如何改变 state tree ，你需要编写 reducers。

Reducer 只是一些纯函数，它接收先前的 state 和 action，并返回新的 state。

## 第二章 Action

### 2.1 定义

Action 是把数据从应用（例如用户输入、服务器响应）传到 store 的有效载荷。它是 store 数据的**唯一**来源。一般来说你会通过 store.dispatch() 将 action 传到 store。

```js
const ADD_TODO = 'ADD_TODO'

{
  type: ADD_TODO,
  text: 'Build my first Redux app'
}
```

### 2.2 Action 创建函数

在 Redux 中的 action 创建函数只是简单的返回一个 action 。

这样做将使 action 创建函数更容易被移植和测试。

```js
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}
```

### 2.3 action.js 源码

```js
/**
 * action 类型
 */

export const ADD_TODO = 'ADD_TODO'
export const TOGGLE_TODO = 'TOGGLE_TODO'
export const SET_VISIBILITY_FILTER = 'SET_VISIBILITY_FILTER'

/**
 * 其他的常量
 */

export const VisibilityFilters = {
    SHOW_ALL: 'SHOW_ALL',
    SHOW_COMPLETED: 'SHOW_COMPLETED',
    SHOW_ACTIVE: 'SHOW_ACTIVE'
}

/**
 * action 创建函数
 */

export function addTodo(text) {
    return {
        type: ADD_TODO,
        text
    };
}

export function toggleTodo(index) {
    return {
        type: TOGGLE_TODO,
        index
    };
}

export function setVisibilityFilter(filter) {
    return {
        type: SET_VISIBILITY_FILTER,
        filter
    };
}
```

## 第三章 Reducer

### 3.1 定义

Reducers 指定了应用状态的变化*如何响应* actions 并发送到 store 的。（记住 actions 只是描述了有事情发生了这一事实，并没有描述应用如何更新 state。）

reducer 就是一个**纯函数**，接收旧的 state 和 action，返回新的 state。**只要传入参数相同，返回计算得到的下一个 state 就一定相同。没有特殊情况、没有副作用，没有 API 请求、没有变量修改，单纯执行计算。**
```
(previousState, action) => newState
```

### 3.2 注意事项

**永远不要**在 reducer 里做这些操作：
- 修改传入参数；
- 执行有副作用的操作，如 API 请求和路由跳转；
- 调用非纯函数，如 Date.now() 或 Math.random()。

```js
import { VisibilityFilters, SET_VISIBILITY_FILTER } from './actions';

const initialState = {
    visibilityFilter: VisibilityFilter.SHOW_ALL,
    todos: []
}

function todoApp(state = initialState, action) {
    switch(action.type) {
        case SET_VISIBILITY_FILTER:
            return Object.assign({}, state, {
                visibilityFilter: action.filter
            })
        default:
            return state
    }
}
```

**注意：**

1. 不要修改 state。 使用 Object.assign() 新建了一个副本。不能这样使用 Object.assign(state, { visibilityFilter: action.filter })，因为它会改变第一个参数的值。你必须把第一个参数设置为空对象。你也可以开启对 ES7 提案对象展开运算符的支持, 从而使用 { ...state, ...newState } 达到相同的目的。
2. 在 default 情况下返回旧的 state。遇到未知的 action 时，一定要返回旧的 state。

### 3.3 拆分 Reducer

对 state 每一个属性分别抽离出一个 reducer 进行处理。

注意每个 reducer 只负责管理全局 state 中它负责的一部分。每个 reducer 的 state 参数都不同，分别对应它管理的那部分 state 数据。

```js
function visibilityFilter(state = SHOW_ALL, action) {
    //...
    return state
}

function toodos(state = [], action) {
    //...
    return state
}

function todoApp(state = {}, action) {
    return {
        visibilityFilter: visibilityFilter(state.visibilityFilter, action),
        todos: todos(state.todos, action)
    }
}

export default todoApp;
```

### 3.4 reducers.js

```js
import { combineReducers } from 'redux';

import {
    ADD_TODO,
    TOGGLE_TODO,
    SET_VISIBILITY_FILTER,
    VisibilityFilters
} from './actions';

const { SHOW_ALL } = VisibilityFilters;

// 参考初始化的 state ，下面两个函数 *分别* 对其两个属性进行操作，再整合。
// const initialState = {
//     visibilityFilter: VisibilityFilter.SHOW_ALL,
//     todos: []
// }

// 抽出一个 reducer 来专门管理 visibilityFilter
function visibilityFilter(state = SHOW_ALL, action) {
    switch(action.type) {
        case SET_VISIBILITY_FILTER:
            return action.filter;
        default:
            return state;
    }
}

//
function todos(state = [], action) {
    switch(action.type) {
        case ADD_TODO:
            return [
                ...state, 
                {
                    text: action.text, 
                    completed: false
                }
            ];
        case TOGGLE_TODO:
            return state.map((item, index) => {
                //只处理选中的 todo 项
                if(index === action.index) {
                    return Object.assign({}, item, {
                        completed: !item.completed
                    })
                }
                return item;
            })
        default:
            return state;
    }
}

const todoApp = combineReducers({
    visibilityFilter,
    todos
});

//等价于
// function todoApp(state = {}, action) {
//     return {
//       visibilityFilter: visibilityFilter(state.visibilityFilter, action),
//       todos: todos(state.todos, action)
//     }
//   }

export default todoApp;
```

## 第四章 Store

### 4.1 定义

在前面的章节中，我们学会了
- 使用 action 来描述“发生了什么”
- 使用 reducers 来根据 action 更新 state 的用法

Store 就是把它们联系到一起的对象。Store 有以下职责
- 维持应用的 state
- 提供 getState() 方法获取 state
- 提供 dispatch(action) 方法更新 state
- 通过 subscribe(listener) 注册监听器
- 通过 subscribe(listener) 返回的函数注销监听器

再次强调一下 **Redux 应用只有一个单一的 store**。

通过 createStore() ，传递 reducer，创建 store 。
```js
import { createStore } from 'redux';
import { todoApp } from './reducers';

//createStore() 的第二个参数是可选的, 用于设置 state 初始状态。
let store = createStore(todoApp, window.STATE_FROM_SERVER);
```

### 4.2 发起 Actions

```js
import {
    addTodo,
    toggleTodo,
    setVisibilityFilter
} from './actions';

// 打印初始状态
console.log(store.getState());

// 每次 state 更新时，打印日志
// 注意 subscribe() 返回一个函数用来注销监听器
const unsubscribe = store.subscribe(() => console.log(store.getState()));

// 发起一系列 action
store.dispatch(addTodo('Learn about actions'))
store.dispatch(addTodo('Learn about reducers'))
store.dispatch(addTodo('Learn about store'))
store.dispatch(toggleTodo(0))
store.dispatch(toggleTodo(1))
store.dispatch(setVisibilityFilter(VisibilityFilters.SHOW_COMPLETED))

// 停止监听 state 更新
unsubscribe()
```

### 4.3 index.js 源码
```js
import { createStore } from 'redux'
import todoApp from './reducers'

let store = createStore(todoApp)
```