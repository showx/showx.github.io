---
layout: post
title: ubuntu13.04设置root权限详解
date: 2014-11-12 15:10
author: admin
comments: true
categories: []
---
很多朋友安装升级Ubuntu 13.04之后不知道ubuntu 13.04 root权限设置的具体方法，今天这篇文章就将为大家详细介绍设置root权限的步骤，新手朋友可以来看一看哦~

Ubunto 13.04默认是不允许root登录的，在登录窗口只能看到普通用户和客人登录，所以只能先以普通身份登陆Ubuntu，然后我们再做一些修改，使Ubunto能使用root权限，步骤如下：

1. 以普通用户登录;

2. 按Ctrl+Atl+T打开终端窗口;

3. 在终端窗口里面输入: sudo -s，然后输入普通用户登录的密码，回车即可进入root用户权限模式;

4. 修改系统配置文件，vi /etc/lightdm/lightdm.conf.增加


复制代码代码如下:

greeter-show-manual-login=true
allow-guest=false
修改完的整个配置文件是


复制代码代码如下:

[SeatDefaults]</p> <p>greeter-session=unity-greeter</p> <p>user-session=ubuntu</p> <p>greeter-show-manual-login=true #手工输入登陆系统的用户名和密码</p> <p>allow-guest=false #不允许guest登录
5. 然后我们启动root帐号：


复制代码代码如下:

sudo passwd root
根据提示输入roott帐号密码。

最后重启ubuntu，登录窗口会有“登录”选项，这时候我们就可以通过root登录了。
