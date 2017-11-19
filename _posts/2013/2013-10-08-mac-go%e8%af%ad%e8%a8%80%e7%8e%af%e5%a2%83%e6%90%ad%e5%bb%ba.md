---
layout: post
title: mac go语言环境搭建
date: 2013-10-08 10:28
author: admin
comments: true
categories: []
---
在Go 1发布之前,开发者要想使用Go,只能自行下载代码并进行编译,而现在可以直接下 载对应的安装包进行安装,安装包的下载地址为http://code.google.com/p/go/downloads/list。
在*nix环境中,Go默认会被安装到/usr/local/go目录中。安装包在安装完成后会自动添加执行 文件目录到系统路径中。
安装完成后,请重新启动命令行程序,然后运行以下命令以验证Go是否已经正确安装: $ go version
    go version go1
如果该命令能够正常运行并输出相应的信息,说明Go编译环境已经正确安装完毕。如果提 示找不到go命令,可以通过手动添加/usr/local/go/bin到PATH环境变量来解决。
