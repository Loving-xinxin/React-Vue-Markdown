## react

#### react 环境搭建

使用官方提供的脚手架 creat-react-app , 如何使用

1. `npx creat-react-app my-react-app` 前提 Node >= 8.10 和 npm >= 5.6 (node -v 或 npm -v 检查版本)
2. 全局安装 creat-react-app 包 ， npm i -g creat-react-app ，使用命令 `creat-react-app my-react-app` 即可

#### react 项目启动修改

- index.js 相当于 vue 中的 main.js
  index.css 相当于 vue 中的 glocal.css

  ```js
  import React from "react";
  // 让 react 的 js 内能写 html 标签
  // 我们称 这种语法  叫 jsx 语法
  import ReactDOM from "react-dom";
  // react 将虚拟 dom 转化成 真实 dom 的工具
  import "./index.css";
  import App from "./App";
  import * as serviceWorker from "./serviceWorker";
  // 缓存  可以删除不要

  ReactDOM.render(<App />, document.getElementById("root"));
  // 渲染
  serviceWorker.unregister();
  ```

#### react 的 基础知识

1. react 组件
   组件名 必须首字母大写

   1. 函数式组件（无状态组件 没有 state ） 直接写一个函数 函数名首字母大写 该函数必须返回标签 并且默认导出该函数
   2. react 组件 是使用 class 创建

   jsx 语法内 需要注意

   1. class 名 className='xxxx' label 的 for 写成 htmlFor
   2. 标签内的属性使用 js 的话 例如 src={logo} 在标签之间写 js 使用{} 例如 <span>{a}</span>

- 无状态函数

  ```js
  import React from "react";
  function index(props) {
    // 该函数内直接 const function () => {} 在下面绑定使用
    // 必须使用箭头函数
    // props. 直接使用 必须在函数内声明形参
    return <div className="">index</div>;
  }
  // 代码片段 rfc
  ```

- 类 函数

  ```js
  import React, { Component } from "react";
  // 直接  function () => {} 在下面绑定使用
  // this.props 使用
  class index extends Component {
    state = {};
    render() {
      return;
    }
  }
  export default index;
  // 代码片段 rcc
  ```

##### JSX 语法

在 js 文件内写 html 语法
如何在 html 内写 js ，html 内的 js 语法都使用 { } 套上

###### 1.条件渲染

---直接在 html 内使用 js 做判断即可

###### 2.列表渲染

将你的列表使用 map 方法转化成标签数组直接放到 html 内，自动会被渲染， 注意要加 key 属性

###### 3.事件处理

如何绑定

1. 首先在组件的 class 内创建一个方法 handleClick
2. 使用标签的 onClick 属性绑定事件 例如`<span onClick={this.handleClick}></span>`

事件绑定如何传递参数

1. 事件绑定的必须是一个函数不能是函数调用
2. 要把你定义好的 handleClick 放到一个箭头函数内去执行并传参，然后将该箭头函数绑定给标签 onClick 属性。 例如
   `<span onClick={()=> this.handleClick(argument) }></span>`
   **需要额外注意的就是 事件对象 event ，这个对象必须是事件函数才有的**

###### 4.表单

##### （1） input 双绑定

使用 value 和 onChange

```js
// return 中
<input
  type="text"
  value={val}
  onChange={e => {
    this.change(e);
  }}
/>;
// class 中直接写箭头函数
change = e => {
  this.setState({
    val: e.target.value
  });
};
```

##### （2） textarea 双绑定

    使用 value 和 onChange

##### （3） select 双绑定

    使用 value 和 onChange

##### state

组件的状态，react 框架要求页面所有的变化需要和 state 有直接关系

1. 如何定义
   在 class 内
   ```js
   class Button extends Component {
     state = {
       count: 0
     };
   }
   ```
2. render 函数内获取 this.state
3. 修改 state 必须通过 setState 函数修改
   ```js
   handleClick ()=>{
     this.setState({
       count:100
       // 修改 state 中的 count 为 100
     })
   }
   ```

##### props

父子组件传递数据

1. 在父组件内直接给子组件定义一个属性把传递的数据当作属性值传递过去
   `<Button text='父组件的数据'>`
2. 在子组件内会有一个默认的 props 属性来存储父组件传递过来的数据
   `this.props.text`
3. 可以通过 Button(组件名).defaultProps 设置 props 的默认值，还可以使用环境自带的 prop-types 包对 props 进行类型检查

   ```js
   import PropTypes from "prop-types";
   Btn.defaultProps = {
     text: "默认按钮"
   };
   Btn.propTypes = {
     text: PropTypes.string
   };
   ```

##### React 组件生命周期

1.  组件的生命周期可分成三个状态：

    Mounting：已插入真实 DOM
    Updating：正在被重新渲染
    Unmounting：已移出真实 DOM

2.  生命周期的方法有

    ##### Mounting:

    -- constructor(){}:直接为 this.state 赋值
    -- render(){}:
    -- componentDidMount(){}:会在组件挂载后（插入 DOM 树中）立即调用(首次刷新时)。依赖于 DOM 节点的初始化应该放在这里，之后组件已经生成了对应的 DOM 结构，可以通过 this.getDOMNode()来进行访问。网络请求获取数据(axios),使用 this.setState() 但是动态路由参数更改只更新数据不更新组件。

    ##### Updating:

    -- render(){}:
    -- componentDidUpdate(){} 在组件完成更新后立即调用,在初始化时不会被调用。这个生命周期，是用于 组件更新 state 或者 props 的内容时，才会出现。点击每个动态路由， 通过 props 接受的参数都会发生变化。这个生命周期函数内 可以直接写 setState ，但是此语句必须写在 一个判断语句中，否则会机内 死循环 。函数内的自带参数 prevProps 是指路由变化前的路由状态，根据前后的变化是否相同判断是否进行 state 的变化

    ##### Unmounting:

    -- componentWillUnmount(){} 在组件从 DOM 中移除之前立刻被调用。

##### react 项目启动修改 port

```json
"start":"set PORT=3006 && react-scripts start"
```

##### react 组件的样式处理

一般来说我们直接创建对应的 css 文件，再导入到组件内，也可以在组件内的 html 标签内写行内样式。导入文件样式都是全局的。

1. 如何在 react 项目内使用 sass

   - 安装 npm i node-sass
   - react 新环境默认支持 sass 语法
   - 在文件中直接使用即可

2. 如何让组件的 css 私有化

   - 直接把组件的样式写在行内
   - 使用 styled-components 包解决
