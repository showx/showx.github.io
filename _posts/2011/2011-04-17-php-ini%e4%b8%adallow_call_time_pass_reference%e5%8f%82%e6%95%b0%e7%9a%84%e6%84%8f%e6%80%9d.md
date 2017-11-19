---
layout: post
title: php.ini中allow_call_time_pass_reference参数的意思
date: 2011-04-17 06:27
author: admin
comments: true
categories: []
---
从php手册中可以找到：
allow_call_time_pass_reference boolean
是否启用在函数调用时强制参数被按照引用传递。此方法已不被赞成并在 PHP/Zend 未来的版本中很可能不再支持。鼓励使用的方法是在函数定义中指定哪些参数应该用引用传递。鼓励大家尝试关闭此选项并确保脚本能够正常运行，以确保该脚本也能在未来的版本中运行（每次使用此特性都会收到一条警告，参数会被按值传递而不是按照引用传递）。

在函数调用时通过引用传递参数是不推荐的，因为它影响到了代码的整洁。如果函数的参数没有声明作为引用传递，函数可以通过未写入文档的方法修改其参数。要避免其副作用，最好仅在函数声明时指定那个参数需要通过引用传递。

当allow_call_time_pass_reference=Off时
<?php
function abc($a,$b){
    echo "$a\n";
    echo "$b\n";
    $b = 'cde';
}

$a = 'abc';
$b = "bcd";
//不好的用法，会引发一个php warnning
abc($a ,&$b);

echo "$b\n";
?>
要想通过引用来传递参数$b，程序可改为
<?php
function abc($a,& $b){
    echo "$a\n";
    echo "$b\n";
    $b = 'cde';
}

$a = 'abc';
$b = "bcd";
//正确的用法
abc($a ,$b);

echo "$b\n";
//output is:
//abc
//bcd
//cde
?>
