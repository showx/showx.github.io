---
layout: post
title: php str_replace
date: 2014-12-03 17:39
author: admin
comments: true
categories: []
---
str_replace @1 @2 @3 @4 ，  第四个参数要用变量,不然报错。

str_replace("aa","bb","aasfd","1");这样不行

$i = 1;
str_replace("aa","bb","aasfd",$i);

