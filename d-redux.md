- 安装

  - npm i redux

- 创建

  - 创建 src/store.js
  - 在 store.js 内

    ```js
    import { createStore } from "redux";
    const inititalState = {
      count: 0
    };
    const rootReducer = (state = inititalState) => {
      return state;
    };
    const store = createStore(rootReducer);
    export default store;
    ```

  - 静态获取

    - 在组件内 导入 store 使用 store.getState()

  - 修改 store
  - 在 store.js 内的 reducer 函数内

  ```js
  const rootReducer = (state = inititalState, action) => {
    switch (action.type) {
      case "ADD":
        console.log("触发了ADD类型的 action");
        //state.count = state.count + 1;
        //return state;
        // 不可以对 state 直接进行修改 原因是由于 redux 要求不变性(保证不修改原来的 state 返回新的 state 如果修改了，那么 prev state和next state 一样导致视图不会更新（即使数据发生了变化视图也不会跟随变化）)
        // const newState = { ...state };
        // newState.count = newState.count + 1;
        // return newState;
        return {
          count: state.count + 1
        };
      default:
        return state;
    }
  };
  ```

- redux 的中间件

  - redux-logger 作用是当你更改 store 中的数据的时候，帮你在控制台输出记录
  - 如何使用 需要在 redux 中导出一个方法 applyMiddleware 在创建的时候使用该方法给 store 添加上中间件功能

  - 导入

  ```js
  import { createStore, applyMiddleware } from "redux";
  import logger from "redux-logger";
  ```

  - 使用

  ```js
  const store = createStore(rootReducer, applyMiddleware(logger));
  ```

####

## redux

安装 `npm install --save redux`

1. 在 src 文件下，创建 store 文件，

```js
// createStore 方法需要接受一个函数 作为参数，该函数被叫做reducer 函数
// reducer 函数会默认接受一个 state 作为参数，给该参数赋的值就是原始的 state 并且 把该参数返回 (return)
// 这样 createStore 方法自行的时候就会创建一个 store 了，reducer 函数内的 state 参数就是 共享的 state ，也就是 store 的数据
import { createStore } from "redux";
const inititalState = {
  count: 10
};
const rootReducer = (state = inititalState) => {
  return state;
};
const store = createStore(rootReducer);
export default store;
```

简写方法

```js
export default createStore((state = { count: 0 }) => state);
```

组件内使用 这只是静态获取

```js
import store from "../store";
store.getState();
```

### action 修改 state

```js
// action 相当于 vue的 mutation
// action 类型需要写成 大写 的
// 通过 store 下的 dispatch 发 action
// store.dispatch({type:'ADD',payload:"载荷数据"})
const rootReducer = (state = inititalState, action) => {
  switch (action.type) {
    // 多个 判断，写多个 case
    case "ADD":
      // 不可以 对 state 直接进行修改，
      return { count: state.count + 1 };

    // 或者;
    // const newState = { ...state };
    // newState.count = newState.count + 1;
    // return newState;   总之不能直接对 state 进行修改

    default:
      // state 的初始值，相当于 最开始的 vuex 的state
      return state;
  }
};
```

组件内

```js
 <button
          onClick={() => store.dispatch({ type: "ADD", payload: "载荷数据" })}
        >
          +
        </button>
        <span>{num}</span>
```

### redux 的中间件，redux-logger 当你改变 state 中的数据 ，帮你在 控制台 显示

`import { applyMiddleware } from "redux";`
`import logger from "redux-logger";`
导入
`const store = createStore(rootReducer, applyMiddleware(logger));`
使用

## 若要 redux 和 react 关联，

`npm i react-dredux`
在 index.js 文件内，引入
`import {Provider} from 'react-redux';`
react 获取 redux 中的 state 的数据 类似当做自己的 state 。store 数据变 react 组件内也更新
需要从 react-redux 包中拿 provider 组件，该组件的作用是获取 redux 中 store 数据提供给 react 中的所有组件
我们将 该组件包裹在 App 组件外，意思是提供给 App 组件 store ，也就是意味着所有的组件都能够获取 store 中的数据
`<Provider store={store}> <App /> </Provider>,`
两个子组件都需要使用 store 中的数据，
在 父组件 去动态获取 store 中的数据

- 在 react-redux 包中拿 connect 方法
- connect 是一个高阶函数需要调用两次 ，connect()()
- 第一次调用需要接收一个函数作为参数，这个函数被叫做 mapStateToProps 将 store 中的 state 映射成该组件 的 props

  <!-- - 这个函数默认接受一个  state 作为参数，该参数 指的就是 store 中的 state ，函数还必须返回一个 对象，这个对象就是作为组件 -->

- 第二次调用，需要接受一个组件作为参数，此次调用作用是将 mapStateToProps 函数的返回值 当做 props 传递给组件 并 返回该组件

```js
import {connect} from 'react-redux'
render(){
  log(this.props) 这个 log 的值就是 下边的 return 的值，
  return (
    <子组件 xxx={this.props.xxx}>
  )
}
const mapStateToProps=state=>{
  return {
    xxx:state.xxx
  }
}
export default connect(mapStateToProps)(组件的名字)
```

## 在有多个数据的情况下，需要 使用 return {...state,state 下的变量: 需要更改的};

### react.Fragment 允许不使用 div 写多个节点

## 流程

1. 在 index.js 文件内,

```js
import store from "store";
import { Provider } from "react-redux";
//  引入 方法， 设置桥梁  将 store 和 react 联系起来,
ReactDOM.render(
  // 使 各个组件能够 使用 store 内的数据
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```

2. 在 使用 的组件

```js
import { connect } from "react-redux";
// 高级函数，使用和两次 ()()
// 默认接受 state 。
const mapStateToProps = state => {
  return {
    posts: state.posts
  };
};
// 使用时 ，第一个返回 值为 函数
export default connect(mapStateToProps)(Home);
```

# 模块化

1. 在 src 下创建 store 文件夹 ，并创建 reducers 文件夹、actionTypes 文件 和 index.js 文件 和 actions.js 文件

#### index.js 文件

```js
// 引入 创建的 store ， applyMiddleware 是使用中间件 ，combineReducers 是将 组件组合到一起
import { createStore, applyMiddleware, combineReducers } from "redux";
import logger from "redux-logger";
// 引入 中间件  logger 组件
import comments from "./reducers/comments";

// 下边就是自己写的组件，引入并且 合并到 index 文件内
import posts from "./reducers/posts";

// redux-thunk 插件是 为了将 异步请求放到一个统一的文件，中间件 所以 在 index.js 文件内使用
import thunk from "redux-thunk";

const rootReducer = combineReducers({
  comments,
  posts
});

const store = createStore(rootReducer, applyMiddleware(logger, **thunk**));
export default store;
```

#### 模块的 js 文件

在 reducers 文件夹内，分组件 进行过 模块化

```js
// 将 重新定义的 名字，从 actionsTypes 文件内引出 ，利于统一化改变和维护
import { GET_COMMENTS, DEL, ADD_COMMENTS } from "./../actionTypes/actionTypes";
// 导出的 state ，就是以前定义的变量，现在分模块化，只存在一个 变量
export default (state = [], action) => {
  switch (action.type) {
    // 变量的修改。 数组的修改 使用 扩展运算符 ，将后面的数组赋予给前面，
    // 对象的数组单独更新 {...state,comments: state.comments.filter(item => item.id !== action.payload)};
    case GET_COMMENTS:
      return [...state, ...action.payload];
    case DEL:
      return [...state.filter(item => item.id !== action.payload)];
    case ADD_COMMENTS:
      return [...state, action.payload];

    default:
      return state;
  }
};
```

### actionTypes 文件，作用是 将 变量的 名字 统一化管理

```js
export const GET_POSTS = "GET_POSTS";
export const GET_COMMENTS = "GET_COMMENTS";
export const DEL = "DEL";
export const ADD_COMMENTS = "ADD_COMMENTS";
```

### actions.js 文件 包含异步请求的数据

```js
import {
  GET_POSTS,
  GET_COMMENTS,
  ADD_COMMENTS,
  DEL
} from "./actionTypes/actionTypes";
// 创建 异步的 action ,必须搭配 redux-thunk 或 redux-saga
import axios from "axios";
// 普通的 action 函数 会返回一个 对象 (type：XXX，payload：XXX)
// 异步的 action 创建函数需要返回一个函数，该返回函数需要一个参数 dispatch，在该返回的函数内部可以直接发送 异步请求，请求成功之后 使用该 dispatch 发出 action
const addComments = payload => {
  console.log(payload.payload);
  // 这个 addComments 函数由于会被 mapDispatchToProps 方法包装，包装之后就可以获取到 disoatch 函数，当执行被包装的 action 创建函数时会把 dispatch 当做参数传递给 被包装的 action 创建函数的返回值的参数
  return dispatch => {
    axios
      .post("http://localhost:3008/comments", {
        text: payload.payload.val,
        postId: payload.payload.id
      })
      .then(res => {
        dispatch({ type: ADD_COMMENTS, payload: res.data });
        // 传递的函数 调用
        payload.payload.clearVal();
      });
  };
};
// 导出的 函数名 ，在组件内  通过 thsi.props.addComments(传递 的参数) 调用 ，
export { addComments };
```

### 组件内的各种调用

1. 组件内 通过 connect 调用过 index 内的 state，即 存在 mapStateToProps

```js
// 引入
import { getPosts } from "./../store/actions";
// 调用 ，通过 props 使用
componentDidMount() {
    this.props.getPosts();
  }
  // 导出，
export default connect(mapStateToProps,
// 可以利用 mapDispatchToProps 将 action 创建函数附带上 dispatch 功能，这样的意思就是直接 action 创建函数 默认触发 dispatch
//  {getPosts} 是 mapDispatchToProps 的语法糖，通过 props 触发 这个函数
  { getPosts }
)(Home);
```

2. 不存在 mapStateToProps 。

```js
// 引入多个 函数的名字
import { del, addComments } from "./../store/actions";
// 引入 connect 方法，搭建桥梁
import { connect } from "react-redux";

// 导出 connect(null,{引入的变量})(组件名字)
export default connect(
  null,
  { del, addComments }
)(PostComment);
```
