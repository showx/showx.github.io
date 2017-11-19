---
layout: post
title: SecureCRT设置
date: 2015-08-18 10:54
author: admin
comments: true
categories: []
---
<h2>SecureCRT设置</h2>
本文主要介绍SecureCRT的使用方法和技巧。
<h4>一、基本设置</h4>
1、修改设置

为了SecureCRT用起来更方便，需要做一些设置，需要修改的有如下几处：

1、退出主机自动关闭窗口

Options =&gt; Global ptions =&gt; General =&gt; Default Session =&gt; Edit Default Settings...

<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909234211.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" />

Terminal中将Close on disconnect 选上，当用户从主机中退出后可以自动关闭当前连接的窗口。

<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909234413.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" />

2、修改默认卷屏行数

当你做一个操作，屏幕输出有上百行，当需要将屏幕回翻时，这个设置会有很大帮助，默认为500行，可以改为10000行，不用担心找不到了。

Terminal =&gt; Emulation =&gt; Scrollback 修改为10000。

<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909234714.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" />

3、修改SFTP默认<a class="keylink" href="http://www.2cto.com/soft" target="_blank">下载</a>路径（可选）：

对于使用SSH的连接中，可以使用SFTP下载文件，在这里可以设置文件的下载目录（默认为下载到“我的文档”中）

Connection =&gt; SSH2 =&gt; SFTP Tab =&gt; Initial directories =&gt; Local directory

<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909234815.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" />

4、修改Xmodem/Zmodem上传下载路径（可选）

SecureCRT可以使用Xmodem/Zmodem方便的上传和下载文件。

在Session ptions =&gt;Xmodem/Zmodem =&gt; Directories中设置

5、拷贝与粘贴的设置

通过鼠标操作即可拷贝或粘贴所需内容是一个非常方便的设置

Options =&gt; Global ptions =&gt; Terminal =&gt; Mouse

选中Copy on select 和 Paste on middle button

这样设置后，只要用鼠标选中所需内容，则将内容拷贝到剪切板中，点击鼠标中键即可粘贴内容。

<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909234816.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" />

另外可以设置使用Windows下的拷贝粘贴快捷键，Options =&gt; Global ptions =&gt; General =&gt; Default Session =&gt; Edit Default Settings... =&gt; Terminal =&gt; Mapped keys =&gt; Use windows copy and paste hotkeys

<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909234917.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" />

6、Tabs设置

从Secure5.0以后，增加了Tabs（标签）选项，多个连接可以在同一个窗口下打开，类似IE7.0的风格。将Double-click 选项修改为 Close Tab，双击标签可关闭连接窗口。

<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909235019.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" width="367" height="210" border="0" />
<h4>二、界面介绍</h4>
1、菜单

1）File文件

Connect... 连接，打开一个连接或者编辑已有的连接，创建新连接。

Quick Connect... 快速连接，快速连接对话框，快速连接主机的最便捷方式

Connect in Tab... 在Tab中打开一个新的会话窗口。

Clone Session 克隆当前会话窗口。

Connect SFTP Tab 打开SFTP窗口，对于SSH连接，此选项可用。在此会话窗口中可使用SFTP命令传输文件。

Reconnect 重新连接

Disconnect 中断当前会话窗口的连接

Log Session 把当前窗口的会话记录到log文件中。

Raw Log Session 将更详细的会话记录到log文件中，包括服务器更详细的响应信息。

Trace Options 在log文件中记录协议会话信息选项。（包括客户端与主机互相连接时的一些信息内容）

2）Edit编辑

拷贝粘贴等

3) View视图

显示各种工具条

4) Options选项

包括全局选项和Session选项

5) Transfer传递文件

使用Xmodem/Zmodem上传下载文件

6) Script.脚本

运行一个脚本文件，或记录一个新的脚本。（类似Word中的宏功能）

7) Tools工具

键盘映射编辑，密钥生成工具等

8) Help帮助

2、对话框和按钮

点击File =&gt; Connect可出现Connect对话框。

从左至右按钮依次为：

连接（激活选中的连接条目）；快速连接（快捷连接新的主机）；新建连接（在对话框中新增一个连接条目）；剪切；复制；粘贴；删除（对话框中的条目）；新建文件夹，属性（显示选中条目的属性），创建条目的桌面快捷方式，帮助。

<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909235220.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" />

Connect对话框下方有两个选项：

Show dialog on start (启动SecureCRT时显示Connect对话框)；

Open in a tab (以新标签卡的形式打开一个会话)，选中此选项，新的会话窗口如下图所示：

<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909235526.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" />

否则将打开多个SecureCRT窗口：<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909235727.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" />
<h4>三、使用方法</h4>
1、新建连接

File =&gt; Connect =&gt; 点击 New Session 按钮，出现以下窗口，填写连接的名字，协议（SSH1，SSH2，Telnet, Rlogin等）

<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909235828.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" /><img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909240029.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" />

点击SSH2选项，填写主机名或者IP地址，端口号，用户名。另外可设置会话窗口的颜色方案，点击Appearance选项，可自己设计或者选择已有的颜色方案，更改字体，光标等。

<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909240130.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" /><img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909240231.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" />

2、快速连接

点击快速连接按钮出现下面的对话框，填入主机信息和用户名即可快速连接。

下面有两个选项Save session(保存快速连接的信息到连接对话框中)；Open in a tab (以新标签卡的形式打开一个会话)

<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909240332.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" />

3、使用SSH连接主机

按照上面的介绍新建一个SSH连接，如果是第一次连接会有如下提示，点击Accept &amp; Save即可。

<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909240438.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" />

对于SSH连接，鼠标右键单击条目卡，可出现右键菜单，单击其中的Connect SFTP Tab，可打开SFTP窗口

<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909240639.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" /><img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909240743.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" />

可使用SFTP命令下载和上传文件，本地路径设置见Connection =&gt; SSH2 =&gt; SFTP Tab =&gt; Initial directories =&gt; Local directory，默认为“我的文档”。

基本的SFTP命令：

get [-a | -b] remote-path 下载文件，(-a) 强制使用ascii模式，(-b)强制使用binary模式

put [-a | -b] local-path 上传文件，(-a) 强制使用ascii模式，(-b)强制使用binary模式

建议使用-b选项，否则上传到UNIX或LINUX主机上的文件后有^M字符。

4、使用Telnet连接主机

新建一个Telnet连接，在Telnet选项中填写主机IP，端口号信息。

<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909240844.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" /><img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909240945.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" />

在<a class="keylink" href="http://www.2cto.com/os/linux/" target="_blank">Linux</a>主机下，可以使用Xmodem/Zmodem方便的上传和下载文件

基本命令：sz 下载文件到本地；rz 上传本地文件到主机。

5、其它技巧

1）使用脚本来进行重复性工作

可以像word的宏一样，把你的重复性操作记录为一个脚本文件

Script. =&gt; Start Recording Script, 开始记录

Script. =&gt; Stop Recording Script，停止记录， Save as …保存成script文件。下次调用时Script. =&gt; Run =&gt; Select Script. to run …

<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909241046.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" />

2)使用Map key来进行重复输入工作

可以设置为全局选项（对所有连接都有效），也可以只设置为Session选项，如下图

Options =&gt; Session ptions =&gt; Terminal =&gt; Mapped keys =&gt; Map a key，出现Map Key 对话框

<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909241147.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" /><img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909241248.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" />

例如，单击F12键，在Send String 输入你要经常重复使用的命令，ok

<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909241349.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" />

则下次在会话窗口中点击F12键将直接输入df –m

3）自动登录

以登录一个Telnet的主机为例，Session ptions =&gt; Connection =&gt; Logon Scripts =&gt; Automate logon, 在login后的send中输入用户名，在Password后的send中输入密码。则可实现自动登录。

<img src="http://www.2cto.com/uploadfile/Collfiles/20141009/2014100909241450.jpg" alt="SecureCRT设置 - robin - 宁静致远的博客" border="0" />

用SecureCRT来上传和下载数据

今天才知道，原来SecureCRT可以使用linux下的zmodem协议来快速的传送文件,而且还使用非常方便哦，我还傻傻的找其他软件来sftp，笨死了：（

你只要设置一下上传和下载的默认目录就行

options--&gt;session options--&gt;file transfer 下可以设置上传和下载的目录

剩下的你只要在用SecureCRT登陆linux终端的时候：

发送文件到客户端：

sz filename

zmodem接收可以自行启动.

从客户端上传文件到linux服务端：

只要服务端执行,

rz

然后在 SecureCRT 里选文件发送,协议 zmodem

简单吧，如果你以前一直使用ssh，而又没有对外开放ftp服务，你就直接使用这种方式来传输你的文件吧，很方便哦：）
