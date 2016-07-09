---
title: js学习笔记（一）
date: 2016-04-27 11:46:26
tags: JavaScript
---
1、比较运算的时候，`===`会比`==`严谨，因为后者会自动转换数据类型再比较

2、NaN的数据类型为number，但是他不与任何值相等，包括他本身，要判断是否为NaN，只能通过`isNaN()`

3、浮点数的比较要注意，只能通过比较两个差值是否小于一定的区间的方法来判断是否相等，不能直接比较，如：

     1 / 3 === (1 - 2 / 3); // false

应该写成这样:

    Math.abs(1 / 3 - (1 - 2 / 3)) < 0.0000001; // true

4、js的对象是键值组成的集合，其中键都是字符串类型，又称为对象的属性，值为任意数据类型

5、不用var申明的变量会被视为全局变量（换句话说，用var申明的变量只在当前作用域内有效），为了避免这一缺陷可能导致页面中多个同名变量比如i引起混乱的问题发生，所有的JavaScript代码都应该使用strict模式（只要在js最上面加一句’use strict'即可）
<!-- more -->
6、字符串是不可变的，如果对字符串的某个索引赋值，不会有任何错误，但是，也没有任何效果，数组就可以按照索引来操作

7、字符串的常用方法：

	indexOf( ); 用来找到指定子字符串所在的索引，不存在返回-1
	substring( ); 返回指定索引内的子字符串，[ ) 形式，如果只指定一个数，那就是从当前到结尾

8、数组可以通过操作其length属性来改变内容和长度，虽然js没有数组越界的说法，但是不建议通过这种方式来改变数组

9、数组的常用方法：

     indexOf( ); 用来找到数组中某个元素的索引值，没有则返回-1
     slice( ); 返回指定索引范围内的子数组，[ ) 形式，如果不指定数值，则从头截取到尾 = 复制了这个数组
     push( ); 向数组尾部插入元素 ——unshift ( )
     pop( ); 从数组尾部开始删除一个元素，如果对空数组用pop( ) ,则会返回undefined ——shift ( )
     sort( ); 数组内元素排序，可以自定义排序函数
     reverse( ); 给数组内部的元素整个颠倒顺序
     splice( ); 堪称数组的【万能公式】，可以只删除元素，也可以只添加元素，也可以删除并添加元素
     concat( ); 用于把当前数组和任意个元素和数组连接并返回新数组
     join( ); 将当前数组的元素以指定的符号连接成一个字符串

10、对象访问属性时通过.操作符完成的，但这要求属性名必须是一个有效的变量名。如果属性名包含特殊字符，就必须用'’括起来，比如：

	var xiaohong = {
	    name: '小红',
	    'middle-school': 'No.1 Middle School'
	};

在访问middle-school必须通过xiaohong[‘middle-school’]的方式访问，不能用xiaohong.middle-school，而name属性既可以用xiaohong.name来访问也可以用xiaohong[’name’]来----所以写属性名字是尽量依据标准

JavaScript对象的所有属性都是字符串，不过属性对应的值可以是任意数据类型。

js的对象是动态的，可以自由添加或者删除属性，比如：

	var xiaoming = {
	    name: '小明'
	};
	xiaoming.age; // undefined
	xiaoming.age = 18; // 新增一个age属性
	xiaoming.age; // 18
	delete xiaoming.age; // 删除age属性
	xiaoming.age; // undefined
	delete xiaoming['name']; // 删除name属性
	xiaoming.name; // undefined
	delete xiaoming.school; // 删除一个不存在的school属性也不会报错

当我们要检测对象是否有某一个属性时，可以使用in操作符，比如：

	‘name’ in xiaohong →→→ 存在则返回true，否则返回false
	
但是！用in无法判断出该属性是否为对象的自有属性，因为可能是从object对象中继承得来的，所以这里有另一个方法 hasOwnProperty( ) ,比如：

	xiaohong.hasOwnProperty ( ’name’ ) →→→ 存在则返回true，否则返回false

11、JavaScript把null、undefined、0、NaN和空字符串''视为false，其他值一概视为true，因此上述代码条件判断的结果是true。

12、for 循环的变体 →→→ for in，用来循环遍历对象中的每一个属性，数组也是一个对象，所以也可以拿来循环数组的索引，注意，数组循环出来的数据类型都是string，不是number

13、关键字argument 表示当前函数调用者传入的所有参数，类似于array但不是array

14、js函数中有一个特点就是首先会扫描整个函数体，然后把var申明提升到最前面，但并不会连同赋值一起放到最前面

15、命名空间的问题：就是通过创建一个对象的形式开辟一个自己的全局空间，并把所有的变量和函数都绑定到自己的空间上去，这样会有效的避免变量名重复的冲突，比如：

	// s属于自己的一个唯一全局变量MYAPP:
	var MYAPP = {};
	
	// 其他变量:
	MYAPP.name = 'myapp';
	MYAPP.version = 1.0;
	
	// 其他函数:
	MYAPP.foo = function () {
	    return 'foo';
	};


16、对象上绑定的函数我们称之为 方法 ，方法内部如果用到关键字 this 要特别注意这个 this 的指向问题。
我们可以通过apply ( )和 call( ) 的方法来改变this的指向。
要指定函数指向哪个对象，可以调用函数本身的apply( )方法，他接收两个参数，第一个表示需要绑定的this的变量，第二个表示函数本身的参数：

	function getAge() {
	    var y = new Date().getFullYear();
	    return y - this.birth;
	}
	
	var xiaoming = {
	    name: '小明',
	    birth: 1990,
	    age: getAge
	};
	
	xiaoming.age(); // 25
	getAge.apply(xiaoming, []); // 25, this指向xiaoming, 参数为空

和apply类似的还有call，唯一的区别就是apply参数是打包成一个数组，而call则是按顺序传入：

	Math.max.apply(null, [3, 5, 4]); // 5
	Math.max.call(null, 3, 5, 4); // 5

注意，对普通函数的调用，我们通常将this绑定为null
