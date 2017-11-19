---
layout: post
title: linux下查看监听端口对应的进程
date: 2014-08-15 15:07
author: admin
comments: true
categories: []
---
方法一 
1.通过lsof命令查看PID 
ipv4 
[root@test proc]# lsof -Pnl +M -i4          
COMMAND     PID     USER   FD   TYPE  DEVICE SIZE NODE NAME 
java       1419     1401   10u  IPv4 6793357       TCP *:8453 (LISTEN) 
AutonomyD  6147     1401    6u  IPv4 7597365       TCP *:20003 (LISTEN) 
AutonomyD  6147     1401   14u  IPv4 7597369       TCP *:20000 (LISTEN) 
也可以使用: 
[root@test proc]# lsof -Pnl +M -i4|grep 8453 
java       1419     1401   10u  IPv4 6793357       TCP *:8453 (LISTEN) 

ipv6 
[root@test proc]# lsof -Pnl +M -i6 
COMMAND     PID     USER   FD   TYPE  DEVICE SIZE NODE NAME 
java       1419     1401  286u  IPv6 7616547       TCP 192.168.1.29:55829->192.168.1.17:7001 (CLOSE_WAIT)
java       1419     1401  290u  IPv6 6987470       TCP 192.168.1.29:33836->192.168.1.154:1521 (ESTABLISHED) 
java       1419     1401  297u  IPv6 6793642       UDP *:1133 
java       1419     1401  304u  IPv6 6987472       TCP 192.168.1.29:33838->192.168.1.154:1521 (ESTABLISHED) 
java       1419     1401  306u  IPv6 6987479       TCP 192.168.1.29:33839->192.168.1.154:1521 (ESTABLISHED) 
java       1419     1401  307u  IPv6 7006208       TCP 192.168.1.29:60340->192.168.1.154:1521 (ESTABLISHED) 
也可以使用: 
[root@test proc]# lsof -Pnl +M -i6|grep 5001 
java      12886        0  530u  IPv6 6988341       TCP *:5001 (LISTEN) 
2.通过ps命令查看进程情况 
[root@test proc]# ps -ef|grep 12886 
root     12886 12851  0 Dec09 ?        00:00:43 /home/bjca/bea/jdk160_05/bin/java -client -Xms256m -Xmx512m -XX:CompileThreshold=8000 -XX:PermSize=48m -XX:MaxPermSize=128m -Xverify:none -da -Dplatform.home=/home/bjca/bea/wlserver_10.3 -Dwls.home=/home/bjca/bea/wlserver_10.3/server -Dweblogic.home=/home/bjca/bea/wlserver_10.3/server -Dweblogic.management.discover=true -Dwlw.iterativeDev= -Dwlw.testConsole= -Dwlw.logErrorsToConsole= -Dweblogic.ext.dirs=/home/bjca/bea/patch_wlw1030/profiles/default/sysext_manifest_classpath:/home/bjca/bea/patch_wls1030/profiles/default/sysext_manifest_classpath:/home/bjca/bea/patch_cie660/profiles/default/sysext_manifest_classpath -Dweblogic.Name=AdminServer -Djava.security.policy=/home/bjca/bea/wlserver_10.3/server/lib/weblogic.policy weblogic.Server 

3.lsof命令参数解释 
　　1) -P :这个选项约束着网络文件的端口号到端口名称的转换。约束转换可以使lsof运行得更快一些。在端口名称的查找不能奏效时，这是很有用的。 
　　2) -n : 这个选项约束着网络文件的端口号到主机名称的转换。约束转换可以使lsof的运行更快一些。在主机名称的查找不能奏效时，它非常有用。 
　　3) -l :这个选项约束着用户ID号到登录名的转换。在登录名的查找不正确或很慢时，这个选项就很有用。 
　　4) +M :此选项支持本地TCP和UDP端口映射程序的注册报告。 
　　5) -i4 :仅列示IPv4协议下的端口。 
　　6) -i6 : 仅列示IPv6协议下的端口。 
方法二 
1.使用netstat查看进程PID 
[root@test ~]#  netstat -anp|grep 5001 
tcp        0      0 :::5001                     :::*                        LISTEN      12886/java          
2.使用ps查看进程情况 
[root@test 12886]# ps -ef|grep 12886 
root     12886 12851  0 Dec09 ?        00:01:14 /home/bjca/bea/jdk160_05/bin/java -client -Xms256m -Xmx512m -XX:CompileThreshold=8000 -XX:PermSize=48m -XX:MaxPermSize=128m -Xverify:none -da -Dplatform.home=/home/bjca/bea/wlserver_10.3 -Dwls.home=/home/bjca/bea/wlserver_10.3/server -Dweblogic.home=/home/bjca/bea/wlserver_10.3/server -Dweblogic.management.discover=true -Dwlw.iterativeDev= -Dwlw.testConsole= -Dwlw.logErrorsToConsole= -Dweblogic.ext.dirs=/home/bjca/bea/patch_wlw1030/profiles/default/sysext_manifest_classpath:/home/bjca/bea/patch_wls1030/profiles/default/sysext_manifest_classpath:/home/bjca/bea/patch_cie660/profiles/default/sysext_manifest_classpath -Dweblogic.Name=AdminServer -Djava.security.policy=/home/bjca/bea/wlserver_10.3/server/lib/weblogic.policy weblogic.Server 
root     27592 27546  0 09:11 pts/2    00:00:00 grep 12886 
