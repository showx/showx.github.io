---
layout: post
title: setlocal/endlocal
date: 2013-01-20 00:53
author: admin
comments: true
categories: []
---
主要说明setlocal/endlocal命令的作用。setlocal和endlocal命令执行结果是让中间的程序对于系统变量的改变只在程序内起作用，不会影响整个系统级别。

比如这个例子中，在第二行setlocal之后，第三行对于变量path进行了赋值，第四行就是显示一下该值。在第六行endlocal后（此行，应该没有空格），重新显示一下系统变量path（第七行），会发现仍然是程序运行之前的path值，没有被程序改变
