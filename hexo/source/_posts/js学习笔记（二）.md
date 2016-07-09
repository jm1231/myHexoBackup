---
title: js学习笔记（二）
date: 2016-04-29 18:47:06
tags: JavaScript
---
#### 1. 数组中的几个高阶函数（高阶函数可以接受函数作为参数)  

1.1	➡️ `map( )` && `filter ( )`  
	功能相近，都是接受一个函数作为参数，区别就在**map**是将函数作用在数组的每一个元素上并返回新的数组，而**filter**是作用后只返回为true的元素。  

1.2➡️ `reduce ( )`  
    接受两个参数，先将数组的前两项运算，然后在将计算出的结果和第三项计算，以此类推  

1.3➡️ `sort( )`   
    数组排序，默认是按照ascii码进行排序，可以通过函数参数自定义排序方式
<!-- more -->
#### 2. 标准对象和包装对象  
2.1➡️ 不要使用`new Number()`、`new Boolean()`、`new String()`创建包装对象；

2.2➡️ 用`parseInt()`或`parseFloat()`来转换任意类型到number；
	
2.3➡️ 用`String()`来转换任意类型到string，或者直接调用某个对象的`toString()`方法；
	
2.4➡️ 通常不必把任意类型转换为boolean再判断，因为可以直接写`if (myVar) {...}`；
	
2.5➡️ `typeof`操作符可以判断出number、boolean、string、function和undefined；
	
2.6➡️ 判断Array要使用`Array.isArray(arr)`；
	
2.7➡️ 判断null请使用`myVar === null`；

2.8➡️ 判断某个全局变量是否存在用`typeof window.myVar === 'undefined'`；
	
2.9➡️ 函数内部判断某个变量是否存在用`typeof myVar === 'undefined'`。
	
2.10➡️ 任何对象都有`toString()`方法，但是null和undefined就没有！
	
1.11➡️ number对象调用toString()会报错，需要写成`123..toString( )`或者`(123).toString( )`；

#### 3. date对象的使用要注意 
3.1➡️ getMonth( )的时候，月份是0～11的，所以表示当前月份要 ＋1

3.2➡️ js获取当前时间是浏览器从本机操作系统获取的时间，所以不一定准确，因为用户可以把当前时间设定为任何值，有两个方法可以创建指定日期的时间对象:

   
	var d = new Date(2015, 5, 19, 20, 15, 30, 123);
	d; // Fri Jun 19 2015 20:15:30 GMT+0800 (CST)
	
和

	var d = Date.parse('2015-06-24T19:49:22.875+08:00');
	d; // 1435146562875

但 , 它返回的不是Date对象，而是一个时间戳。不过有时间戳就可以很容易地把它转换为一个Date：

	var d = new Date(1435146562875);
	d; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)


#### 4. json中的数据格式包括

4.1➡️ number：和JavaScript的number完全一致；
	 
4.2➡️ boolean：就是JavaScript的true或false； 
	 
4.3➡️ string：就是JavaScript的string； 
	 
4.4➡️ null：就是JavaScript的null； 
	 
4.5➡️ array：就是JavaScript的Array表示方式——[]；
	  
4.6➡️ object：就是JavaScript的{ ... }表示方式。

并且，JSON还定死了字符集必须是`UTF-8`，表示多语言就没有问题了。为了统一解析，JSON的字符串规定必须用双引号`""`，Object的键也必须用双引号`""`。
	
	序列化：：
	JSON.stringify(xiaoming, ['name', 'skills'], '  ');

	反序列化：：
	JSON.parse('{"name":"小明","age":14}');


#### 5. BOM:

5.1➡️ window：innerWidth/innerHeight/outerWidth/outerHeight

5.2➡️ navigator: 显示浏览器的各种信息（因为这些信息容易被篡改，所以并不十分准确）

5.3➡️ screen: screenHeight/screenWidth（Retina屏幕不能正确计算，比如2880\*1800的只能识别出1440\*900）

5.4➡️ location: href / reload( ) / assign( )

5.5➡️ document: title / cookie( document.cookie可以查看当前页面的cookie，不安全，服务器端应该设置httpOnly ) / DOM

5.6➡️ history: back( ) / forward( )

     
#### 6. **原型和继承**（js中的面向对象）
#### 7. **闭包**

_ _ _
# 后面6&&7两部分比较重要而且相对难理解，会留到后面单独开辟一篇来说 