---
title: API编写过程中的错误记录与总结
date: 2016-05-26 20:17:16
tags: [MySQL,API,php]
---
## 错误记录
### sql语句错误
1. 关键词后面没有加空格
2. 中英文标点混乱
3. 表名与字段名没有对应
4. 对应接口中的方法名写错

### 传值
1. 传值失败 

## 总结
### 工具使用
1. 定义好了代码片段可以直接通过快捷键将大段代码快速的堆放到页面中，效率高（代码需要长期的积累，将常用的保存使用）
2. 使用`ctrl + alt + a`快捷键用来排版，优化代码界面

<!-- more -->

### API编写过程中思想的转变
#### 第一版

```php
<?php 
//引用想关的文件
require_once('../config.php');
require_once('../public/class_base.php');
//接受相关的数据
$h_id       = $_REQUEST['h_id'];

//生成sql语句
$query_h_id = "h_id = ".$h_id;
$sql        = "select * from mj_hosp_tbl where ".$query_h_id;

//丢到数据库里去查询
$rs         = Base::getSingleRs($sql);

//得到返回的信息
if (!$rs) {
	echo "no data";
}
else{
	echo json_encode($rs);
}
?>
```

一开始接触API编写时，都是单个接口对应单个文件的形式，这样造成的后果就是，当API变得繁多，文件数量庞大，而且每次使用不同API都需要请求不用的文件。

#### 第二版
> 第二版的改进思路：能不能只请求一个文件就能调用不同的API

所以我们定义了一个HIS类，将所有API都封装到对应的函数中，然后在一个页面`his_route.php`中引入类文件，并通过`method`参数传入的数值来调用相对应的api。

```php
<?php 
class HIS_MJ extends Base
{
	function __construct()
	{
		# code...
	}

	public static function hosp_get_info($h_id)
	{
		$query_h_id = "h_id = ".$h_id;
		$sql        = "select * from mj_hosp_tbl where ".$query_h_id;
		$rs         = Base::getSingleRs($sql);
		if (!$rs) {
			$result['msg']    = '失败';
			$result['status'] = 0;
			$result['data']   = $sql;
		}
		else {
			$result['msg']    = '成功';
			$result['status'] = 1;
			$result['data']   = $rs;
		}
	    return $result;
	}
	
	function __destruct()
	{
		# code...
	}
}
```

```php
<?php 
require_once('../config.php');
require_once('../public/class_base.php');
require_once('../class/class_his_mj.php');
$method = $_REQUEST['method'];
switch ($method) {
case 'hosp_get_info':
		echo json_encode(HIS_MJ::hosp_get_info($_REQUEST['h_id']));
		break;
case 'hosp_insert_info':
		echo json_encode(HIS_MJ::hosp_insert_info($_REQUEST['h_name'],$_REQUEST['h_pic'],$_REQUEST['h_rate'],$_REQUEST['h_feature'],$_REQUEST['h_tel'],$_REQUEST['h_street'],$_REQUEST['h_desc'],$_REQUEST['id']));
		break;	
default:
		# code...
		break;	
}
?>
```

待改进部分：多个参数传递的方式改为数组形式看起来会更整洁清晰！