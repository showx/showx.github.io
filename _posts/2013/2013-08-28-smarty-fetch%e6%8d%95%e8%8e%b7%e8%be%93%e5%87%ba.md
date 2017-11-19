---
layout: post
title: smarty fetch捕获输出
date: 2013-08-28 21:03
author: admin
comments: true
categories: []
---
虽然很小用到，但还是讲一下。。

<?php
include_once 'init_smarty.php';

$smarty->assign('title','标题');
$smarty->assign('content','内容');

$output = $smarty->fetch('index.html');

echo $output;
// $smarty->display('index.html');
?>

使用fetch函数，你可以将要输出的html赋值给一个变量，可以对里面的数据进行一些输出，再将他输出。
smarty中的display方法，实际上调用的就是fetch，只不过是直接将他显示出来而已，而fetch默认是不显示，返回给一个变量的。
