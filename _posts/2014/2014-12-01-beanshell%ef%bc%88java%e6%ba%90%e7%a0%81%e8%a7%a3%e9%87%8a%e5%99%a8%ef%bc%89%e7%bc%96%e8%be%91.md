---
layout: post
title: BeanShell（JAVA源码解释器）编辑
date: 2014-12-01 22:16
author: admin
comments: true
categories: []
---
BeanShell是一个小巧免费的JAVA源码解释器，支持对象式的脚本语言特性，亦可嵌入到JAVA源代码中，能动态执行JAVA源代码并为其扩展了脚本语言的一些特性，像JavaScript和perl那样的弱类型、命令式、闭包函数等等特性都不在话下。
不敢再译了，具体还是参考beanshell的官方网站吧……
设置环境
l 把;bsh-xx.jar放到$JAVA_HOME/jre/lib/ext文件夹下
l unix: export CLASSPATH=$CLASSPATH:bsh-xx.jar
l windows: set classpath %classpath%;bsh-xx.jar
运行方式：
l 界面UI方式 ：java bsh.Console
l 命令行方式 ：java bsh.Interpreter
l运行脚本文件：java bsh.Interpreter filename [ args ]
