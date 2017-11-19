---
layout: post
title: Linux_ftp_命令行下下载文件get与上传文件put的命令应用
date: 2014-12-31 16:16
author: admin
comments: true
categories: []
---
 
 
Server Ubuntu 10.04上所使用的ftp服务器软件名称是vsftpd。
 
介绍：从本地以用户anok登录的机器192.168.0.16上通过ftp远程登录到192.168.0.6的ftp服务器上，登录用户名是peo。以下为使用该连接做的实验。
 
查看远程ftp服务器上用户peo相应目录下的文件所使用的命令为：ls，登录到ftp后在ftp命令提示符下查看本地机器用户anok相应目录下文件的命令是：!ls。查询ftp命令可在提示符下输入：?，然后回车。
 
1、从远程ftp服务器下载文件的命令格式：
get  远程ftp服务器上当前目录下要下载的文件名  [下载到本地机器上当前目录时的文件名]，如：
get  nmap_file  [nmap]
意思是把远程ftp服务器下的文件nmap_file下载到本地机器的当前目录下，名称更改为nmap。
带括号表示可写可不写，不写的话是以该文件名下载。
 
如果要往ftp服务器上上传文件的话需要去修改一下vsftpd的配置文件，名称是vsftpd.conf，在/etc目录下。要把其中的“#write_enable=YES”前面的“#”去掉并保存，然后重启vsftpd服务：
sudo service vsftpd restart。
 
2、向远程ftp服务器上传文件的命令格式：
put  本地机器上当前目录下要上传的文件名  [上传到远程ftp服务器上当前目录时的文件名]，如：
put  sample.c  [ftp_sample.c]
意思是把本地机器当前目录下的文件smaple.c上传到远程ftp服务器的当前目录下，名称更改为ftp_sample.c。
带括号表示可写可不写，不写的话是以该文件名上传。
 
3、最后附上ftp常用命令，如下所示：
 
FTP>open  [ftpservername]，和指定的远程Linux FTP服务器连接｡
FTP>user  [username]  [password]，使用指定远程Linux FTP服务器的用户登录｡
FTP>pwd，显示远程Linux FTP服务器上的当前路径｡
FTP>ls，列出远程Linux FTP服务器上当前路径下的目录和文件｡
FTP>dir，列出远程Linux FTP服务器上当前路径下的目录和文件(同上)｡
FTP>mkdir  [foldname]，在远程Linux FTP服务器上当前路径下建立指定目录｡
FTP>rmdir  [foldname]，删除远程Linux FTP服务器上当前路径下的指定目录｡
FTP>cd  [foldname]，更改远程Linux FTP服务器上的工作目录｡
FTP>delete  [filename]，删除远程Linux FTP服务器上指定的文件｡
FTP>rename  [filename]  [newfilename]，重命名远程Linux FTP服务器上指定的文件｡
FTP>close，从远程Linux FTP服务器断开但保留FTP命令参数提示｡
FTP>disconnect，从远程Linux FTP服务器断开但保留FTP命令参数提示(同上)｡ 
FTP>bye，结束和远程Linux FTP服务器的连接。
FTP>quit，结束和远程Linux FTP服务器的连接(同上)。
FTP>!，直接从远程Linux FTP服务器进入到本地shell中｡
FTP>exit，(接上步)从本地shell环境中返回到远程Linux FTP服务器环境下｡

FTP>!ls，列出本地机器上当前路径下的目录和文件｡
FTP>lcd  [foldname]，更改本地机器的工作目录｡

FTP>?，显示ftp命令说明｡
FTP>help，显示ftp命令说明(同上)｡
 
至此。
