---
layout: post
title: 解决centos yum安装"No package nginx available."问题
date: 2015-01-31 10:42
author: admin
comments: true
categories: []
---
<strong>问题描述：</strong>

[root@Centos ~]# yum install nginx
Loaded plugins: fastestmirror
Repository base is listed more than once in the configuration
Repository updates is listed more than once in the configuration
Repository extras is listed more than once in the configuration
Repository centosplus is listed more than once in the configuration
Repository contrib is listed more than once in the configuration
Loading mirror speeds from cached hostfile
* addons: mirror.bit.edu.cn
* base: mirrors.btte.net
* extras: mirrors.btte.net
* updates: mirror.bit.edu.cn
Setting up Install Process
No package nginx available.
Nothing to do

<strong>问题原因：</strong>

nginx位于第三方的yum源里面，而不在centos官方yum源里面

<strong>解决方法：</strong>

安装epel(Extra Packages for Enterprise Linux)

a、去epel网站<a href="http://fedoraproject.org/wiki/EPEL" target="_blank">http://fedoraproject.org/wiki/EPEL下载</a>

b、我的系统是centos5.7，cpu是x86_64，所以我下载的是wget http://dl.fedoraproject.org/pub/epel/5/x86_64/epel-release-5-4.noarch.rpm

如果是centos6， 则应该下载 wget <a href="http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm" target="_blank">http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm</a>

c、安装epel

rpm -ivh epel-release-5-4.noarch.rpm

再次执行 yum install nginx,则会提示安装成功了

<strong>注：</strong>

epel的安装跟centos的系统版本、cpu硬件架构有关，

查看系统版本（lsb-release -a）,

查看cpu硬件架构(arch)

epel它是RHEL 的 Fedora 软件仓库，为 RHEL 及衍生发行版如 CentOS、Scientific Linux 等提供高质量软件包的项目。装上了 EPEL，就像在 Fedora 上一样，可以通过 yum install package-name，随意安装软件。
