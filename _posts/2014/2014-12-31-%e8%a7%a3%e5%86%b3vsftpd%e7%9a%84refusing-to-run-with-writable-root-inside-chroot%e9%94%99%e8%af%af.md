---
layout: post
title: 解决vsftpd的refusing to run with writable root inside chroot错误
date: 2014-12-31 16:17
author: admin
comments: true
categories: []
---
升级vsftpd或者vsftpd-ext后，登录时可能会出现这样的错误：

500 OOPS: vsftpd: refusing to run with writable root inside chroot ()
这是由于下面的更新造成的：

- Add stronger checks for the configuration error of running with a writeable
root directory inside a chroot(). This may bite people who carelessly turned
on chroot_local_user but such is life.
问题的是因为用户的根目录可写，并且使用了chroot限制，而这在最近的更新里是不被允许的。要修复这个错误，可以用命令chmod a-w /home/user去除用户根目录的写权限，注意把目录替换成你自己的。

或者你可以在vsftpd的配置文件中增加下列两项中的一项：

对于标准的vsftpd build (vsftpd):

allow_writeable_chroot=YES
对于扩展的vsftpd build (vsftpd-ext):

allow_writable_chroot=YES


===========

最近在搞ubuntu，server版13.10
安装完ftp后，sudo atp-get install vsftpd
安装完后登陆不上，需要更改/etc/vsftpd.conf
local_root 设置根目录，前提的都是些权限方面的东西，设置即可。
设置完后，在ubuntu上面ftp localhost 可以正常登录，但是很ls 时报错：
 
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
500 OOPS: priv_sock_get_cmd
但是window下面 cmd 输入 ftp server 可以正常登录，ls正常显示和
winscp 软件提示无法列出目录。提示无法获得目录列表 OOPS: priv_sock_get_it
flashfxp能正常登录，但是server端什么都不显示，不会执行list命令。
解决办法如下：配置文件中添加：
seccomp_sandbox=NO
添加之后重启就好了
sudo /etc/init.d/vsftpd restart
 
参考文章：
http://stackoverflow.com/questions/16589570/windows-azure-ubuntu-500-oops-priv-sock-get-cmd-error-while-making-an-ftp-con
