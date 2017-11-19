---
layout: post
title: ubuntu 下修改host文件
date: 2015-08-19 08:54
author: admin
comments: true
categories: []
---
有些时候，我们需要让某些域名指向本地，来实现调试，下面介绍下ubuntu下修改host实现域名指向本地的方法。
Ubuntu系统的Hosts只需修改/etc/hosts文件，在目录中还有一个hosts.conf文件，刚开始还以为只需要修改这个就可以了，结果发现是需要修改hosts。修改完之后要重启网络。
打开host文件
vim gedit /etc/hosts
添加你需要的项，格式如下：
格式:
<b>    &lt;IP&gt; &lt;HOSTNAME&gt;.&lt;DOMAIN&gt; &lt;ALIAS&gt;</b>
示例:
216.239.37.99 www.google.com
编辑后，你需要重新启动一下你的网络。
/etc/init.d/networking restart
