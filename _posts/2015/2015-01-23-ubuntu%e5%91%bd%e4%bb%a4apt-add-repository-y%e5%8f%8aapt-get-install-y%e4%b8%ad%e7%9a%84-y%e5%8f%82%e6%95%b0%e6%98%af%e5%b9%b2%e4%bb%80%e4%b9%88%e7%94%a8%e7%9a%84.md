---
layout: post
title: ubuntu命令apt-add-repository -y及apt-get install -y中的-y参数是干什么用的
date: 2015-01-23 16:00
author: admin
comments: true
categories: []
---
就是如果只有sudo apt-get install 的话，如sudo apt-get install scilab,会显示

<a class="ikqb_img_alink" title="点击查看大图" href="http://a.hiphotos.baidu.com/zhidao/pic/item/7acb0a46f21fbe09d3e47a786a600c338744ad20.jpg" target="_blank" rel="nofollow"><img class="ikqb_img" src="http://a.hiphotos.baidu.com/zhidao/wh%3D600%2C800/sign=01bf170c4ec2d562f25dd8ebd721bcd7/7acb0a46f21fbe09d3e47a786a600c338744ad20.jpg" alt="" /></a>

就是让您确认安装

但是如果是sudo apt-get install scilab -y 那么，就不再显示上图中的信息，即当安装包的时候会询问y/n,这个参数是所有询问默认y，下边不再提醒，在终端输入以上命令时，直接下载安装，不再要求确认

<a class="ikqb_img_alink" title="点击查看大图" href="http://d.hiphotos.baidu.com/zhidao/pic/item/b2de9c82d158ccbf77e50d5618d8bc3eb1354104.jpg" target="_blank" rel="nofollow"><img class="ikqb_img" src="http://d.hiphotos.baidu.com/zhidao/wh%3D600%2C800/sign=1440cffbd833c895a62b907de1235fc8/b2de9c82d158ccbf77e50d5618d8bc3eb1354104.jpg" alt="" /></a>

apt-add-repository -y完全类似
