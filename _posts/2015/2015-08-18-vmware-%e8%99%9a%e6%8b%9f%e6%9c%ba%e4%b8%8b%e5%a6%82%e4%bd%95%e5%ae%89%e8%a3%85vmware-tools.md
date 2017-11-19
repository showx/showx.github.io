---
layout: post
title: VMware 虚拟机下如何安装VMware tools
date: 2015-08-18 22:25
author: admin
comments: true
categories: []
---

VMware 虚拟机下如何安装VMware tools
1、选择菜单栏VM->setting 下，选中CD/DVD，右边点选Use ISO image file，选择镜像路径为VMware安装文件夹下的Linux.iso
2、选择菜单栏VM->install VMware tools下，会弹出VMware tools文件夹，里面有一个压缩包VMwareTools-8.4.5-324285.tar.gz
3、在桌面home文件夹里新建一个temp文件夹，将压缩包右键copy到这里来
4、新建终端，输入
cd temp
tar xzvf VMwareTools-8.4.5-324285.tar.gz
解压缩好后
cd vmware-tools-distrib
然后用root用户运行文件，即输入 sudo ./vmware-install.pl
注：网上很多资料都没有红色字体sudo，所以执行不了
5、开始安装了，没有yes提示时，按回车，有yes提示，输入yes，按照提示一步一步安装
注：第一次yes处，等的时间会比较久，不要以为是卡住了
6、直到出现enjoy vmware
7、OK，安装好了，要重启下，输入sudo reboot
8、恭喜了，安装好后，就可以在虚拟机和window间自由移动鼠标，而且虚拟机里鼠标移动更流畅了。
9、此时，打开VM->setting 里，选Options下的Shared Folders ，点选always enable，在下面Add..添加你想和
window硬盘共享的一个文件夹，共享的文件夹在filesystem  mnt/hgfs/下找到
