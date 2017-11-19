---
layout: post
title: MySQL远程连接ERROR 2003 (HY000):Can't connect to MySQL server on'XXXXX'的问题
date: 2014-12-04 17:49
author: admin
comments: true
categories: []
---

问题描述：
 
从一台linux远程连接另一台linux上的MySQL, 出现ERROR 2003 (HY000): Can't connect to MySQL server on 'xxx.xxx.xxx.85'(111)错误。
 
[mysql@vvmvcs0 ~]$ mysql -hxxx.xxx.xxx.85 -uroot -p
Enter password:  www.2cto.com  
ERROR 2003 (HY000): Can't connect to MySQL server on 'xxx.xxx.xxx.85' (111)
[mysql@vvmvcs0 ~]$ perror 111
OS error code 111: Connection refused
查看errorCode
 
[mysql@vvmvcs0 ~]$ perror 111 
OS error code 111: Connection refused
 
问题分析：
1，可能网络连接问，远程ping xxx.xxx.xxx.85 ，能ping通，排除此情况
 
[mysql@vvmvcs0 ~]$ ping xxx.xxx.xxx.85 
PING xxx.xxx.xxx.85 (xxx.xxx.xxx.85) 56(84) bytes of data.
64 bytes from xxx.xxx.xxx.85: icmp_seq=1 ttl=63 time=0.230 ms
 
2，排查可能由于85上my.cnf里配置了skip_networking或者bind_address，只允许本地socket连接
2.1 在[mysqld]下设置skip_networking,
知识说明： 这使用MySQL只能通过本机Socket连接(socket连接也是本地连接的默认方式），放弃对TCP/IP的监听  www.2cto.com  
当然也不让本地java程序连接MySQL(Connector/J只能通过TCP/IP来连接）。
2.2 可能使用了bind_address=127.0.0.1(当然也可以是其他ip)
 
[mysqld] 
bind_address=127.0.0.1
知识说明：这种情况可以TCP/IP连接
通过查看了my.cnf文件，以上两个都是没设置的，排除掉这两种情况
 
3，排查DNS解析问题,检查是否设置了： skip_name_resolve。 这个情况肯定不可能，因为我用的是ip,不是主机名。
 
[mysqld]
skip_name_resolve
知识说明：这个参数加上后，不支持主机名的连接方式。
 
4, 排查用户和密码问题， 其实用户和密码的错误，不会出现111的，所以排除用户密码问题
ERROR 1045 (28000): Access denied for user 'root'@'XXXX' (using password: YES)
 
5，排查--port问题，有可能85的MySQL port不是默认3306， 这样我远程连接时，没有指定--port，用的是3306, 而85上没有对3306进行监听。
ps -ef | grep mysqld
果然是： 85上的MySQL使用的是3308 port.
最终连接方式：加上--port=3308
 
[mysql@vvmvcs0 ~]$ mysql -hxxx.xxx.xxx.85 -uroot -p --port=3308 
Enter password: 
Welcome to the MySQL monitor. Commands end with ; or \g.
 
为什么出现这么低级的错误呢？
因为我一直在用85上的MySQL, 而且每次都是直接用mysql -uroot就连接上了，没有指定--port，这样我就一直以为这MySQL的port一直是默认的3306的。
 
其实根本原因是：
 
1. MySQL本地连接，如果不指mysql --protocol=tcp, 连接默认是socket方式连接的。这点大家都知道。  www.2cto.com  
2, MySQL socket连接是根据sokect文件来的，与--port不相关的，如果是一机多实例，则用-S(或者--socket=name ）来指定连接哪个实例。
就是这个socket连接对--port无识别效果，导致排查这个问题这么久。
见下面： 其实85上只有一个port为3308的MySQL实例，但是用3306仍然是连接上此实例，说明socket连接方式忽略--port参数。
 
-bash-3.2$ mysql -uroot --port=3308
 Welcome to the MySQL monitor. Commands end with ; or \g.
 
 mysql -uroot --port=3306 
Welcome to the MySQL monitor. Commands end with ; or \g.
再次说明基础细节很重要啊。
