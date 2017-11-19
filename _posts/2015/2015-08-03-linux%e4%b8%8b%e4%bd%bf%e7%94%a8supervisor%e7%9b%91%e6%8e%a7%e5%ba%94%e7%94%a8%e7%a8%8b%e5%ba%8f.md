---
layout: post
title: linux下使用supervisor监控应用程序
date: 2015-08-03 11:14
author: admin
comments: true
categories: []
---
1 应用场景
应用程序需要24小时不间断运行。这时可使用supervisor监控应用程序的进程。当发生应用程序内部错误退出、进程被杀死等情况时，自动重启应用程序。

2 supervisor
supervisor由python写成， 简单好用。官方网站 http://supervisord.org，上面有详细的指南文档。

3 安装supervisor

1）安装 setuptools
$sudo apt-get install python-setuptools

2)  使用easy_install安装 supervisor
$sudo easy_install supervisor

安装完成后出现：
/usr/bin/supervisord
 --supervisor服务守护进程
/usr/bin/supervisorctl--supervisor服务控制程序

4 配置文件
以下命令可以将配置文件示例打印到控制台：
$ echo_supervisord_conf
若要生成配置文件，使用命令：
$ sudo echo_supervisord_conf > /etc/supervisord.conf
或者在当前目录生成：
$ echo_supervisord_conf > supervisord.conf

--------------------------------

Notice: 如何停止子进程

场景：如果supervisord.conf中配置的command是执行一个bash，而bash里执行java，那么当使用supervisorctl stop [programname]停止程序时，只有上层进程被停止，而java进程没有被停止。

解决办法：

在配置文件中设置：

stopasgroup=true               
killasgroup=true 

---------------------------------

5 配置要运行的程序
在上面生成的配置文件后，追加：

[plain] view plaincopy
[program:helloworld]  
command=./helloworld              ;执行命令  
process_name=%(program_name)s  
autostart=true                   ; 程序是否随supervisor启动而启动  
autorestart=true                 ;程序停止时，是否自动重启  
startsecs=10  

6 启动supervisor
$ supervisord -c supervisord.conf


7 使用supervisorctl管理程序
1）开启/停止某个程序
# supervisorctl [start | stop] [program名称]      //在supervisord.conf中定义的

2）查看进程状态
$supervisorctl status

8 通过web方式查看状态
配置文件中配置：

[inet_http_server]         ; inet (TCP) server disabled by default
port=127.0.0.1:9001        ; (ip_address:port specifier, *:port for all iface)

那么可以从浏览器通过访问 localhost:9001来查看进程状态


9 添加supervisord为Linux系统服务，开机自动启动


1）将supervisord.conf拷贝到 /etc 目录下

2）启动脚本 supervisord.sh  （centos/redhat）

[plain] view plaincopy
#!/bin/sh  
#  
# /etc/rc.d/init.d/supervisord  
#  
# Supervisor is a client/server system that  
# allows its users to monitor and control a  
# number of processes on UNIX-like operating  
# systems.  
#  
# chkconfig: - 64 36  
# description: Supervisor Server  
# processname: supervisord  
  
# Source init functions  
. /etc/rc.d/init.d/functions  
  
prog="supervisord"  
  
prog_bin="/usr/local/bin/supervisord"  
PIDFILE="/tmp/supervisord.pid"  
  
start()  
{  
        echo -n $"Starting $prog: "  
  
# Source init functions  
. /etc/rc.d/init.d/functions  
  
prog="supervisord"  
  
prog_bin="/usr/local/bin/supervisord"  
PIDFILE="/tmp/supervisord.pid"start()  
{  
        echo -n $"Starting $prog: "  
        daemon $prog_bin --pidfile $PIDFILE  
        [ -f $PIDFILE ] && success $"$prog startup" || failure $"$prog startup"  
        echo  
}  
  
stop()  
{  
        echo -n $"Shutting down $prog: "  
        [ -f $PIDFILE ] && killproc $prog || success $"$prog shutdown"  
        echo  
}  
  
case "$1" in  
  start)  
    start  
  ;;  
  stop)  
    stop  
  ;;  
  status)  
        status $prog  
  ;;  
  restart)  
    stop  
    start  
  ;;  
  *)  
    echo "Usage: $0 {start|stop|restart|status}"  
  ;;  
  
esac  


3）添加为系统服务

# mv supervisord.sh  /etc/init.d/supervisord
# chkconfig --add  supervisord
# chkconfig --level 345 supervisord on

4） 开启/停止服务

# service supervisord [start | stop]


参考材料：

http://supervisord.org/running.html
http://serverfault.com/questions/96499/how-to-automatically-start-supervisord-on-linux-ubuntu


＝＝＝＝＝＝＝＝＝

https://scottrobertson.me/post/supervisor-php-nginx/
