---
layout: post
title: Resource interpreted as Script but transferred with MIME type text/html
date: 2014-08-28 09:23
author: admin
comments: true
categories: []
---
这有由于HTTP Response Header中指定的MIME 类型 与文件类型不匹配
不同的浏览器可能返回姐结果不一样
FireFox，Chrome，会遇到上述问题
HTML页面在加入了<!DOCTYPE html>或者其他指定HTML页面文档类型的说明时，才能保证，在IE和其他浏览器HTML页面的样式展现上保持一致


在开发的一个项目中有用PHP生成JS代码的功能，程序在引用的时候一切正常。但是在Chrome浏览器里显示：＂Resource interpreted as Script but transferred with MIME type text/html: ….. ＂。

这是由于服务器端给你发回的http响应的content-type值是text/plain（默认）而你所期望返回的是兼容javascript类型的。所以浏览器提示上面的警告信息。

解决方法，可以在服务器端的返回字段里增加：Content-type:application/x-javascript。由于我使用的是PHP所以在代码前面使用header输出:


header('Content-type:application/x-javascript');

问题
