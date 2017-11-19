---
layout: post
title: mac下.bashrc不起作用
date: 2015-08-19 01:12
author: admin
comments: true
categories: []
---


～/.bashrc里面的一些设置，比如alias命令的设置“不起作用”，新开一个终端都要source一下才起作用。
unix下当shell是login shell，.bash_profile才会加载，而bashrc正好相反。
真正的区别是在linux下，当用户登录到一个图形界面，然后打开一个终端terminal，那些shell是non-login shell。
然而，在OS X登录的时候，并没有运行着一个shell，所以，在运行Terminal.app的时候，其实那是一个login shell。
后来新建了 .bash_profile加载一次.bashrc就ok啦
if [ "${BASH-no}" != "no" ]; then
    [ -r ~/.bashrc ] && . ~/.bashrc
fi


