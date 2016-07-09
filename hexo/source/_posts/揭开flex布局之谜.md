---
title: 揭开flex布局之谜
date: 2016-05-08 15:15:16
tags: css
---
# flex布局
用习惯了传统的`display`、`position`和`float`布局方式，今天深入的学习了一下flex布局。
## 一、什么是flex？
flex布局就是弹性布局，任何一个容器都可以指定弹性布局`display:flex;`，行内元素也可以`display:inline-flex;`

> *注意*，采用flex布局后，**子元素**的`float`、`clear`、`vertical-align`属性将失效

## 二、父容器的属性
- flex-direction
- flex-wrap
- flex-flow
- justify-content
- align-items
- align-content
<!-- more -->

### 1.flex-direction
规定了容器中子项目的排列方向  

```css
flex-direction:row | row-reverse | column | column-reverse

```
### 2.flex-wrap
规定了容器中子项目需不需要换行显示  

```css
flex-wrap:nowrap | wrap | wrap-reverse

```
### 3.flex-flow
flex-direction和flex-flow的简写形式，默认值为row nowrap

```css
flex-flow:<flex-direction> || <flex-wrap>

```
### 4.justify-content
规定了容器中子项目在主轴上的对齐方式 

```css
justify-content:flex-start | flex-end | center | space-between | space-around

```
### 5.align-items
规定了容器中子项目在交叉轴上的对齐方式 

```css
align-items:flex-start | flex-end | center | baseline | stretch

```
### 6.align-content
规定了容器中子项目在多轴上的对齐方式，如果只有一根轴线，则不起作用

```css
align-content:flex-start | flex-end | center | space-between | space-around | stretch

```

## 三、子项目的属性
- order
- flex-grow
- flex-shrink
- flex-basis
- flex
- align-self

### 1.order
规定了容器中子项目的排列顺序，数字越小排位越靠前，默认为0，负数也可以设置。 

```css
order:<integer>

```
### 2.flex-grow
规定了容器中子项目的放大比例，默认为0，即就算有剩余空间，也不放大。如果有一个子项目设置了flex-grow为2，其他都为1，那么在多余的剩余空间中，为2的占据的剩余空间将大一倍。

```css
flex-grow:<number>

```
### 3.flex-shrink
规定了容器中子项目的缩小比例，默认为1，即空间不足时，该项目将缩小。负值无效。

```css
flex-shrink:<number>

```
### 4.flex-basis
规定了容器中子项目分配剩余空间之前，项目占据的主轴空间。浏览器就是根据这一属性来计算是否还有剩余空间。默认值为auto。可以设置固定300培px，将占据固定空间。

```css
flex-basis:<length> | auto
```
### 5.flex
flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。建议优先使用none（0 0 auto）auto（1 1 auto）
```css
flex:none | auto | [<flex-grow> <flex-shrink>||<flex-basis>]
```
### 6.align-self
允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch

```css
align-self: auto | flex-start | flex-end | center | baseline | stretch
```