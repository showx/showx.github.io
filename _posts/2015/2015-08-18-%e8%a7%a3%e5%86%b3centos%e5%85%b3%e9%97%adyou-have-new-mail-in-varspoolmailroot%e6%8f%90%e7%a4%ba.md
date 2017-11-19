---
layout: post
title: 解决Centos关闭You have new mail in /var/spool/mail/root提示
date: 2015-08-18 17:23
author: admin
comments: true
categories: []
---
昨天搬到阿里云了。
装的系统是Centos 6.3的加固版
今天查看内存的时候 出现一天奇怪的提示
You have new mail in /var/spool/mail/root
有的时候每敲一下回车，就出来You have new mail in /var/spool/mail/root的提示，究竟是为什么呢？
Linux 系统经常会自动发出一些邮件来提醒用户系统中出了哪些问题（收件箱位置：/var/mail/）。可是这些邮件都是发送给 root 用户的。出于系统安全考虑，通常不建议大家直接使用 root 帐户进行日常操作。所以要想点办法来让系统把发给 root 用户的邮件也给自己指定的外部邮箱发一份，或者是直接关闭此项服务。
 
1、关闭sendmail服务，这里介绍一种不用关闭sendmail服务的方法
 代码如下	复制代码

echo “unset MAILCHECK” >> /etc/profilesource /etc/profile

关闭sendmail的功能：
 代码如下	复制代码

chmod 0 /usr/sbin/sendmailmv /usr/sbin/sendmail /usr/sbin/sendmail.bakln -s /var/qmail/bin/sendmail /usr/sbin/sendmail

清空 /var/spool/mail/root日志

 
 代码如下	复制代码
cat /dev/null > /var/spool/mail/rootcat /dev/null>;/var/spool/mail/root

或者转发到自己的邮箱，下面介绍下怎么转发到自己的邮箱（此方法未经本人亲自验证 来源于网络，有喜欢折腾的请自己研究，成功了 可以跟帖分享经验）
2、root邮件转发到自己的邮箱
方法一：
修改此文件
 代码如下	复制代码
/etc/log.d/logwatch.conf
添加MailTo = root,xxx@xxx.com
方法二
 代码如下	复制代码
/etc/aliases
添加root: xxx@xxx.com
注意：好像如果设置成和主机同域的，好像邮件就发不成，比如本机邮件就是moper.me，那么发这个就没法发，相应的发其他邮箱就可以成功。
关于“/etc/aliases”：
当sendmail收到一个要送给xxx的信时，它会依据/etc/aliases文件中的内容送给另一个使用者。这个功能可以创造一个只有在信件 系统内才有效的使用者。例如mailing list就会用到这个功能，在 mailing list 中，我们可能会创造一个叫 redlinux@link.ece.uci.edu的 mailinglist，但实际上并没有一个叫redlinux的使用者。实际 aliases档的内容是将送给这个使用者的信都收给mailing list处理程式负责分送的工作。
/etc/aliases是一个文本文档，而sendmail需要一个二进位格式的 /etc/aliases.db。newaliases的功能传是将/etc/aliases转换成一个sendmail所能了解的db文件：
 代码如下	复制代码
[root@centos ~]# newaliases
除root外的其它用的邮件可以通过在用户/home/下建立一个.forward文件实现转发:
 代码如下	复制代码
//somebody
other1
other2
文件权限设为600,作用一样,但.forward可以由用户自行维护,而aliases则只有治理员才能修改。
设定~/.forward档案加入转寄目的即可
网上很多教程是你抄我，我抄你，根本就没有验证过的，比如有种方法是修改”/usr/share/logwatch/default.conf/logwatch.conf“配置文件，在centos6中根本就没有这个文件，至于以前的版本有没有就不知道了。
还有很多教程，只有“echo "unset MAILCHECK" >> /etc/profile”，而没有“source /etc/profile”，这也是不对的。
