---
layout: post
title: php 使用 Register Globals
date: 2014-10-27 16:21
author: admin
comments: true
categories: []
---
可能 PHP 中最具争议的变化就是从 PHP » 4.2.0 版开始配置文件中 PHP 指令 register_globals 的默认值从 on 改为 off 了。对此选项的依赖是如此普遍以至于很多人根本不知道它的存在而以为 PHP 本来就是这么工作的。本节会解释用这个指令如何写出不安全的代码，但要知道这个指令本身没有不安全的地方，误用才会。

当 register_globals 打开以后，各种变量都被注入代码，例如来自 HTML 表单的请求变量。再加上 PHP 在使用变量之前是无需进行初始化的，这就使得更容易写出不安全的代码。这是个很艰难的抉择，但 PHP 社区还是决定默认关闭此选项。当打开时，人们使用变量时确实不知道变量是哪里来的，只能想当然。但是 register_globals 的关闭改变了这种代码内部变量和客户端发送的变量混杂在一起的糟糕情况。下面举一个错误使用 register_globals 的例子：

Example #1 错误使用 register_globals = on 的例子

<?php
// 当用户合法的时候，赋值 $authorized = true
if (authenticated_user()) {
    $authorized = true;
}

// 由于并没有事先把 $authorized 初始化为 false，
// 当 register_globals 打开时，可能通过GET auth.php?authorized=1 来定义该变量值
// 所以任何人都可以绕过身份验证
if ($authorized) {
    include "/highly/sensitive/data.php";
}
?>
