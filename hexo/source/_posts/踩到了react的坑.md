---
title: 踩到了react的坑
date: 2016-05-07 21:48:02
tags: React
---
今天尝试第一次用react构建一个原型界面，发现他的语法和正常的html和css语法有个别差异还是满诡异的，特此记录，共勉，以后不能再犯这样的错误。

### 1.关于img标签的闭合问题
 一般我在写html的时候，img标签都是这样写的：
 ![img未闭合](http://7xtbjo.com2.z0.glb.clouddn.com/img%E6%9C%AA%E9%97%AD%E5%90%88.png)
 然而在浏览器中却报错了：
 ![react中的img未闭合报错](http://7xtbjo.com2.z0.glb.clouddn.com/react%E4%B8%ADimg%E6%9C%AA%E9%97%AD%E5%90%88%E6%8A%A5%E9%94%99.png)
 <!-- more -->
 百思不得其解，抱着试试看的态度加了个闭合：
 ![img闭合](http://7xtbjo.com2.z0.glb.clouddn.com/img%E9%97%AD%E5%90%88.png)
 我勒个擦擦！！图片竟然正常显示了！！
 > 结论：在react项目中，但标签一定要记得闭合，语法要严格。
 
### 2.关于css样式书写
 css语法也和习惯的标准css语法有差异，用一张图来说明问题：
 ![react中的css写法](http://7xtbjo.com2.z0.glb.clouddn.com/css%20in%20react.png)
 > react中的css是被当做一个变量，然后以`style={css变量名}`的方式调用。
 
### 3.React.render() vs ReactDOM.render()
因为现在网络上面对于react相关的教程说明较少，看到一些别人写的demo不明白的地方不免会有一些困惑，我就一直弄不明白 `React.render()` 和 `ReactDOM.render()` 到底有什么区别，因为感觉达到的效果都是一样的，为什么会有这两种写法，然后google了一下，发现stackoverflow上面有人和我有一样的困惑，人家的解答（见下图）也算是给我上了一课：

![React.render() vs ReactDOM.render()](http://7xtbjo.com2.z0.glb.clouddn.com/react.render%E5%92%8Creactdom.render%E5%8C%BA%E5%88%AB.png)

官方文档中有说明:
> To make this more clear and to make it easier to build more environments that React can render to, we’re splitting the main react package into two: react and react-dom.

> The old names will continue to work with a warning until 0.15 is released.

也就是说，随着react的版本升级，把原来让一个组件干的活拆分成两个组件了，更加精细化，换句话，ReactDOM.render()是更高级的写法 ==.