---
layout: post
title: 从【MySQL server has gone away】说起
date: 2015-02-03 15:20
author: admin
comments: true
categories: []
---
<strong>本文目的</strong>

这几天开发了一个PHP CLI程序，用于后台定时调度执行一些任务。此脚本采用了PHP的多进程（pcntl_fork），共享内存和信号量进行IPC和同步。目的是将串行的任务并行执行，缩短执行时间。可是在工作子进程中，访问myql时一直报错，通过mysql_error返回的信息却是冷冷的一句话“MySQL server has gone away”。简单说一句自己挂掉了就完事，太不负责任了。经过仔细搜索，终于发现问题的原因，在此做个分享，也作为备忘。

<strong>什么导致“MySQL server has gone away”</strong>

据<a href="http://dev.mysql.com/doc/refman/5.0/en/gone-away.html">官方文档</a>描述，主要有以下一些原因导致此异常出现（我粗略的翻译一下，以原文为准）：

1. mysqld线程被杀死.

2. 使用了关闭的链接资源

3. 无权限

4. TCP/IP超时

5. mysql服务器端超时

6. windows兼容问题

7. sql过长

8. 请求包过长

9. DNS解析问题

10. 多进程并发使用同一个链接

11. mysql的未知bug

<em>吐槽：以上情况虽然列举得很详细，但是为什么不能在第一时间通过mysql_error</em><em>返回呢？</em>

<strong>问题的原因</strong>

由于我的使用场景是在多进程环境下，所以很快将问题锁定在了mysql链接资源上，果然很快找到答案：<strong>mysql_connect</strong><strong>函数的第四个参数$new_link</strong><strong>没有设置为true.</strong>

首先看看mysql_connect函数签名：

<em>resource <strong>mysql_connect</strong> ([ string </em><em>$server</em><em> = ini_get("mysql.default_host") [, string </em><em>$username</em><em> = ini_get("mysql.default_user") [, string </em><em>$password</em><em> = ini_get("mysql.default_password") [, bool </em><em>$new_link</em><em> = false [, int </em><em>$client_flags</em><em> = 0 ]]]]] )</em>

默认情况，$new_link设置为false，也就是会重用现有的链接资源（如果有）。设想一下，如果多个进程同时调用mysql_connect，并且没有设置$new_link为true，那么这些进程就会同时使用同一个链接资源，也就是上面第10个原因，导致mysql报告“mysql server has gone away”的异常。

所以，将$new_link设置为true后，此问题就解决了。

<strong>问题的本质</strong>

mysql链接资源，底层其实是封装了socket和相关的数据，如果多个进程用一个链接，也就是同一个socket和服务器交互，结果可想而知，服务器被混淆了，所以只能吐出一句冷冷的“MySQL server has gone away”了事。

但是，我还有一个疑问，php在apache环境下访问mysql时，并没有显示设置$new_link为True，但是从没有出现上述这个现象，难道是apache处理并发时，采用的单进程而不是多进程？

<strong>总结</strong>

初次涉及多进程相关开发，往往遇到许多莫名奇妙的问题，有些现象时有时无。这时候，一定冷静分析问题的现象，不要随意将责任推究给其他系统（比如linux,mysql等），而是要在自己的程序上找原因。

在刚遇到此问题时，由于一时没有找到问题的原因，我还怀疑过是mysql的性能太低，不能承受我的程序的并发。其实这是一种出于本能的自我安慰。mysql的并发性能在业绩是有口皆碑的，怎么会在我的几个进程面前倒下。经过一番网上搜素和思考，终于找到问题原因，现在可以松一口气了。

值得高兴的是，程序改成并发后，性能有了质的飞跃，执行时间缩小到原来的十分之一，系统使用率高达90%多，改进前不到10%。
