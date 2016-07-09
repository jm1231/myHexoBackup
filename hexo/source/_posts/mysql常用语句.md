---
title: mysql常用语句
date: 2016-04-25 07:59:02
tags: MySQL
---
对于我这样的菜鸟来说，我觉得很有必要先解释一下`mysql`是一个什么玩意儿，维基上面是这酱紫解释的：
> MySQL（官方发音为英语发音：/maɪ ˌɛskjuːˈɛl/“My S-Q-L”，但也经常读作英语发音：/maɪ ˈsiːkwəl”），是一个关系数据库管理系统。
>
> 体积小、速度快、总体拥有成本低，开放源码
>
> 目前Internet上流行的网站构架方式是LAMP（Linux Apache MySQL PHP），即是用Linux作为操作系统，Apache作为Web服务器，MySQL作为数据库，PHP（部分网站也使用Perl或Python）作为服务器端脚本解释器。除了LAMP之外，用于Solaris、Windows和Mac上的网站构架也分别被称为SAMP、WAMP和MAMP。

<!-- more -->

ok，暂时就先了解这么多，更深入的体会要在使用中才能摸熟。这几天有结合`node.js`来操纵`mysql`，下面就先总结一下常用的几个数据库操作语句,后面有用到新的再往上面加。

#### 1、启动服务
	mysql.server start

#### 2、连接数据库
	mysql -uroot [-p密码 -h地址];

	如果没有设置密码，超级用户root是可以不用密码就可以进入数据库

#### 3、修改用户密码
	mysqladmin -uroot [-p旧密码] password 新密码;

	如果一开始没有设置密码，那-p这一项就可以省略直接写新密码就行

#### 4、创建数据库
	create database 数据库名;

#### 5、显示数据库
	show databases;

#### 6、删除数据库
	drop database 数据库名;

#### 7、使用数据库
	use 数据库名;

#### 8、创建数据表
	create table tbname (字段名 数据类型(长度) [是否为空 是否主键 自动增加 默认值]);
	
	show create table tablename;

#### 9、显示数据表
	show tables;

#### 10、获取表结构
	desc 表名;

#### 11、删除数据表
	drop table 数据表名;

#### 12、向数据表中插入数据
	//插入单字段数据
	insert into 数据表名 (字段名) values (值);
	
	//插入多字段数据
	insert into 数据表名 (字段名1，字段名2) values (值1，值2);

#### 13、查询表中的数据
	select 字段名 from 数据表名 [where 表达式][offset m][limit n];

	offset表示偏移量，一般设置为0，limit表示要返回的记录数

#### 14、删除记录
	delete from 记录表名 [where 表达式];
	
#### 15、修改表中的数据
	//更新单字段数据
	update 数据表名 set 字段=新值 where 表达式;
	
	//更新多个字段数据
	update 数据表名 set 字段1=值1,字段2=值2,字段3=值3 where 表达式;
	
#### 16、增加表中的字段
	alter table 表名 add 字段名 数据类型(长度) [是否为空 是否主键 自动增加 默认值];
	*可以通过设置关键字`first`将新增加的字段放在第一位，`after 字段名`放置在该字段名后
	
	alter table 表名 drop 字段名;	
	*当表里面只有一个字段时无法用drop来删除
	
	alter table 表名 modify 原字段名 新数据类型(长度);
	alter table 表名 change 原字段名 新字段名 新数据类型(长度);
	
	alter table 原表名 rename to 新表名;
	
---
#tips:语句结束一定要加分号`;`哦！！！	
	
