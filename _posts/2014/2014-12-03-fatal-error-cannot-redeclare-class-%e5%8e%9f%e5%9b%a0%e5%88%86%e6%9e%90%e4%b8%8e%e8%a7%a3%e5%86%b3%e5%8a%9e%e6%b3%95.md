---
layout: post
title: Fatal error: Cannot redeclare class 原因分析与解决办法
date: 2014-12-03 11:25
author: admin
comments: true
categories: []
---
我使用的都是php __autoload状态自动加载类的，今天好好的程序不知道怎么在运行时提示Fatal error: Cannot redeclare class 了，看是重复定义了类，下面我来分析一下解决办法。

 

错误提示
Fatal error: Cannot redeclare class ….
从字面来看也很好理解，说明是重复定义了类，找了一下自己的代码，是因为存在同名的类导致的，修改了类名就好了。
原因分析
1.在同一个文件中重复声明了两次同名的类：
例如：
 
 代码如下	复制代码
<?php   
class Foo {}   
  
// some code here   
  
class Foo {}   
?> 
在第二个 Foo 的地方就会报错。
解决：去掉第二个Foo，或者重命名。
为了防止重复定义，可以在定义一个新的类的时候判断一下这个类是否已经存在：
 
 代码如下	复制代码
if(class_exists('SomeClass') != true)   
{   
   //put class SomeClass here   
}  
if(class_exists('SomeClass') != true)
{
   //put class SomeClass here
}

2.重复包含相同的类文件：
例如：对于某个类文件some_class.php，在a.php中
 代码如下	复制代码
include "some_class.php";  
include "some_class.php";
在b.php中
 代码如下	复制代码

include "a.php";   
include "some_class.php";  
include "a.php";
include "some_class.php";
就会报错。
解决：将上述的include全部替换为include_once
3.该类为PHP类库中内置的类。
判断方法：在一个空文件中写入
 
 代码如下	复制代码
<?php   
class Com   
{   
  
}   
?> 
这时候提示Cannot redeclare class Com，说明这个类就是PHP内置的类。不能使用。
另外，要避免使用太大众化的类名，比如Com，这个类在Linux使用可能是正常的，在Windows环境却无法运行。

再记一个网上找到的解决方法，可能在某些场合有用，先记着
 代码如下	复制代码

if (!class_exists('pageModule')){     
require_once(PATH_site.'fileadmin/scripts/class.page.php');
 }
上面的办法不适用于使用了php __autoload类加载的方法 ,但己经可以解决办法问题了，__autoload是自动加载的我们只要把相同类名找出来然后重命名即可。
