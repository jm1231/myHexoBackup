---
title: React路由
date: 2016-05-16 22:10:48
tags: React
---
## 什么是路由？
**"React Router keeps your UI in sync with the URL"**

官方给出的解释就是这样，也就是说路由是用来保证UI和URL保持同步的。这里已经没有了超级链接跳转的概念，完全就是通过路由来控制页面的显示。

github上面有路由的几个lesson用来学习体验路由，克隆下来就可以看：

```
 git clone https://github.com/reactjs/react-router-tutorial.git
```

## 安装使用
通过npm安装react-router到项目文件夹

```
 npm install --save react-router
```

**tips：**使用的时候要注意版本差异问题，语法变化蛮大的。

安装完成后需要在页面中引入router的相关组件，这里列出了三个。

<!-- more -->

```js
//ES 6写法
import { Router, Route, Link } from 'react-router'
//ES 5的引用方法，习惯用这种 ==
var Router = require('react-router').Router
var Route = require('react-router').Route
var Link = require('react-router').Link
```

## demo练习

```js
import React from 'react'
import { Router, Route, Link } from 'react-router'

const App = React.createClass({
  render() {
    return (
      <div>
        <h1>App</h1>
        <ul>
          <li><Link to="/about">About</Link></li>
          <li><Link to="/inbox">Inbox</Link></li>
        </ul>
        {this.props.children}
      </div>
    )
  }
})

const About = React.createClass({
  render() {
    return <h3>About</h3>
  }
})

const Inbox = React.createClass({
  render() {
    return (
      <div>
        <h2>Inbox</h2>
        {this.props.children || "Welcome to your Inbox"}
      </div>
    )
  }
})

const Message = React.createClass({
  render() {
    return <h3>Message {this.props.params.id}</h3>
  }
})

React.render((
  <Router>
    <Route path="/" component={App}>
      <Route path="about" component={About} />
      <Route path="inbox" component={Inbox}>
        <Route path="messages/:id" component={Message} />
      </Route>
    </Route>
  </Router>
), document.body)
```

上面的例子，我们可以理解成，在App组件中，嵌套了About和Inbox的组件，而在Inbox组件中又嵌套了Message的组件。通过路由，我们可以不用连接跳转从而实现各个不同页面之间的显示。