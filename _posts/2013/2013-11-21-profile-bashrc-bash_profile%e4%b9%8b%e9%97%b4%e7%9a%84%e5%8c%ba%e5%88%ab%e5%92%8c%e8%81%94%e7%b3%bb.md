---
layout: post
title: profile bashrc bash_profile之间的区别和联系
date: 2013-11-21 17:09
author: admin
comments: true
categories: []
---

执行顺序为：/etc/profile -> (~/.bash_profile | ~/.bash_login | ~/.profile) -> ~/.bashrc -> /etc/bashrc -> ~/.bash_logout
 
关于各个文件的作用域，在网上找到了以下说明：
 
（1）/etc/profile： 此文件为系统的每个用户设置环境信息，当用户第一次登录时，该文件被执行，并从/etc/profile.d目录的配置文件中搜集shell的设置。
 
（2）/etc/bashrc：为每一个运行bash shell的用户执行此文件。当bash shell被打开时，该文件被读取。
 
（3）~/.bash_profile：每个用户都可使用该文件输入专用于自己使用的shell信息，当用户登录时，该文件仅仅执行一次!默认情况下，他设置一些环境变量，执行用户的.bashrc文件。
 
（4）~/.bashrc：该文件包含专用于你的bash shell的bash信息，当登录时以及每次打开新的shell时，该该文件被读取。
 
（5）~/.bash_logout：当每次退出系统(退出bash shell)时，执行该文件；另外，/etc/profile中设定的变量(全局)的可以作用于任何用户，而~/.bashrc等中设定的变量(局部)只能继承 /etc/profile中的变量，他们是"父子"关系。
 
~/.bash_profile是交互式、login 方式进入 bash 运行的
 
~/.bashrc 是交互式 non-login 方式进入 bash 运行的通常二者设置大致相同，所以通常前者会调用后者。
 
 
 
/etc/profile和/etc/environment等各种环境变量设置文件的用处
 
 
先将export LANG=zh_CN加入/etc/profile ，退出系统重新登录，登录提示显示英文。
 
 
将/etc/profile 中的export LANG=zh_CN删除，将LNAG=zh_CN加入/etc/environment，退出系统重新登录，登录提示显示中文。
 
 
用户环境建立的过程中总是先执行/etc/profile然后在读取/etc/environment。为什么会有如上所叙的不同呢？
 
应该是先执行/etc/environment，后执行/etc/profile。
 
/etc/environment是设置整个系统的环境，而/etc/profile是设置所有用户的环境，前者与登录用户无关，后者与登录用户有关。
 
系统应用程序的执行与用户环境可以是无关的，但与系统环境是相关的，所以当你登录时，你看到的提示信息，象日期、时间信息的显示格式与系统环境的LANG是相关的，缺省LANG=en_US，如果系统环境LANG=zh_CN，则提示信息是中文的，否则是英文的。
 
 
对于用户的SHELL初始化而言是先执行/etc/profile,再读取文件/etc/environment.对整个系统而言是先执行/etc/environment。这样理解正确吗？
 
/etc/enviroment --> /etc/profile --> $HOME/.profile -->$HOME/.env (如果存在)
 
/etc/profile 是所有用户的环境变量
 
 
/etc/enviroment是系统的环境变量
 
 
登陆系统时shell读取的顺序应该是
/etc/profile ->/etc/enviroment -->$HOME/.profile -->$HOME/.env
原因应该是jtw所说的用户环境和系统环境的区别了
 
 
当在Linux、Unix和Mac OS X下工作时，我总是我总是忘记要修改哪个bash配置文件来设置shell的PATH和其他的环境变量。是应该修改在home目录下的.bash_profile还是.bashrc文件？

你可以修改任意一个文件，当其中文件不存在的时候你也可以创建它们。但是他们为什么是两个文件？它们的区别是什么？

参考bash 手册，执行.bash_profile 是为了登录shell的，但.bashrc是一个交互式的非登录shells。

那什么是登录或非登录shell呢？

当你或者在机器前，或者在远程通过ssh，通过控制台进行登录（输入用户名和密码）：在初始化命令行提示符的时候会执行.bash_profile 来配置你的shell环境。但是如果你已经登录到机器，你在Gnome或者是KDE也开了一个新的终端窗口（xterm），这时，.bashrc会在窗口命令行提示符出现前被执行。当你在终端敲入/bin/bash时.bashrc也会在这个新的bash实例启动的时候执行。

那为什么有两个不同的文件呢？

比方说，你想字在每次登录时打印一些关于你机器的很长的诊断信息，比如平均负载，内存使用情况，当前用户，等等。你只希望在登录的时候看到它们，所以你只需要把这些放在.bash_profile中。如果你放在.bashrc中，你会在每次打开一个新的终端窗口时看见这些信息。Mac OS X除外。

Mac OS X的终端窗口是个例外。每个终端窗口在打开的时候都会执行登录shell即.bash_profile代替了.bashrc。其他的GUI终端仿真器到做的相同，但大多数情况下不这样做。

建议

大多数的时候你不想维护两个独立的配置文件，一个登录的一个非登录的shell。当你设置PATH时，你想在两个文件都适用。可以在.bash_profile中调用.bashrc，然后将PATH和其他通用的设置放到.bashrc中。

要做到这几点，添加以下几行到.bash_profile中：
if [ -f ~/.bashrc ]; then
   source ~/.bashrc
fi
现在，当你从控制台登录机器的时候，.bashrc就会被执行。
