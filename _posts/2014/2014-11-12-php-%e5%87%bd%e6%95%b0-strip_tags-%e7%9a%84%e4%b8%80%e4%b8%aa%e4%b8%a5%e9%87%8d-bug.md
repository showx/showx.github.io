---
layout: post
title: PHP 函数 strip_tags 的一个严重 bug
date: 2014-11-12 17:38
author: admin
comments: true
categories: []
---
PHP 函数 strip_tags 提供了从字符串中去除 HTML 和 PHP 标记的功能，该函数尝试返回给定的字符串 str 去除空字符、HTML 和 PHP 标记后的结果。

由于 strip_tags() 无法实际验证 HTML，不完整或者破损标签将导致更多的数据被删除。

比如下述代码：

<div>string</div>string<string<b>hello</b><div>string</div>
通过 strip_tags($str, ‘<div>’) 过滤，我们可能期望得到如下结果：

<div>string</div>string<stringhello<div>string</div>
而实际操作结果是这样的：

<div>string</div>string
这一切都是因为加红的那个左尖括号，查了 PHP 的文档，有一个警告提示：

由于 strip_tags() 无法实际验证 HTML，不完整或者破损标签将导致更多的数据被删除。

既然在执行过滤前无法验证代码正确性，遇到和标签相关的字符 “<” 或 “>” 后面的代码就全挂了！

参考资料：http://php.net/manual/zh/function.strip-tags.php
