---
layout: post
title: Go安装的设置问题：GOROOT，GOPATH
date: 2014-08-24 22:58
author: admin
comments: true
categories: []
---
Mac下使用Google官方的Go语言安装包：https://code.google.com/p/go/downloads/list 安装的Go，会自动把 /usr/local/go/bin 目录加入PATH中。这样我们直接在控制台就可以执行go语言的一些命令。
http://golang.org/cmd/go/#hdr-GOPATH_environment_variable
http://www.cnblogs.com/ghj1976/archive/2013/01/16/2863142.html 
NewImage
Go的二进制编译包假设你把Go安装在 /usr/local/go (或者Window是 c:\Go)目录下。当然你也可以安装在其他目录下，不过这时候你就需要设置GOROOT环境变量了。
http://golang.org/doc/install#install
例如，你如果安装Go在你的Home目录下，你应该$HOME/.profile文件增加下面设置。
 
export GOROOT=$HOME/go export PATH=$PATH:$GOROOT/bin
Window下则是：
 
Under Windows, you may set environment variables through the "Environment Variables" button on the "Advanced" tab of the "System" control panel. Some versions of Windows provide this control panel through the "Advanced System Settings" option inside the "System" control panel.

比如我的Mac本，其实我没有设置GOROOT，但是通过 go env 可以得到GOROOT的目录是：/usr/local/go 
我猜测这应该是没有设置时的默认设置。如果有设置，会覆盖。
NewImage
 
GOPATH
GOPATH的作用是告诉Go 命令和其他相关工具，在那里去找到安装在你系统上的Go包。
GOPATH是一个路径的列表，一个典型的GOPATH设置如下，类似PATH的设置，Win下用分号分割：
GOPATH=/home/user/ext:/home/user/mygo 
每一个列表中的路径是一个工作区的位置。每个工作区都有源文件、相关包的对象、执行文件。
http://golang.org/doc/code.html
 
下面是一个建立工作区的步骤：
创建 $HOME/mygo 目录和作为源代码的 src 目录。
$ mkdir -p $HOME/mygo/src # create a place to put source code 
下一步就是设置 GOPATH，另外你应该把 这个目录下的bin目录放在 PATH 环境变量，这样你就可以直接在命令行执行而不用给出完整目录。
export GOPATH=$HOME/mygo export PATH=$PATH:$HOME/mygo/bin
 
GOPATH 必须设置编译和安装包，即使用标准的Go目录树，类似如下：
GOPATH=/home/user/gocode/home/user/gocode/ src/ foo/ bar/ (go code in package bar) x.go quux/ (go code in package main) y.go bin/ quux (installed command) pkg/ linux_amd64/ foo/ bar.a (installed package object)

http://golang.org/cmd/go/#hdr-GOPATH_environment_variable
