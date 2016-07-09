---
title: 关于chrome、Safari和firefox在渲染a链接上的问题
date: 2016-04-26 13:20:17
tags: 浏览器兼容
---
这两天做秀野堂官网，在二级列表页面上遇到了一个问题，如下图↓↓↓↓↓↓
<div style="text-align:center;">
<img src = http://7xtbjo.com2.z0.glb.clouddn.com/wrong.PNG height=600>
</div>
<!-- more -->
在safari和firefox里面，前几排列表还算正常显示，后面的就被挤出正常显示范围之外了
但是在chrome中一切正常显示,如下图↓↓↓↓↓↓↓↓↓↓

<div style="text-align:center;">
<img src = http://7xtbjo.com2.z0.glb.clouddn.com/right.PNG height=600>
</div>
 

> *解决方法*：给li里面的a元素也设置高度和行高就可以了，最主要的是display:block属性。
