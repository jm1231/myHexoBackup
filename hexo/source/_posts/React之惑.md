---
title: React之惑
date: 2016-05-18 23:01:21
tags: React
---
学习`react.js`已经有一段时间了，从一开始的好奇到云里雾里稀里糊涂到现在渐渐清晰整体结构，着实花费了很大的力气。

## 认识react之初
react.js刚开始接触的时候，感受最深的就是组件化和虚拟DOM。

和之前用传统的方式（即：HTML和CSS）切页面的方式相比较，react最大的不同就是组件化。组件化的目的就是要复用，而且修改起来方便定位，效率提高不少。

```js
    var Mycomponent = React.cereateClass({
        render: function(){
            return <input type='text' />
        }
    })
```

这就是自定义的一个Mycomponent组件，可以在任意页面需要用到的地方调用这个组件。

但是在组件化的过程中遇到的一个难题：**到底组件化到什么地步才算合适？**

<!-- more -->

我们做项目的开始，一直都是抱着能颗粒化到最小程度就颗粒化到最小程度的态度来细分，以至于后面用路由在不同页面之间传值的时候造成了一定的困扰，因为有很多组件都是由单个细小的组件拼接起来，组成一个相对复杂的组件往往已经嵌套了大于3层的结构，这样值传递就很容易造成换乱，逻辑绕不太清。

说完了组件化，那么虚拟DOM又是什么鬼？

就我的理解，虚拟DOM就是render出来的那玩意儿，但它并不是真正的DOM节点，而是一个js的对象。当DOM结构有数据发生变化时，他能很快度的做出反应，比较之后只需要把有变化的地方更新，无需刷新整个DOM树，提高了性能。

## 经历react路由
一开始接触路由的时候，简直就是满脸的恐惧。

我知道路由是用来干嘛的，就是在一个页面中实现不同页面的跳转，而无需再通过a链接。但是自己操作的时候，各种不知道如何下手。

- 如何在同一个页面中渲染出不同的页面？
- 不同页面之间如何传值？
- 如何从后台请求数据？
- 如何添加事件？

问题多到乱如麻，虽然现在写下这些问题的时候心里已经有了各个问题的答案，不能说有多熟悉代码，但是多少已经知道要怎么做了。今天之前真的是完全一堆乱麻，这可能就是一个渐渐熟悉的过程，输入太多的知识，需要一段时间来融会贯通。

关于刚刚提到的那几个问题，现在可以粗略的给出我的答案。

### 如何在同一个页面中渲染出不同的页面？
首先引入react-router的几个模块。

```js
import { Router, Route, Link } from 'react-router’;
```

其中Link模块就是可以实现页面的跳转，具体使用代码如下：

```js
var XYT_item = React.createClass({
getInitialState() {
   return {
        name:this.props.name,
        id:this.props.id,
        direction:this.props.direction
   };
},

    render(){
        return (
            <div style={XYTss.item}>
            <Link to={`${this.state.direction}/${this.state.id}`}>{this.state.name}</Link>
             </div>
        );
    }

});
```


Link to部分就表示了要跳转的路径，如果有多个子项需要跳转，那么就要在路径上面加上id用来区别。

### 不同页面之间如何传值？
这里涉及到了`this.props.xxx`的使用。

```js
var XYT_pic = React.createClass({
	render: function(){
		return (
			<div style={XYTss.pic}>
				<img src={this.props.src}/>
			</div>
			)
	}
})
```

注意上面的组件`XYT_pic`被嵌套在下面组件`XYT_docItem`中，所以img标签的src属性就要从父级获得，通过`this.props.src`将图片链接`http://www.abc.com/resource/smallimage/1.jpg`传递进去。如果有多层嵌套，那么，这里传值的逻辑就要把握好，以免出错。

```js
var XYT_docItem = React.createClass({
	render: function(){
		return (
			<div style={XYTss.doctorItem}>
				<XYT_pic src='http://www.abc.com/resource/smallimage/1.jpg'/>
				<XYT_detail />
				<XYT_buttonTall />
			</div>
		)
	}
})
```

### 如何从后台请求数据？
直接上代码。

```js
componentDidMount:function(){
       //方法一：fetch
       var doctorArr = [];
       this.setState({data: doctorArr});

        fetch('http://localhost:8085/php/1.php?params=doc')
        .then((response) => response.text())
        .then((responseText) => {
            this.setState({data: JSON.parse(responseText)});
        })
        .catch((error) => {
          console.warn(error);
        });                    

       //方法二：ajax
        $.ajax({
           url: 'http://localhost:8085/php/1.php?params=doc',
           dataType: 'json',
           type: 'get',
           success: function(data) {
             this.setState({data: data});
           }.bind(this),
           error: function(xhr, status, err) {
             console.error(this.props.url, status, err.toString());
           }.bind(this)
         });

    },
```
这里将数据请求放在`componentDidMount`中，也就是dom一完成挂载就触发数据请求。上面的代码展示了两种数据请求的方法，`fetch`比较新颖，而且全部用的`es6`的语法，值得好好研究一下。第二种方法则是较传统的`ajax`。

### 如何添加事件？
依旧上代码说明。

```js
var YourComponent = React.createClass({
  handleClick: function(e) {
    // your code here 
    // ..
  },
  render: function() {
    return (
      <div>
      		<p onClick={this.handleClick}>点我触发点击事件</p>
      </div>
    );
  }
});
```

react中支持丰富的事件，具体请查看官方文档：[官方文档关于event部分](http://facebook.github.io/react/docs/events.html)

---
OK，至此总结差不多了。这仅仅是我接触学习react这几天以来的感受，很浅显，仍然需要不断的联系和运用才能有更深的体会。
