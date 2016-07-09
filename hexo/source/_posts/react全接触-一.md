---
title: React全接触(一)
date: 2016-05-05 21:02:34
tags: React
---
最近开始学习和接触`React`，借此机会记录下自己的学习心得。

#### 一、先了解一下React
![react](http://n1.itc.cn/img8/wb/smccloud/fetch/2015/09/15/8173412602700564.JPG)

`React`一开始是facebook内部的项目，主要用在instagram网站上面的，后来发现这个框架用起来很方便，所以干脆就开源了，一下子吸引了众多的粉丝。它主要就是用来干这个事情的：

> building large applications with data that changes over time.

什么意思呢？通俗的理解也就是说，只要做了一个应用之后，底层数据发生了变化，那么它用户界面的数据渲染就会同时发生相应的改变，很方便。

可能很多人都听说过 `React` 和 `React Native`，他们到底是什么关系呢？
<!-- more -->
其实，`React Native` 就是 `React` 蓬勃发展的产物，`React` 本身越滚越大，从最早的UI引擎变成了一整套前后端通吃的 Web App 解决方案，最后衍生出来的 `React Native` 项目，是希望用写 Web App 的方式去写 Native App。如果能够实现，整个互联网行业都会被颠覆，因为同一组人只需要写一次 UI ，就能同时运行在服务器、浏览器和手机。

OOOOOOOOOOOK.....扯了这么多，我们进入正题，进入`React`的学习。

#### 二、开始React前的准备工作
可以在[官网](https://facebook.github.io/react/downloads.html)下载安装包，里面已经有一些demo可以参考学习用。


**这里要注意，运行例子的时候一定要在服务器环境下，否则可能没有办法正常显示内容，而且浏览器下面需要安装react的插件**

以chrome浏览器为例，如果你没有安装插件，可能会在控制台出现这样的一段：
> Download the React DevTools for a better development experience: https://fb.me/react-devtools

这个很好解决，只需要按照提示安装就好了，不过谷歌浏览器安装插件可能需要科学上网才行哦～

插件装好了之后可能就是搭建本地服务器的问题了，推荐大家使用`nginx`，直接在终端里面敲命令

	brew install nginx
	
就可以搞定，前提是已经安装过`homebrew`，如果觉得这个方式比较麻烦，也可以使用`mamp`，`wamp`之类的软件，建本地服务器很方便。

如果想要一个更加优质的使用体验，那就涉及到各种工具的使用，所以之前先把它们准备好。新建一个文件夹准备放react的相关资料，然后进入到这个文件夹中用终端的方式安装：
	
	npm install react react-tools gulp react-jsx react-dom grunt grunt-cli --save-dev
	
	npm install webpack bower -g
	
这里出现了`npm`，这其实是`node.js`的一个包管理器，可以快速的安装一些工具。

OOOOOOOOOOK......准备工作完成，下面终于要揭开react的面纱啦！	