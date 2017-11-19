---
layout: post
title: Nginx报错403 forbidden (13: Permission denied)的解决办法
date: 2015-02-03 13:41
author: admin
comments: true
categories: []
---
由于开发需要，在本地环境中配置了LNMP环境，使用的是Centos 6.5 的yum安装，安装一切正常，但是由于默认网站文件夹比较奇葩，于是把网站文件用mv命令移动到了新的目录，并相应修改了配置文件，并重启Nginx。

那么好，问题来了！本以为重启就OK了。居然报个“403 is forbidden“的错误。。查看/var/log/nginx/error.log日志显示：xxx 403 forbidden (13: Permission denied)错误。我勒个去~

引起nginx 403 forbidden通常是三种情况：<strong>一是缺少索引文件，二是权限问题，三是SELinux状态。</strong>

一、缺少index.html或者index.php文件，就是配置文件中index index.html index.htm这行中的指定的文件。
<ol class="linenums">
	<li class="L0"><span class="pln">server </span><span class="pun">{</span></li>
	<li class="L1"><span class="pln"> listen </span><span class="lit">80</span><span class="pun">;</span></li>
	<li class="L2"><span class="pln"> server_name localhost</span><span class="pun">;</span></li>
	<li class="L3"><span class="pln"> index index</span><span class="pun">.</span><span class="pln">php index</span><span class="pun">.</span><span class="pln">html</span><span class="pun">;</span></li>
	<li class="L4"><span class="pln"> root </span><span class="pun">/</span> <span class="kwd">var</span><span class="pun">/</span><span class="pln">www</span><span class="pun">;</span></li>
	<li class="L5"><span class="pun">}</span></li>
</ol>
如果在/ var/www下面没有index.php,index.html的时候，直接访问域名，找不到文件，会报403 forbidden。

二、权限问题，如果nginx没有web目录的操作权限，也会出现403错误。

解决办法：修改web目录的读写权限，或者是把nginx的启动用户改成目录的所属用户，重启Nginx即可解决
<ol class="linenums">
	<li class="L0"><span class="pln">chmod </span><span class="pun">-</span><span class="pln">R </span><span class="lit">755</span> <span class="pun">/</span> <span class="kwd">var</span><span class="pun">/</span><span class="pln">www</span></li>
</ol>
三、SELinux设置为开启状态（enabled）的原因

首先查看本机SELinux的开启状态，如果SELinux status参数为enabled即为开启状态
<ol class="linenums">
	<li class="L0"><span class="str">/usr/</span><span class="pln">sbin</span><span class="pun">/</span><span class="pln"> sestatus </span><span class="pun">-</span><span class="pln">v</span></li>
</ol>
或者使用getenforce命令检查
<h5>找到原因了，如何关闭 SELinux 呢</h5>
1、临时关闭（不用重启）
<ol class="linenums">
	<li class="L0"><span class="pln">setenforce </span><span class="lit">0</span></li>
</ol>
2、修改配置文件 /etc/ selinux/config，将SELINUX=enforcing改为SELINUX=disabled
<ol class="linenums">
	<li class="L0"><span class="pln">vi </span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln"> selinux</span><span class="pun">/</span><span class="pln">config</span></li>
</ol>
<img src="http://www.hi-docs.com/static/img/nginx-selinux.jpg" alt="Nginx报错403 forbidden (13: Permission denied)的解决办法" width="100%" />

注意：修改配置文件需要重启系统 reboot
