---
title: php API初体验
date: 2016-05-24 21:35:07
tags: [php,MySQL,API]
---
为了能够更方便的实现增删改查，我们可以通过API来操纵mysql，更加快速和高效。

完成一个API的编写，主要包含了下面几个部分：

- 引入文件
- 接收相关参数
- 生成sql语句
- 将sql语句放入数据库中执行
- 得到返回信息

## 引入文件
这里，我们引入了[秀野堂主](www.xiuyetang.com)已经编写好的两个文件：

1. config.php ---------- php的配置文件，里面包含了数据库的地址和接收请求的地址
2. class_base.php ---------- php的一个基类，封装了很多常用，直接调用

![php引入文件](http://7xtbjo.com1.z0.glb.clouddn.com/phpAPI%E5%BC%95%E5%85%A5%E6%96%87%E4%BB%B6.png)

<!-- more -->

## 接收相关的参数
参数我理解成可以与数据库建立连接的元素，其实就是数据库里面的字段，通过它配合sql语句才能对数据库进行相关的操作。

接受单个参数，主要用于查询或者删除的时候用：
![单个参数](http://7xtbjo.com1.z0.glb.clouddn.com/%E5%8D%95%E4%B8%AA%E5%8F%82%E6%95%B0.png)

接受多个参数，主要用于插入新的数据和更新数据的时候用：
![多个参数](http://7xtbjo.com1.z0.glb.clouddn.com/%E5%A4%9A%E4%B8%AA%E5%8F%82%E6%95%B0.png)

### 生成sql语句
这里的重难点就是对于sql语句的运用，可以参考这一篇文章：
[mysql常用语句](http://jm1231.github.io/2016/04/25/mysql%E5%B8%B8%E7%94%A8%E8%AF%AD%E5%8F%A5/)

增：
![add](http://7xtbjo.com1.z0.glb.clouddn.com/%E5%A2%9E.png)

删：
![delete](http://7xtbjo.com1.z0.glb.clouddn.com/%E5%88%A0.png)

改：
![modify](http://7xtbjo.com1.z0.glb.clouddn.com/%E6%94%B9.png)

查：
![search](http://7xtbjo.com1.z0.glb.clouddn.com/%E6%9F%A5.png)

**tips：**我在这里踩到一个坑，就是表建好了之后，我又全部重新更改了各个字段的名字，

```sql
alter table tablename change originalname newname datatype(len);
```

结果导致数据表原来的自增数，默认值还有primary key等属性全部消失，折腾了半天才解决这个问题：

```sql
alter table doct_tbl change doc_id doct_id int(3) not null auto_increment;
alter table mj_doct_tbl AUTO_INCREMENT=100;
```

这里的自增语句单独写就能成功，如果和第一句合在一起写，总是会报错。
![自增](http://7xtbjo.com1.z0.glb.clouddn.com/mysql%E8%AE%BE%E7%BD%AE%E8%87%AA%E5%A2%9E.png)

## 将sql语句放入数据库中执行
这里用到了base基类里面封装的一些方法：
增：
![add](http://7xtbjo.com1.z0.glb.clouddn.com/add.png)

查：
![search](http://7xtbjo.com1.z0.glb.clouddn.com/get.png)

删、改：
![del&modi](http://7xtbjo.com1.z0.glb.clouddn.com/modify.png)

## 得到返回信息
返回的消息方便我们检查是否操作成功。
返回true or false的消息：
![bo](http://7xtbjo.com1.z0.glb.clouddn.com/%E8%BF%94%E5%9B%9E%E6%B6%88%E6%81%AF.png)

返回有具体数据：

![multi](http://7xtbjo.com1.z0.glb.clouddn.com/%E8%BF%94%E5%9B%9E%E6%B6%88%E6%81%AF2.png)









