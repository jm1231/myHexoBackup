---
title: 认识gulp
date: 2016-05-10 00:13:44
tags: gulp
---
# 认识脚手架之一 - gulp
### 什么是gulp
---
gulp是基于node.js的一款开源脚手架工具，其依靠于流式操作(stream)和简单的API,迅速的在开发者心目中占据了一定的地位，本文将简单介绍gulp的简单使用及其重要的几个api。


如果想更加深入的了解gulp，请参考官方网站:[gulp官网](http://www.gulpjs.com.cn/)。

### 流式操作
---
什么是流式操作，简单讲，就是读取文件 → 修改文件 → 输出文件，示例图：
![stream](http://7xtbjo.com2.z0.glb.clouddn.com/stream.png)

<!-- more -->

### 安装gulp
---
首先应该确保电脑上已经安装了node和npm，新建一个gulp的文件夹用来测试学习。
#### 1.全局安装gulp
> $ npm install -g gulp 

#### 2.作为项目的开发依赖安装
> $ npm install --save-dev gulp

#### 3.在项目文件夹下新建一个gulpfile.js的文件
安装完gulp之后需要初始化出来一个package.json的文件，后面迁移项目时可以很方便的根据这个json文件安装会所有的依赖包。

如何初始化？
> $ npm init

初始化ok，接着就要新建gulpfile.js了：

```javascript
var gulp = require('gulp');
gulp.task('default',function(){
	//默认任务代码
})
```
#### 4.运行gulp
> $ gulp

执行这步操作后，上面定义的default任务将会被执行，如果想要单独执行特定的任务，只需要：
> $ gulp (taskName) (otherTaskName)

### 常用api
---
#### 1. gulp.src

```javascript
gulp.src('client/templates/*.jade')     
  .pipe(jade())  
  .pipe(minify())  
  .pipe(gulp.dest('build/minified_templates'));
```
示例代码表示要读取client/templates文件夹下面的所有jade文件并进行相关的操作。
#### 2. gulp.dest

```javascript
gulp.src('./client/templates/*.jade')
  .pipe(jade())
  .pipe(gulp.dest('./build/templates'))
  .pipe(minify())
  .pipe(gulp.dest('./build/minified_templates'));
```
示例代码表示读取相关文件并进行操作之后，将会写入指定的目录中，也即是build/minified_templates文件夹中。
#### 3. gulp.task

```javascript
gulp.task('mytask', ['array', 'of', 'task', 'names'], function() {
  // 做一些事
});
```
需要注意的是，如果在定义的`mytask`任务后面有数组参数，那就是一个阻塞的过程，必须先执行数组中的任务完后才会执行当前定义的任务。
#### 4. gulp.watch

```javascript
var watcher = gulp.watch('js/**/*.js', ['uglify','reload']);
watcher.on('change', function(event) {
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});
```
上述代码表示监听在**文件夹下面的任意js文件，当发生改变时，立即执行uglify和reload任务。

### gulp插件
---
gulp还提供了丰富的插件来完成日常的工作内容，按需查询[gulp插件集](http://gulpjs.com/plugins/)。
