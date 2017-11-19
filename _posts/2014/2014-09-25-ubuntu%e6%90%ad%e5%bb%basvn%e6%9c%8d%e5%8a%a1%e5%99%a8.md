---
layout: post
title: Ubuntu搭建SVN服务器
date: 2014-09-25 20:43
author: admin
comments: true
categories: []
---
Ubuntu 10.04
Subversion 1.6.6

1、 SVN安装
$ sudo apt-get install subversion
2、 添加SVN管理用户及subversion组
# adduser svnuser
# addgroup subversion
# addgroup svnuser subversion
3、 创建项目目录
# mkdir /home/svn
# cd /home/svn
# mkdir myProject
# chown -R root:subversion myProject
# chmod -R g+rws myProject
4、 创建SVN文件仓库
# svnadmin create /home/svn/myProject
myProject文件夹必须为空
5、 修改文件仓库访问权限
# chmod 700 /home/svn/myProject
6、 设置访问权限
位于/home/svn/myProject/conf/文件夹下的authz、passwd、svnserve.conf文件
svnserve.conf：svn服务配置文件，该文件版本库目录的conf目录下。 
passwd：用户名口令文件，该文件名在文件svnserve.conf中指定，缺省为同目录下的。 
authz：权限配置文件，该文件名也在文件svnserve.conf中指定，缺省为同目录下的。
(1)设置svnserve.conf
# vim svnserve.conf
取消一下四行的注释
anon-access = read
auth-access = write
password-db = password
authz-db = authz
并将anon-access = read的read改为none，禁止匿名用户访问。
(2)设置passwd
# vim passwd
[users]
admin = admin
user = user
设置两个用户admin和user
(3)设置authz
# vim authz
[groups]
admin = admin
user = user
[/]
@admin=rw
*=r
admin属于admin组，具有读写权限；
user用户属于user组，具有读权限。
7、 启动SVN服务
# svnserve -d -r /home/svn
-d 表示以守护进程模式运行
-r 指定SVN根目录
8、设置SVN开机启动
(1).创建执行脚本svn.sh（/root路径下）
#!/bin/bash
svnserve -d -r /home/svn
(2).添加可执行权限
#chmod ug+x /root/svn.sh
(3).添加自动运行
#vim /etc/init.d/rc.local
在最后添加一行内容如下：
/root/svn.sh
(4).检查
重启服务器，使用ps -aux |grep svn看看svn进程是否启动了。
