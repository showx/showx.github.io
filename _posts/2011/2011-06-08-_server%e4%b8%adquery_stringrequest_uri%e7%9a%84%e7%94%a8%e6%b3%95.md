---
layout: post
title: $_SERVER中QUERY_STRING,REQUEST_URI的用法
date: 2011-06-08 06:41
author: admin
comments: true
categories: []
---
在写程序的过程中经常会用到$_SERVER函数，有时候对变量不太了解就会造成很大的误解。今天偶找了几个小例子来说明一下常用到的四个变量的用法~~ 
$_SERVER存储当前服务器信息，其中有几个值如

$_SERVER["QUERY_STRING"]，

$_SERVER["REQUEST_URI"]，

$_SERVER["SCRIPT_NAME"],

$_SERVER["PHP_SELF"]

常常容易混淆，以下通过实例详解$_SERVER函数中QUERY_STRING，REQUEST_URI，SCRIPT_NAME和PHP_SELF变量区别，掌握这四者之间的关系，便于在实际应用中正确获取所需要的值，供参考。

1，$_SERVER["QUERY_STRING"]

说明：查询(query)的字符串

2，$_SERVER["REQUEST_URI"]

说明：访问此页面所需的URI

3，$_SERVER["SCRIPT_NAME"]

说明：包含当前脚本的路径

4，$_SERVER["PHP_SELF"]

说明：当前正在执行脚本的文件名

实例：

1，http://ask.mbatrip.com (打开主页)

结果：

$_SERVER["QUERY_STRING"] = “”

$_SERVER["REQUEST_URI"]  = “/”

$_SERVER["SCRIPT_NAME"]  = “/index.php”

$_SERVER["PHP_SELF"]     = “/index.php”

2，http://ask.mbatrip.com/?tags/上传(附带查询)

结果：

$_SERVER["QUERY_STRING"] = “tags/上传″

$_SERVER["REQUEST_URI"]  = “/?tags/上传″

$_SERVER["SCRIPT_NAME"]  = “/index.php”

$_SERVER["PHP_SELF"]     = “/index.php”

3，http://ask.mbatrip.com/?tags/上传/2

结果：

$_SERVER["QUERY_STRING"] = “tags/上传/2”

$_SERVER["REQUEST_URI"]  = “/index.php?tags/上传/2”

$_SERVER["SCRIPT_NAME"]  = “/index.php”

$_SERVER["PHP_SELF"]     = “/index.php”

$_SERVER["QUERY_STRING"]获取查询语句，实例中可知，获取的是?后面的值

$_SERVER["REQUEST_URI"] 获取http://ask.mbatrip.com后面的值，包括/

$_SERVER["SCRIPT_NAME"] 获取当前脚本的路径，如：index.php

$_SERVER["PHP_SELF"] 当前正在执行脚本的文件名

总结一下，对于QUERY_STRING，REQUEST_URI，SCRIPT_NAME 和PHP_SELF，深入了解将有利于我们在$_SERVER函数中正确调用这四个值。通过实例详解$_SERVER函数中 QUERY_STRING，REQUEST_URI，SCRIPT_NAME和PHP_SELF掌握四个变量之间的区别。


