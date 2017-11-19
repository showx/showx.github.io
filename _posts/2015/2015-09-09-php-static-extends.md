---
layout: post
title: php static extends
date: 2015-09-09 11:06
author: admin
comments: true
categories: []
---
工作上遇到的问题：
class a{public static $a='1';public static function a(){echo self::$a}
class b exntends a{public static $a='2'}

b::a();
输出是1,
原因在parent self,静态方法要使用static
把class a的self改为static即可
