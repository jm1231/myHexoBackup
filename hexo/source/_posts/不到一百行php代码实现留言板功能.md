---
title: 不到一百行php代码实现留言板功能
date: 2016-06-09 21:22:10
tags: php
---
乍一看这个标题，赶脚很牛逼，有木有？！其实，我特么熬了5个小时！！！

摔！！

喝口茶，平静一下，我们来慢慢分析。

## 需求分析
留言板功能我们都熟悉，无非就是两大功能点：

1. 写留言
2. 回复留言

## 拆分实施步骤
围绕上面的两大功能点，我们可以更具体的想一下下一步该怎么走。

<!-- more -->

### 写留言
写留言这个功能，抽象来看，就是将填写的留言数据存到数据库中，然后再返回页面进行显示的一个过程。

继续往下想，要将数据进行存储的时候，需要哪些字段，最重要的应该就是id，name和comments了。

### 回复留言
当我们已经从数据库中读取出来了n条留言，需要对其中的一条或者多条进行回复，这个功能又该怎么和数据库联系起来？

其实也不难想，首先要回复哪一条留言，先要将他的id拿到，把id作为fid给最新的这一条回复。这里又牵扯到了fid这一个字段。

## 建立数据表
分析完了需求和实施步骤之后发现，要实现这个功能，最主要的几个字段就是id,name,comments和fid。了解这个之后我们就可以开始建表。

```sql
create table comments (
    id int(3) primary key not null AUTO_INCREMENT,
    name varchar(20) not null,
    comments varchar(200) not null,
    fid int(3) not null
)AUTO_INCREMENT=100;

```

## 具体实践
### 先实现发布留言

按照上面的分析，我们要将数据存入数据库再回显。首先想到的就是需要一个form表单来提交留言信息。

```html
<form action="oper.php" method="post">
	<textarea name="comments"></textarea>
	<input type="submit" name="action" value="提交">
<form>
```

这是最基本的框架。

要想将数据写进数据库，首先就应该将内容传进去。通过表单，后台可以接收到文本域里面的内容`$comments = $_REQUEST['comments']`。现在我们已经拿到了数据，关键是怎么插入到数据库中去。可以定义一个`insert函数`，用来连接数据库，并将写好的sql语句丢进去执行，返回结果并跳转到上一级页面回显数据。

*insert()函数用来插入数据 ↓↓↓↓↓↓*

```php
function insert($param)
{
    $sql = "insert into comments (name,comments,fid) values ('".$param['name']."','".$param['comments']."','".$param['fid']."')";
    $DB = new mysql("192.168.0.105", 'root', "111111", "mjdb", "conn", "utf8");
    $DB->longquery($sql);
    $feedback = $DB->insert_id();
    return $feedback;
}
```

*发布时候要做的事，接收相关参数丢到数据库执行并返回数据到页面 ↓↓↓↓↓↓*

```php
case '发布':
        $rs['comments'] = $_REQUEST['comments'];
        $rs['name'] = $_REQUEST['name'];
        $rs['fid'] = $_REQUEST['fid'];
        $feedback = insert($rs);
        echo "<script>window.location = 'comments.php';</script>";
        break;
```

到这里，就已经可以接收值并且回传到页面中了。但是我们上面html片段中，还差name和fid没有传过去，完善一下：

```html
<form action="oper.php" method="post">
	<textarea name="comments"></textarea>
	<input type="hidden" name="name" value="mj">
	<input type="hidden" name="fid" value="fid">
	<input type="submit" name="action" value="提交">
<form>
```

这里有个问题，一开始数据表中没有数据的时候，fid怎么办？没有值，所以我想着现在页面中自定义一个`fid=0`作为初始默认的值，对应的表单应该作出调整：

`<input type="hidden" name="fid" value=".$fid.">`

### 开始回复留言
要回复留言，肯定要涉及到id的传递，不然无法判断是对哪一条留言进行的回复。

**我的思路**  
> 点击某条留言回复 → 将id带着跳转到回复框 → 输入回复内容 → 点击reply

再点击reply之后发生了什么？或者说我们希望发生什么，
其实就是要将回复的内容(comments)，回复的对象id传递到后台，插入数据库并回显。

所以我们的表单结构又需要进行调整：

```html
<form action="oper.php" method="post">
	<textarea name="comments"></textarea>
	<input type="hidden" name="name" value="mj">
	<input type="hidden" name="id" value=".$id.">
	<input type="hidden" name="fid" value=".$fid.">
	<input type="submit" name="action" value="提交">
	<input type='submit' name='action' value='reply'>
<form>
```
*回复留言其实也是在数据库中插入了一条新的数据，所以只要调用上文中提到的insert()就行*

```php
case 'reply':
        $rs['fid'] = $_REQUEST['id'];
        $rs['name'] = $_REQUEST['name'];
        $rs['comments'] = $_REQUEST['comments'];
        $feedback = insert($rs);
        echo "<script>window.location = 'comments.php';</script>";
        break;
```

至此，回复留言的模块也快要搞定了。我们希望回复留言之后，内容应该是在对应的留言下方缩进显示，这样会有明显的层级关系，但是我们现在做出来的还仅仅是回复之后作为一条新数据显示在最后面，无法看出是对哪一条记录的回复。要解决这个问题，就要用到***递归***。

**使用递归的思路：**
> 回复留言时，我们是将回复对象的id作为fid来使用，这样就有一个关联性，对于数据库来说，很容易就能判断出来，你回复的对象是谁，所以我们从此处入手，编写递归函数。

```php
function go($fid, $dep=0){
    $sql = "select * from comments where fid = ".$fid;
    $DB = new mysql("192.168.0.105", 'root', "111111", "mjdb", "conn", "utf8");
    $DB->longquery($sql);
    $rs = $DB->get_assoc_result();
    $rows = count($rs);
    if($rows>0){
        $dep++;
        for ($i=0; $i < $dep; $i++) {
            $str .= "--------";
        }
        for ($i=0; $i < $rows ; $i++) {
            echo $str.$rs[$i]['comments']."'<a href='oper.php?action=del&id=".$rs[$i]['id']."'>删除</a>
             <a href='comments.php?id=".$rs[$i]['id']."'>reply</a><br>";
            echo go($rs[$i]['id'], $dep);
        }
    }
}
```

上面代码的含义就是，首先从数据库中取出初始`$fid=0`时的数据，计算出数据的条数，然后开始循环，循环的过程中又会自己调用一遍函数`go()`,但此时应该是将id作为fid来查找，如果有记录就继续往下显示。

注意到上面还传了一个参数`$dep`，这是用来计算递归的深度，方便显示层级关系时使用。

## 总结
这样一个程序写下来，应该有三个地方需要注意：

1. 前后台如何传值
2. php操作数据库（包括连接和使用）
3. 递归函数的应用

个人感觉，第三条最重要，它涉及到如何理解递归和如何正确的使用递归。


