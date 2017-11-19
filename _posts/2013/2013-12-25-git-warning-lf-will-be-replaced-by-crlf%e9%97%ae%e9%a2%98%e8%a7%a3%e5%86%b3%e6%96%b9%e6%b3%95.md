---
layout: post
title: [GIT] warning: LF will be replaced by CRLF问题解决方法
date: 2013-12-25 14:17
author: admin
comments: true
categories: []
---
开发环境：
操作系统： windows xp
ruby 1.9.2
rails 3.1.3
git version 1.7.8.msysgit.0

问题描述：
启动GIT：

新建了一个rails工程

Ruby代码 收藏代码
$ rails new blog

当切换到blog目录下执行

Ruby代码 收藏代码
$ git init
$ git add .

系统出现如下错误：warning: LF will be replaced by CRLF

原因分析：
CRLF -- Carriage-Return Line-Feed 回车换行
就是回车(CR, ASCII 13, \r) 换行(LF, ASCII 10, \n)。
这两个ACSII字符不会在屏幕有任何输出，但在Windows中广泛使用来标识一行的结束。而在Linux/UNIX系统中只有换行符。
也就是说在windows中的换行符为 CRLF， 而在linux下的换行符为：LF
使用git来生成一个rails工程后，文件中的换行符为LF， 当执行git add .时，系统提示：LF 将被转换成 CRLF

解决方法：

删除刚刚生成的.git文件

Ruby代码 收藏代码
$ rm -rf .git
$ git config --global core.autocrlf false

这样系统就不会去进行换行符的转换了
最后重新执行

Ruby代码 收藏代码
$ git init
$ git add .

系统即可正常运行！
