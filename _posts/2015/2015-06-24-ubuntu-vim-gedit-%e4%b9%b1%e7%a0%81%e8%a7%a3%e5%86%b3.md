---
layout: post
title: ubuntu vim gedit 乱码解决
date: 2015-06-24 11:02
author: admin
comments: true
categories: []
---
<strong>添加中文字符编码：</strong>

sudo vi /var/lib/locales/supported.d/local

#添加下面的中文字符集：

zh_CN.GBK GBK

zh_CN.GB2312 GB2312

zh_CN.GB18030 GB18030

#使其生效：

sudo dpkg-reconfigure locales

----------------------------华丽的分割线-----------------------------------
<div>
&nbsp;

<strong>vim:</strong>

sudo vi /etc/vim/vimrc

#打开vim的配置文件，在其中加入：

set fileencodings=utf-8,gb2312,gbk,gb18030

set termencoding=utf-8

set encoding=utf-8

#使其生效：source /etc/vim/vimrc

----------------------------华丽的分割线-----------------------------------

<strong>gedit：</strong>

#方法1：

gsettings set org.gnome.gedit.preferences.encodings auto-detected "['UTF-8','GB18030','GB2312','GBK','BIG5','CURRENT','UTF-16']"

#方法2：

gconf-editor (sudo apt-get install gconf-editor)

#选择 apps-&gt;gedit2-&gt;preferences-&gt;encodings，添加需要的字符集

</div>
