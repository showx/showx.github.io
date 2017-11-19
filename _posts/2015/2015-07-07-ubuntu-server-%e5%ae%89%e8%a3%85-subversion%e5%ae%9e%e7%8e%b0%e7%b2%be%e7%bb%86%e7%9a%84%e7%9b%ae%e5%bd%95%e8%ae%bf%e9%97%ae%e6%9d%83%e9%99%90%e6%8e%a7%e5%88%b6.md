---
layout: post
title: Ubuntu Server 安装 Subversion实现精细的目录访问权限控制
date: 2015-07-07 13:54
author: admin
comments: true
categories: []
---

安装Subversion和Apache

sudo apt-get install subversion libapache2-svn

创建组subversion,并配置权限将自己和www-data(Apache用户)加入组成员

sudo addgroup subversion

 sudo usermod -G subversion -a www-data

创建代码仓库, 这里我的项目是arlicle

sudo mkdir /home/svn

 cd /home/svn 

sudo mkdir arlicle

 sudo chown -R root:subversion arlicle

 sudo svnadmin create /home/svn/arlicle

对所有使用成员设置权限

sudo chmod -R g+rws arlicle

 sudo chown -R root:subversion arlicle

设置用户访问权限 
修改/etc/apache2/mods-available/dav_svn.conf

DAV svn 

SVNPath /home/svn/arlicle

 AuthType Basic 

AuthName "arlicle subversion repository" 

AuthUserFile /etc/subversion/passwd Require valid-user

重启Apache

sudo /etc/init.d/apach2 restart

创建密码文件和访问用户并设置密码

创建密码文件 和用户for 首次创建

sudo htpasswd -c /etc/subversion/passwd edison

添加一个用户

sudo htpasswd -c /etc/subversion/passwd edison2

这样就可以访问代码仓库了

svn co http://www.arlicle.com/svn/arlicle arlicle --username edison

如果要设置更详细的访问权限, 
修改/etc/apache2/mods-available/dav_svn.conf

DAV svn 

SVNPath /home/svn/arlicle

 AuthType Basic 

AuthName "arlicle subversion repository" 

AuthUserFile /etc/subversion/passwd 

AuthzSVNAccessFile /etc/subversion/accessfile

 Require valid-user

然后编辑accessfile文件,控制每个目录的访问和读写权限

[arlicle:/]
user1 = r
user2 = r
user3 = r
[arlicle:/folder1]
user1 = rw
[arlicle:/folder2]
user2 = rw
也可以创建多个代码仓库
修改/etc/apache2/mods-available/dav_svn.conf

<Location /svn/arlicle1>
DAV svn
SVNPath /home/svn/arlicle1
AuthType Basic
AuthName "arlicle subversion repository"
AuthUserFile /etc/subversion/passwd
AuthzSVNAccessFile /etc/subversion/accessfile
Require valid-user
</Location>

<Location /svn/arlicle2>
DAV svn
SVNPath /home/svn/arlicle2
AuthType Basic
AuthName "arlicle subversion repository"
AuthUserFile /etc/subversion/passwd
AuthzSVNAccessFile /etc/subversion/accessfile
Require valid-user
</Location>

