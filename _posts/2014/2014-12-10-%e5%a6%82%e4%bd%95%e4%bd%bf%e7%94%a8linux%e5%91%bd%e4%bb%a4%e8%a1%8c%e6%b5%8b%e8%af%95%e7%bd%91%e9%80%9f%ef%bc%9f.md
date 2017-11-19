---
layout: post
title: 如何使用Linux命令行测试网速？
date: 2014-12-10 11:12
author: admin
comments: true
categories: []
---
<a title="使用Linux命令行测试网速" href="http://www.geekfan.net/wp-content/uploads/362f19305117711532eaa46a582c08eb.png" rel="lightbox[5521]"><img alt="linux_command_net_speed_0" src="http://s7.51cto.com/wyfs02/M00/11/CF/wKiom1LfLmuQ8vzUAAAmgNSUIXk878.jpg" /></a>

当发现上网速度变慢时，人们通常会先首先测试自己的电脑到网络服务提供商（通常被称为“最后一公里”）的网络连接速度。在可用于测试宽带速度的网站中，Speedtest.net也许是使用最广泛的。

Speedtest.net的工作原理并不复杂：它在你的浏览器中加载JavaScript代码并自动检测离你最近的Speedtest.net服务器，然后向服务器发送HTTP GET and POST请求来测试上行/下行网速。

但在没有图形化桌面时（例如，当你通过命令行远程登陆服务器或使用没有图形界面的操作系统），基于flash、界面友好的Speedtest.net将无法工作。幸运的是，Speedtest.net提供了一个命令行版本——speedtest-cli。下面我将向你演示如何在Linux的命令行中使用speedtest-cli来测试宽带连接速度。

<strong>安装speedtest-cli</strong>

speedtest-cli是一个用Python编写的轻量级Linux命令行工具，在Python2.4至3.4版本下均可运行。它基于Speedtest.net的基础架构来测量网络的上/下行速率。安装speedtest-cli很简单——只需要下载其Python脚本文件。
<ol>
	<li>$ wget https://raw.github.com/sivel/speedtest-cli/master/speedtest_cli.py</li>
	<li>$ chmod a+rx speedtest_cli.py</li>
	<li>$ sudo mv speedtest_cli.py /usr/local/bin/speedtest-cli</li>
	<li>$ sudo chown root:root /usr/local/bin/speedtest-cli&lt;/p&gt;</li>
</ol>
<strong>使用speedtest-cli测试网速</strong>

使用speedtest-cli命令也很简单，它不需要任何参数即可工作。
<ol>
	<li>$ speedtest-cli</li>
</ol>
输入这个命令后，它会自动发现离你最近的Speedtest.net服务器（地理距离），然后打印出测试的网络上/下行速率。

<a title="使用Linux命令行测试网速" href="http://s9.51cto.com/wyfs02/M02/11/CE/wKioL1LfLkmiLFmNAABzj7zmZ2I563.jpg" rel="lightbox[5521]"><img alt="linux_command_net_speed_1" src="http://s9.51cto.com/wyfs02/M02/11/CE/wKioL1LfLkmiLFmNAABzj7zmZ2I563.jpg" width="498" /></a>

如果你愿意分享测试结果，你可以使用参数“–share”。它将会把你的测试结果上传到Speedtest.net服务器并以图形的方式分享给其他人。

<a title="使用Linux命令行测试网速" href="http://s5.51cto.com/wyfs02/M02/11/CE/wKioL1LfLkqBai8zAACRKFfrI8w388.jpg" rel="lightbox[5521]"><img alt="linux_command_net_speed_2" src="http://s5.51cto.com/wyfs02/M02/11/CE/wKioL1LfLkqBai8zAACRKFfrI8w388.jpg" width="498" /></a>

下面是一幅由speedtest-cli自动生成并上传到Speedtest.net的测试结果：

<a title="使用Linux命令行测试网速" href="http://s3.51cto.com/wyfs02/M01/11/CF/wKiom1LfLm6g-TTgAAA2CMlyqYI659.jpg" rel="lightbox[5521]"><img alt="linux_command_net_speed_3" src="http://s3.51cto.com/wyfs02/M01/11/CF/wKiom1LfLm6g-TTgAAA2CMlyqYI659.jpg" /></a>

如果你对目前所有可用的Speedtest.net服务器感兴趣，你可以使用参数“–list”。它会打印出所有的Speedtest.net服务器（按照离你的地理距离由近及远排序）。

<a title="使用Linux命令行测试网速" href="http://s5.51cto.com/wyfs02/M01/11/CF/wKiom1LfLnKBA0AKAADi78WNAvQ048.jpg" rel="lightbox[5521]"><img alt="linux_command_net_speed_4" src="http://s5.51cto.com/wyfs02/M01/11/CF/wKiom1LfLnKBA0AKAADi78WNAvQ048.jpg" width="498" /></a>

在上面的列表中，每个服务器的前面都有一个与其对应的ID。如果想使用指定的服务器来测试你的网速，你只需要在speedtest-cli命令后指定其ID即可。例如，如果想使用在Washington DC的服务器，你只需要指定相对应的服务器ID（如935）。

&nbsp;
