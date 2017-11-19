---
layout: post
title: CentOS设置程序开机自启动的方法
date: 2015-02-03 15:01
author: admin
comments: true
categories: []
---
在<a title="CentOS" href="http://www.centos.bz/">CentOS</a>系统下，主要有两种方法设置自己安装的程序开机启动。
<strong>1、把启动程序的命令添加到/etc/rc.d/rc.local文件中，比如下面的是设置开机启动httpd。</strong>
<div class="hl-surround">
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<pre>#!/bin/sh
#
# This script will be executed *after* all the other init scripts.
# You can put your own initialization stuff in here if you don't
# want to do the full Sys V style init stuff.
 
touch /var/lock/subsys/local
/usr/local/apache/bin/apachectl start</pre>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
</div>
<strong>2、把写好的启动脚本添加到目录/etc/rc.d/init.d/，然后使用命令<a title="7个Linux chkconfig命令实例" href="http://www.centos.bz/2011/07/7-linux-chkconfig-command-examples/">chkconfig</a>设置开机启动。</strong>

chkconfig 功能说明：检查，设置系统的各种服务。

语法：chkconfig [--add][--del][--list][系统服务] 或 chkconfig [--level &lt;等级代号&gt;][系统服务][on/off/reset]

--add 添加服务

--del 删除服务

--list 查看各服务启动状态

比如我们设置自启动mysql：
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<pre> 1 #将mysql启动脚本放入所有脚本运行目录/etc/rc.d/init.d中
 2 cp /lamp/mysql-5.0.41/support-files/mysql.server /etc/rc.d/init.d/mysqld
 3 
 4 #改变权限
 5 chown root.root /etc/rc.d/init.d/mysqld
 6 
 7 #所有用户都可以执行，单只有root可以修改
 8 chmod 755 /etc/rc.d/init.d/mysqld
 9 
10 #将mysqld 放入linux启动管理体系中
11 chkconfig --add mysqld
12 
13 #查看全部服务在各运行级状态
14 chkconfig --list mysqld
15 
16 #只要运行级别3启动，其他都关闭
17 chkconfig --levels 245 mysqld off</pre>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
例如：我们把httpd的脚本写好后放进/etc/rc.d/init.d/目录，使用
<div class="hl-surround">
<div class="cnblogs_code">
<pre>chkconfig --add httpd
chkconfig httpd on</pre>
</div>
命令即设置好了开机启动。

&nbsp;

<strong>3、把启动程序的命令添加到/etc/rc.d/rc.sysinit 文件中</strong>

&nbsp;

脚本/etc/rc.d/rc.sysinit，完成系统服务程序启动，如系统环境变量设置、设置系统时钟、加载字体、检查加载文件系统、生成系统启动信息日志文件等

&nbsp;

比如我们设置自启动apache：
<div class="cnblogs_code">
<pre>echo "/usr/local/apache2/bin/apachectl start" &gt;&gt; /etc/rc.d/rc.sysinit</pre>
</div>
</div>
