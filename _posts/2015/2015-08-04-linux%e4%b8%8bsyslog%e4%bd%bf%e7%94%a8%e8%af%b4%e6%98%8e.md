---
layout: post
title: linux下syslog使用说明
date: 2015-08-04 14:44
author: admin
comments: true
categories: []
---

syslog 系统日志应用
 1) 概述
      syslog是Linux系统默认的日志守护进程。默认的syslog配置文件是/etc/syslog.conf文件。程序，守护进程和内核提供了访问系统的日志信息。因此，任何希望生成日志信息的程序都可以向 syslog 接口呼叫生成该信息。
      几乎所有的网络设备都可以通过syslog协议，将日志信息以用户数据报协议(UDP)方式传送到远端服务器，远端接收日志服务器必须通过syslogd监听UDP 端口514，并根据 syslog.conf配置文件中的配置处理本机，接收访问系统的日志信息，把指定的事件写入特定文件中，供后台数据库管理和响应之用。意味着可以让任何事件都登录到一台或多台服务器上，以备后台数据库用off-line(离线) 方法分析远端设备的事件。
      通常，syslog 接受来自系统的各种功能的信息，每个信息都包括重要级。/etc/syslog.conf 文件通知 syslogd 如何根据设备和信息重要级别来报告信息。
 2) etc/syslog.conf
      /etc/syslog.conf 文件使用下面的格式：
      facility.level    action
      facility.level为选择条件本身分为两个字段，之间用一个小数点（.）分隔。action和facility.level之间使用TAB隔开。前一字段是一项服务，后一字段是一个优先级。选择条件其实是对消息类型的一种分类，这种分类便于人们把不同类型的消息发送到不同的地方。在同一个syslog配置行上允许出现一个以上的选择条件，但必须用分号（;）把它们分隔开。action字段所表示的活动具有许多灵活性，特别是，可以使用名称管道的作用是可以使 syslogd 生成后处理信息。
      要素分析：
facility 指定 syslog 功能，主要包括以下这些：
kern     内核信息，首先通过 klogd 传递；
user     用户进程；
mail     邮件；
daemon   后台进程；
authpriv 授权信息；
syslog   系统日志；
lpr      打印信息；
news     新闻组信息；
uucp     由uucp生成的信息
cron     计划和任务信息。
mark     syslog 内部功能用于生成时间戳
local0----local7   与自定义程序使用，例如使用 local5 做为 ssh 功能
*        通配符代表除了 mark 以外的所有功能
      level 指定syslog优先级：
      syslog 级别如下:（按严重程度由高到低的顺序列出了所有可能的优先级。）
emerg 或 panic   该系统不可用（最紧急消息）
alert            需要立即被修改的条件（紧急消息）
crit             阻止某些工具或子系统功能实现的错误条件（重要消息）
err              阻止工具或某些子系统部分功能实现的错误条件（出错消息）
warning          预警信息（警告消息）
notice           具有重要性的普通条件（普通但重要的消息）
info             提供信息的消息（通知性消息）
debug            不包含函数条件或问题的其他信息（调试级-信息量最多）
none             没有重要级，通常用于排错（不记录任何日志消息）
*                所有级别，除了none
action:
1. /var/log/lastlog : 记录每个使用者最近签入系统的时间, 因此当使用者签入时, 就会显示其上次签入的时间, 您应该注意一下这个时间, 若不是您上次签入的时间, 表示您的帐号可能被人盗用了. 此档可用 /usr/bin/lastlog 指令读取.

2. /var/run/utmp : 记录每个使用者签入系统的时间, who, users, finger 等指令会查这个档案.

3. /var/log/wtmp : 记录每个使用者签入及签出的时间, last 这个指令会查这个档案. 这个档案也记录 shutdown 及 reboot 的动作.

4. /var/log/secure : 登录系统的信息

5. /var/log/maillog : 记录 sendmail 及 pop 等相关讯息.

6. /var/log/cron : 记录 crontab 的相关讯息 ，定时器的信息

7. /var/log/dmesg : /bin/dmesg 会将这个档案显示出来, 它是开机时的画面讯息.

8. /var/log/xferlog : 记录那些位址来 ftp 拿取那些档案.

9. /var/log/messages : 系统大部份的讯息皆记录在此, 包括 login, check password , failed login, ftp, su 等.
Application 中定义level：
   0: LOG_EMERG,紧急情况
   1: LOG_ALERT,高优先级故障，例如数据库崩溃
   2: LOG_CRIT,严重错误，例如硬件故障
   3: LOG_ERR,错误
   4: LOG_WARNING,警告
   5: LOG_NOTICE,需要注意的特殊情况
   6: LOG_INFO,一般信息
   7: LOG_DEBUG,调试信息
kernel中定义level（使用printk函数设定level）：
   0: KERN_EMERG, 系統無法使用
   1: KERN_ALERT, 必須立即執行
   2: KERN_CRIT, 緊急狀態
   3: KERN_ERR, 錯誤狀態
   4: KERN_WARNING, 警告狀態
   5: KERN_NOTICE, 正常狀態且十分重要
   6: KERN_INFO, 報告
   7: KERN_DEBUG, debug-level訊息
例子：
//其中*是通配符，代表任何设备；none表示不对任何级别的信息进行记录。
*.info;mail.none;news.none;authpriv.none;cron.none /var/log/messages
//将authpriv的任何级别的信息记录到/var/log/secure文件中，这主要是一些和认、权限使用相关的信息。
authpriv.* /var/log/secure
//将mail设备中的任何级别的信息记录到/var/log/maillog文件中，这主要是和电子邮件相关的信息。
mail.* -/var/log/maillog
//将cron设备中的任何级别的信息记录到/var/log/cron文件中，这主要是和系统中定期执行的任务相关的信息。
cron.* /var/log/cron
//将任何设备的emerg级别的信息发送给所有正在系统上的用户。
*.emerg *
//将uucp和news设备的crit级别的信息记录到/var/log/spooler文件中。
uucp,news.crit /var/log/spooler
//将和系统启动相关的信息记录到/var/log/boot.log文件中。
local7.* /var/log/boot.log
“mail.*”将发送所有的消息，“mail.!info”把info优先级的消息排除在外。
mail.*;mail.!info /var/log/mail  
下面的规则指定Facility为mail，Severity为err以上级别的日志写入/var/log/mail.err文件，而err以下级别的日志则被忽略：
mail.err                        /var/log/mail.err
facility和level可以使用通配符，也可以指定多个，用逗号隔开：
auth,authpriv.*                 /var/log/auth.log
Facility和level的组合可以有多个，用分号隔开，文件前面加一个减号表示日志不立即写入文件，而是在缓冲中积攒到一定的条件再写，这样 可以提高性能，但是当机可能会丢失数据：
*.*;auth,authpriv.none          -/var/log/syslog
可以把syslog消息通过UDP发送到syslog服务器的514端口：
*.err    @192.168.0.1
发生错误时，在控制台打屏：
*.err    /dev/console
 
      linux日志管理：
      内核信息 -> klogd -> syslogd -> /var/log/messages等文件
      其他信息 -> syslogd -> /var/log/messages等文件
      syslog配置文件 -> /etc/syslog.conf
 3) 调用 syslogd 守护程序
      syslog 守护程序是由 /etc/rc.d/init.d/syslog 脚本在运行级2下被调用的，缺省不使用选项。但有两个选项 -r 和 -h 很有用。
      如果将要使用一个日志服务器，必须调用 syslogd -r。缺省情况下 syslog 不接受来自远程系统的信息。当指定 -r 选项，syslogd 将会监听从 514 端口上进来的 UDP 包。
      如果还希望日志服务器能传送日志信息，可以使用 -h 标志。缺省时，syslogd 将忽略使其从一个远程系统传送日志信息到另一个系统的/etc/syslog.conf 输入项。
 4) klogd 守护进程
      klogd 守护进程获得并记录 Linux 内核信息。通常，syslogd 会记录 klogd 传来的所有信息，然而，如果调用带有 -f filename 变量的 klogd 时，klogd 就在 filename 中记录所有信息，而不是传给syslogd。当指定另外一个文件进行日志记录时，klogd 就向该文件中写入所有级别或优先权。Klogd 中没有和 /etc/syslog.conf 类似的配置文件。使用 klogd 而避免使用 syslogd 的好处在于可以查找大量错误。如果有人入侵了内核，使用 klogd 可以修改错误。
 5) 配置一个中央日志服务器 
1.  编辑/etc/sysconfig/syslog文件。
      在“SYSLOGD_OPTIONS”行上加“-r”选项以允许接受外来日志消息。如果因为关于其他机器的DNS记录项不够齐全或其他原因不想让中央日志服务器解析其他机器的FQDN，还可以加上“-x”选项。此外，你或许还想把默认的时间戳标记消息（--MARK--）出现频率改成比较有实际意义的数值，比如240，表示每隔240分钟（每天6次）在日志文件里增加一行时间戳消息。日志文件里的“--MARK--”消息可以让你知道中央日志服务器上的 syslog守护进程没有停工偷懒。按照上面这些解释写出来的配置行应该是如下所示的样子：
    SYSLOGD_OPTIONS="-r-x-m240"
2.  重新启动syslog守护进程。
      修改只有在syslog守护进程重新启动后才会生效。如果你只想重新启动syslog守护进程而不是整个系统，执行以下两条命令之一：
/etc/rc.d/init.d/syslog stop;   /etc/rc.d/init.d/syslog start
/etc/rc.d/init.d/syslog restart 
3.  如果这台机器上运行着iptables防火墙或TCPWrappers，请确保它们允许514号端口上的连接通过。syslog守护进程要用到514号端口。 
4.  为中央日志服务器配置各客户机器
      让客户机把日志消息发往一个中央日志服务器并不困难。编辑客户机上的/etc/syslog.conf文件，在有关配置行的操作动作部分用一个“@”字符指向中央日志服务器，如下所示：
authpriv.*@192.168.1.40
      另一种办法是在DNS里定义一个名为“loghost”的机器，然后对客户机的syslog配置文件做如下修改（这个办法的好处是：当你把中央日志服务器换成另一台机器时，不用再修改每一个客户机上的syslog配置文件）
authpriv.*@loghost
      接下来，重新启动客户机上的syslog守护进程让修改生效。让客户机在往中央日志服务器发送日志消息的同时继续在本地进行日志工作仍有必要，起码在调试客户机的时候不必到中央日志服务器查日志，在中央日志服务器出问题的时候还可以帮助调试。
 6)与系统日志相关的函数：
openlog， syslog， closelog是一套系统日志写入接口。
程序的用法示例代码如下：syslog.c
[c-sharp] view plaincopyprint?
//syslog.c  
#include    
int main(int argc, char **argv)  
{  
    openlog("MyMsgMARK", LOG_CONS | LOG_PID, 0);  
    syslog(LOG_EMERG,  
           "This is a syslog test message generated by program '%s'/n",  
           argv[0]);  
    closelog();  
    return 0;  
}  

编译运行：
[root@localhost liuxltest]# gcc -o syslog syslog.c
[root@localhost liuxltest]# ./syslog
[root@localhost liuxltest]# 
Message from syslogd@ at Tue Feb 24 13:24:34 2009 ...
localhost MyMsgMARK[16467]: This is a syslog test message generated by program './syslog'
同时，你也可以在/var/log/messages中看到信息如下：
Feb 24 13:24:34 localhost MyMsgMARK[16467]: This is a syslog test message generated by program './syslog'

函数说明：
openlog函数原型如下：
void openlog(const char *ident, int option, int facility);
    此函数用来打开一个到系统日志记录程序的连接，打开之后就可以用syslog或vsyslog函数向系统日志里添加信息了。
    参数说明：
    ident：是一个标记，ident所表示的字符串将固定地加在每行日志的前面以标识这个日志，通常就写成当前程序的名称以作标记。
    option：是下列值取与运算的结果：LOG_CONS， LOG_NDELAY， LOG_NOWAIT， LOG_ODELAY，
LOG_PERROR， LOG_PID，各值意义请参考man openlog手册：
       LOG_CONS
              Write directly to system console if there is an error while sending to system logger.
       LOG_NDELAY
              Open the connection immediately (normally, the connection is opened when the first message is logged).
       LOG_PERROR
              (Not in SUSv3.) Print to stderr as well.
       LOG_PID
              Include PID with each message.
    facility：指明记录日志的程序的类型。
closelog函数原型如下：
void  closelog(void )
    此函数就是用来关闭openlog打开的连接的。
syslog函数原型如下：    
void  syslog(int priority, const char *format, ...);      
    此函数用于把日志消息发给系统程序syslogd去记录。
    参数说明：
    priority：是消息的紧急级别；
    format：是消息的格式，之后是格式对应的参数。就是printf函数一样使用。
应用：
    如果我们的程序要使用系统日志功能，只需要在程序启动时使用openlog函数来连接syslogd程序，后面随时用syslog函数写日志就行了。
    另外，作为syslog的替代程序的新一代工具是syslog-ng，syslog-ng具有很强的网络功能，可以方便地把多台机器上的日志保存到一台中心日志服务器上。
