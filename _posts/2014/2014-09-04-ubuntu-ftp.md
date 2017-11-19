---
layout: post
title: ubuntu ftp
date: 2014-09-04 15:32
author: admin
comments: true
categories: []
---
ubuntu安装ftp服务器

1: 安装vsftpd


~$ sudo apt-get install vsftpd 


ubuntu10.10自己装了，这步省略。

 


2: 配置vsftpd


2.1 修改vsftpd的配置文件。此类配置文件通常位于 /etc 目录下。


~$ sudo gedit /etc/vsftpd.conf


原文件中不少指令被注释，只要启用部分即可，一下是启用的命令（配置文件中对每一条都有具体说明）


listen=YES       # 服务器监听
anonymous_enable=YES       # 匿名访问允许
local_enable=YES    # 本地主机访问允许
write_enable=YES    # 写允许
anon_upload_enable=YES
# 匿名上传允许，默认是NO，嫌麻烦的可以开起来。出了问题我不负责～
anon_mkdir_write_enable=YES  # 匿名创建文件夹允许
dirmessage_enable=YES  # 进入文件夹允许
xferlog_enable=YES   #  ftp 日志记录允许
connect_from_port_20=YES     # 允许使用20号端口作为数据传送的端口
secure_chroot_dir=/var/run/vsftpd/empty
pam_service_name=vsftpd
rsa_cert_file=/etc/ssl/private/vsftpd.pem


保存。


 2.2  设置ftp相关目录
        安装完毕后，/srv下会增加一个ftp目录。同时系统会增加一个名为ftp的用户组，可以用~$ sudo cat    /etc/shadow 查看， 如 ftp:*:14993:0:99999:7:::。我们在/srv/ftp目录下创建两个分别名为upload和download的目录，分别用于上传和下载。接下来我们为刚才创建的几个目录设置权限，如下：


权限                            /srv/ftp                     /srv/ftp/upload                 /srv/ftp/download


用户组（ftp）            读                                  读写                                    读


其他用户                    读                                   读写                                    读                      


执行命令：


~$ sudo chmod 755 /home/ftp


~$ sudo chmod 777 /home/ftp/upload


~$ sudo chmod 755 /home/ftp/download


        如此，一方面我们允许了用户组ftp访问/home/ftp （匿名访问）；一方面赋予了用户组ftp对/srv/ftp/upload的写权利，因此网络上的用户可以方便地上传文件，但注意，当他们上传后，上传的文件只有root对这些文件拥有权限，也就是说这个目录仅能用于上传，无法下载其中的文件；此外赋予了用户组ftp对 /home/ftp/download的读权利，同时我们拷贝进该目录下的文件对于用户组而言通常都有读权利，因此网络上的用户从此目录下能且仅能下载文件。从而满足了我们预先的要求。

 


3：启动vsftpd


~$ sudo service vsftpd start


查看当前所有进程： ~$ ps -e


 2183 ?        00:00:00 vsftpd


        至此服务器端vsftp的最基本配置已完成，vsftpd已开启。（注意你的防火墙配置，作为简单试验可以直接停用防火墙）
        当然关闭vsftpd进程只需要执行~$ sudo service vsftpd stop，同时还可以使用命令~$ pgrep vsftpd 来查看进程vsftp是否存在。


4：vsftpd 设置用户目录，如果你设置了匿名用户也可以登录上传的话～这个可以省了～
(1) 增加组 sudo groupadd ftpgroup


(2 )修改vsftpd.conf

~$ sudo gedit /etc/vsftpd.conf
　　将底下三行
　　#chroot_list_enable=YES
　　# (default follows)
　　#chroot_list_file=/etc/vsftpd/chroot_list
　　改为
　　chroot_list_enable=YES
　　# (default follows)
　　chroot_list_file=/etc/vsftpd/chroot_list


(3) 增加用户ftpuser并设置其目录为/home/nation/ftp/upload
sudo useradd -g ftpgroup -d /home/nation/ftp/upload -M ftpuser

(注：G：用户所在的组 d：表示创建用户的自己目录的位置给予指定

M：不建立默认的自家目录，也就是说在/home下没有自己的目录)

(4 )设置用户口令 passwd ftpuser


(5) 编辑chroot_list文件:
sudo gedit /etc/vsftpd.chroot_list
内容为ftp用户名,每个用户占一行,如：

ftpuser


(6 )重新启动vsftpd：
sudo service vsftpd start
===========================================
ubuntu安装了vsftpd。

/etc/vsftpd.conf文件中已添加：
anonymous_enable=YES
anon_upload_enable=YES
anon_mkdir_write_enable=YES
已设定为允许anonymous登录与上传！

/etc/ftpusers文件中的root也已经注释掉了。现在也允许root登录。

但是我不想只允许anonymous上传，更想让root登录上传！

如何让root上传呢？该怎样设定？

希望知道的朋友帮忙解决下！
先谢过！
