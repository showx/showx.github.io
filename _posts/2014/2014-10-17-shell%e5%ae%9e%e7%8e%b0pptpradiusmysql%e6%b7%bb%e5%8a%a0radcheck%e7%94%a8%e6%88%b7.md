---
layout: post
title: Shell实现PPTP+radius+mysql添加radcheck用户
date: 2014-10-17 20:52
author: admin
comments: true
categories: []
---
前言,Shell实现PPTP+radius+mysql添加radcheck用户，PPTP+radius+mysql搭建VPN不多讲了,在网上搜一下，有很多文章教你做，但要做到自动化的添加radcheck的PPP认证用户，还得用sehll来实现，会LINUX的朋友过来围观,初学者,最好的一例分享了;

干货,都是干货,请看下图源码:



我再把源码贴出来,先来看一看运行效果吧:

[root@dbserver mycmd]# ./addraduser

Enter the passwd of root is:username is:good

password is:good

Add the pptp_user is ok

[root@dbserver mycmd]#

可以看到,整个过程很容易,MYSQL密码手动输入,然后加入帐号,与帐号密码,便可以实现Shell添加radcheck用户了,比起登陆加入要来轻松,并且可以远程添加,只要把LOCALHOST改一下IP地址便可以实现;是不是很容易呢!当然,举一返三,通过例,我们可以做到通过shell实现MYSQL数据库添加的表数据;这便是如何shell操作mysql数据库的实例拉;

#!/bin/bash

HOSTNAME="localhost"

PORT="3306"

USERNAME="root"

read -s -p "Enter the passwd of root is:" p

PASSWD="$p"

DBNAME="radius"

TABLENAME="radcheck"

read -p "username is:" user

read -p "password is:" pwd

insert_sql="insert into $DBNAME.${TABLENAME}(Username,Attribute,op,Value) values                                                           ('$user','password','==','$pwd')"

mysql -h${HOSTNAME} -P${PORT} -u${USERNAME} -p${PASSWD} ${DBNAME} -e "${insert_sql}"

if [ $? == 0 ];then

         echo "Add the pptp_user is ok"

else

         echo "inserting into mysql is not ok, why?"

fi

上面是源码，十几行的代码；

总结一下，shell实现MYSQL数据操作，重点在于这个命令：

 mysql -h${HOSTNAME} -P${PORT} -u${USERNAME} -p${PASSWD} ${DBNAME} -e "${insert_sql}

这个操作命令，看来很容易不是吗?分享结束，谢谢；
