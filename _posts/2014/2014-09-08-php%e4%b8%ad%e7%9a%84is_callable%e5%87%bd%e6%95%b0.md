---
layout: post
title: PHP中的is_callable函数
date: 2014-09-08 19:33
author: admin
comments: true
categories: []
---
今天在看Yii源码的时候，看到一个函数is_callable，发现从网上下的chm格式的帮助手册的解释几乎是0，因此g之，发现也木有比较全面的文档，因此翻译一下官网的最新手册吧：
 
原文地址：http://php.net/manual/en/function.is-callable.php
 
is_callable

(PHP 4 >= 4.0.6, PHP 5)
is_callable — 验证变量的内容是否能够进行函数调用
 
Description
bool is_callable ( callback $name [, bool $syntax_only = false [, string &$callable_name ]] )

验证变量的内容是否能够进行函数调用。可以用于检查一个变量是否包含一个有效的函数名称，或者一个包含经过合适编码的函数和成员函数名的数组。
Parameters（参数）
name
既可以是一个字符串类型的函数名称，也可以是一个对象和成员函数名的组合数组，比如：array($SomeOject, 'MethodName')
syntax_only
如果设置为true，那么只是验证name是一个函数或者方法，函数仅仅会拒绝不是字符串，亦或是结构不合法的数组作为回调函数。合法结构是指一个包含两个成员的数组，第一个是对象或者字符串，第二个是一个字符串。
 
callable_name
接收“调用名称”，在下面的例子里它是“someClass::someMethod"。请注意尽管someClass::someMethod()是一个可调用的静态方法，但是这里并不是真的表示一个静态方法
 
Return Values（返回值）
 
如果可调用返回true，否则返回false。
 
 
Examples
 
Example #1 is_callable() example

<?php

//  How to check a variable to see if it can be called

//  as a function.


//

//  Simple variable containing a function

//


function 
someFunction
() 

{

}


$functionVariable 
= 
'someFunction'
;


var_dump
(
is_callable
(
$functionVariable
, 
false
, 
$callable_name
));  
// bool(true)


echo 
$callable_name
, 
"\n"
;  
// someFunction


//

//  Array containing a method

//


class 
someClass 
{


  function 
someMethod
() 

  {

  }


}


$anObject
=new 
someClass
();


$methodVariable
=array(
$anObject
,
'someMethod'
);


var_dump
(
is_callable
(
$methodVariable
,
tru
e,$callable_name
));  
//bool(true)


echo 
$callable_name
, 
"\n"
;  
//someClass::someMethod


?>


参考：

I haven't seen anyone note this before, but 
is_callable will correctly determine the existence of methods made with 
__call. The method_exists function will not.
is_callable判断是回去调用__call魔术方法来判断，而method_exists不会
