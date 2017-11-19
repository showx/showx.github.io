---
layout: post
title: PHP报错：Only variables can be passed by reference的含义
date: 2014-09-08 20:36
author: admin
comments: true
categories: []
---
在PHP里，如果运行以下代码：

?View Code PHP

< ?php
function eee(&$t)
{
 $w = 'hello '.$t;
 return $w;
}
echo eee('World');

将会在页面上报出错误，如下：
Fatal error: Only variables can be passed by reference in ……
意思是“只有变量能通过‘引用’”，也就是说，这时，我们只能往eee函数里传入变量才能得到正确结果，因为eee函数需要的参数是一个内存地址。 
