---
layout: post
title: Mac OS X中MacPorts安装和使用
date: 2015-08-18 22:23
author: admin
comments: true
categories: []
---

Mac下面除了用dmg、pkg来安装软件外，比较方便的还有用MacPorts来帮助你安装其他应用程序，跟BSD中的ports道理一样。MacPorts就像apt-get、yum一样，可以快速安装些软件。
下面将MacPorts的安装和使用方法记录在这里以备查。
访问官方网站http://www.macports.org/install.php，这里提供有dmg安装和源码安装两种方式，dmg就多说了，下载MacPorts-1.9.2-10.6-SnowLeopard.dmg，下一步下一步安装即可。
通过Source安装MacPorts


wget http://distfiles.macports.org/MacPorts/MacPorts-1.9.2.tar.gz

tar zxvf MacPorts-1.9.2.tar.gz

cd MacPorts-1.9.2

./configure && make && sudo make install

cd ../

rm -rf MacPorts-1.9.2*
然后将/opt/local/bin和/opt/local/sbin添加到$PATH搜索路径中
编辑/etc/profile文件中，加上


export PATH=/opt/local/bin:$PATH

export PATH=/opt/local/sbin:$PATH

MacPorts使用

更新ports tree和MacPorts版本，强烈推荐第一次运行的时候使用-v参数，显示详细的更新过程。

sudo port -v selfupdate
搜索索引中的软件

port search name
安装新软件

sudo port install name
卸载软件

sudo port uninstall name
查看有更新的软件以及版本

port outdated
升级可以更新的软件

sudo port upgrade outdated
Eclipse的插件需要subclipse需要JavaHL，下面通过MacPorts来安装


sudo port install subversion-javahlbindings
