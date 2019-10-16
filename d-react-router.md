### react-router 路由

网址：https://reacttraining.com/react-router/web/guides/quick-start

- 安装 npm i react-router-dom
- 路由中比较重要的组件 ( BroxserRouter | HashRouter ) Route Link NavLink
  BroxserRouter 该组件是路由的包裹组件(凡是路由相关的东西必须由该组件包裹，他模拟的页面是完全仿照浏览器的历史纪录的)
  Route 该组件就代表一个页面，当页面的地址和该组件的 path 匹配时，就展示该组件对应的 component 匹配规则包含规则
  Link NavLink 这两个组件就是用来做路由跳转的(组件的 to 属性)
- 自定义的组件如果被 Route 包裹的子路由，会默认接受 props

##### 1. 下载 安装

打开命令行输入 npm install react-router-dom

##### 2. 在 react 中使用

1. 新建一个 js 文件 存放路由的组件 例如 AppRouter.js

```js
import React from "react";
import { Route } from "react-router-dom";
// as 重命名
import Home from "./Home";
import About from "./About";
// 路由必须包括在 react-router-dom 中的一个组件下（BrowserRouter：仿浏览器记录 和 HashRouter  ：通过锚点跳转 ，hash 模式）
// 只有 Router 包裹的组件内才能使用路由相关的东西
// 一个项目 只能存在一个 Router
const Main = () => {
  return (
    <div>
      {/* 路由 path 的匹配规则是 包含匹配 */}
      <Route path="/" exact component={Home} />
      <Route path="/about" component={About} />
    </div>
  );
};
export default Main;
```

2. 在 app.js 文件中 需要添加路由相关

```js
import { NavLink, BrowserRouter as Router } from "react-router-dom";
function App() {
  return (
    <div className="App">
      {/*想引用 router 里面的东西，必须包裹在 router 里面*/}
      <Router>
        <ul>
          <li>
            <NavLink
              activeStyle={{ color: "blue" }}
              activeClassName="active"
              exact
              to="/"
            >
              Home
            </NavLink>
          </li>
          <li>
            <NavLink activeClassName="active" to="/about">
              About
            </NavLink>
          </li>
        </ul>
        {/* 引入存放路由的组件 */}
        <Main />
      </Router>
    </div>
  );
}

export default App;
```

3. 子路由

```js
import React, { Component } from "react";
import { Route, NavLink } from "react-router-dom";
import Info from "./Info";
class Users extends Component {
  // 子路由里面的地址必须由 副路由的地址开头
  // 在 react 内获取路由地址的相关信息
  // 当一个组件 被当作了 路由组件(改组件 用 Route 包裹)，该组件会默认接受 props 例子：<Route path="/users" component={Users} />
  // 1. history  历史记录的东西
  // 2. location  本地的东西  pathname 就是全路径
  // location:
  // hash: ""
  // key: "s78pr5"
  // pathname: "/users/zhangsan"
  // search: ""
  // state: undefined
  // 3. match match下的 url 给link 用，path 给 Route path 使用
  render() {
    const { match } = this.props;
    console.log(this.props);
    return (
      <div>
        <ul>
          <li>
            <NavLink activeClassName="active" to={`${match.url}/zhangsan`}>
              张三
            </NavLink>
          </li>
          <li>
            <NavLink activeClassName="active" to={`${match.url}/lisi`}>
              李四
            </NavLink>
          </li>
          <li>
            <NavLink activeClassName="active" to={`${match.url}/liuwu`}>
              刘五
            </NavLink>
          </li>
        </ul>
        <Route path={`${match.path}/:name`} component={Info} />
      </div>
    );
  }
}

export default Users;
```

#### 3. 动态路由的更新页面

1. 同一个页面 根据动态路由的 id 或者其他的不同，以此获取响应的 参数

```js
import React, { Component } from "react";
class Info extends Component {
  // 定义一个 变量状态，用于显示或者 axios 请求的参数
  state = {
    title: ""
  };
  // 这个生命周期函数用于 组件一刷新 ，但是动态路由参数更改只更新数据不更新组件。
  componentDidMount() {
    this.setState({
      title: this.props.match.params.id
    });
  }
  // 这个生命周期，是用于 组件更新 state 或者 props 的内容时，才会出现。点击每个动态路由， 通过 props 接受的参数都会发生变化
  componentDidUpdate(prevProps) {
    // 这个生命周期函数内 可以直接写 setState ，但是此语句必须写在 一个判断语句中，否则会机内 死循环 。函数内的自带参数 prevProps 是指路由变化前的路由状态，根据前后的变化是否相同判断是否进行 state 的变化
    if (prevProps.match.params.id !== this.props.match.params.id) {
      this.setState({
        title: this.props.match.params.id
      });
    }
  }
  render() {
    const { title } = this.state;
    return <div>{title}个人信息页</div>;
  }
}

export default Info;
```

2. 路由的事件跳转 this.props.history.push this.props.history.go(-1) 不通过 link 或者 navlink 进行跳转页面，类似于 vue 的 push

当不在路由组件 (route 内包裹的) 内进行 页面的跳转时可以使用 router 的 withRouter。

在 导出时 ，用 withRouter 包裹 导出的名字

```js
import React, { Component } from "react";
import { withRouter } from "react-router-dom";
class Header extends Component {
  render() {
    // 路由的默认 props 内的 history 下的 push 方法
    // witchRouter 必须包裹在 router 下使用，
    return (
      <div>
        <h1
          onClick={() => {
            this.props.history.push("/");
          }}
        >
          返回首页
        </h1>
        <span
          onClick={() => {
            this.props.history.go(-1);
          }}
        >
          返回上一页
        </span>
      </div>
    );
  }
}

export default withRouter(Header);
```

3. 404 页面，当路由不匹配时出现

```js
<div>
  {/* 路由 path 的匹配规则是 包含匹配 */}
  <Switch>
    <Route path="/" exact component={Home} />
    <Route path="/about" component={About} />
    <Route path="/topics" component={Topics} />
    <Route path="*" component={Err} />;
  </Switch>
</div>
// 路由的 switch 方法，switch 下的 route 只能选择一个 匹配 ，前面的匹配到就不匹配后面的
// 所以 home 组件必须严格匹配。否则都是首页
// 匹配 文章 或者 id 类的这种 。需要现在组件内判断 获取的 id 是否能够 获得后台数据 。如果不能就跳转到 404 页面。在 cdm(componentDidMount) 声明函数内
```

4. 路由的重定向

```js
<Switch>
  <Redirect from="/about" to="/newabout" />
  <Route path="/newabout" component={About} />
</Switch>
// 路由的重定向 必须写 switch 。redirect 就是路由重定向需要的
```

#### 弹窗 Prompt

当 when 是 true 时，显示弹窗
`<Prompt when={true} message='xxxx' />`
