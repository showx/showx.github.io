---
layout: post
title: svn提交时出现svn: The log message is a pathname (was -F intended?); use '--force-log' to override
date: 2014-03-16 22:27
author: admin
comments: true
categories: []
---
今天一同事提交修改时遇见了这个问题

&nbsp;
<div>
<div>
<div><b>[python]</b> <a title="view plain" href="http://blog.csdn.net/jk110333/article/details/8468357#">view plain</a><a title="copy" href="http://blog.csdn.net/jk110333/article/details/8468357#">copy</a>
<div></div>
</div>
</div>
<ol start="1">
	<li>[root@root  src] # svn  ci  Makefile  –m  “settime”</li>
	<li>svn: The log message is a pathname (was -F intended?); use '--force-log' to override</li>
</ol>
</div>
&nbsp;

&nbsp;
<p align="left">当提交时出现了上面的错误，大意是log信息与一个文件名相同，使用“--force-log”选项强制执行此命令。</p>
<p align="left">其实这个错误时应为log信息和当前文件夹下一个文件的名字相同造成的，随便修改下-m的信息就行了，如下</p>
&nbsp;
<div>
<div>
<div><b>[python]</b> <a title="view plain" href="http://blog.csdn.net/jk110333/article/details/8468357#">view plain</a><a title="copy" href="http://blog.csdn.net/jk110333/article/details/8468357#">copy</a>
<div></div>
</div>
</div>
<ol start="1">
	<li>[root@root src]# svn ci Makefile –m “settime123-”</li>
	<li>Sending        Makefile</li>
	<li>Transmitting file data .</li>
	<li>Committed revision 60.</li>
	<li></li>
</ol>
</div>
