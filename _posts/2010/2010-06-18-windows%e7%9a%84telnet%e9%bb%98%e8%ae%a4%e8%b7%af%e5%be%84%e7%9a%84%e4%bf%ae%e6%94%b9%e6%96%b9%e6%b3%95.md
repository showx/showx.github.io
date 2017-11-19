---
layout: post
title: windows的telnet默认路径的修改方法
date: 2010-06-18 15:35
author: admin
comments: true
categories: []
---
主机上安装ssh服务，我们管理的时候可以通过secureCRT进行ssh连接。由于运用的需求，我们要求ssh连接上服务器以后必须是在C:＼Documents and Settings＼Administrator&gt;，及当我们在服务器上运行--cmd 进入以后的默认路径是C:＼Documents and Settings＼Administrator&gt;。　　

　　但是不知道为什么。有些机器却没有这么标准，比如我的机器路径是C:＼Documents and Settings＼anywolfs.com&gt;，这让我很郁闷，我必须修改成标准的路径，今天发现一个很简单的方法，不过是手动的：　　

　　找到：HKEY_LOCAL_MACHINE＼SOFTWARE＼Microsoft＼Windows NT＼CurrentVersion＼ProfileList＼S-1-5-21-1790271045-1411784089-857767003-500＼　　

　　下的：ProfileImagePath ，把键值修改为为%SystemDrive%＼Documents and Settings＼Administrator　　

　　重启服务器就好了。　　

　　当然里面有一长串的SID 如：S-1-5-21-1790271045-1411784089-857767003-500，这个id每台机器都不一样，并且在HKEY_LOCAL_MACHINE＼SOFTWARE＼Microsoft＼Windows NT＼CurrentVersion＼ProfileList＼下有很多个这样的ID，我们怎么区分哪个是我们需要修改的呢？　　

　　首先进行一下：开始 --运行 --cmd ，这时候打开的命令行窗口，里面的默认根目录（比如我的C:＼Documents and Settings＼anywolfs.com&gt;）其实＼Documents and Settings＼anywolfs.com在注册表里就是一个键值部分，我们在HKEY_LOCAL_MACHINE＼SOFTWARE＼Microsoft＼Windows NT＼CurrentVersion＼ProfileList＼所有的id下找到那个类型为 REG_EXPAND_SZ 的主键ProfileImagePath的键值就可以了　　

　　%SystemDrive%＼Documents and Settings＼anywolfs.com　　

　　修改这个为%SystemDrive%＼Documents and Settings＼Administrator，搞定
