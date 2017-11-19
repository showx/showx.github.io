---
layout: post
title: ubuntu下设置开机自启动项
date: 2015-01-21 21:12
author: admin
comments: true
categories: []
---
<div class="postTitle"><a id="cb_post_title_url" class="postTitle2" href="http://www.cnblogs.com/end/archive/2012/10/12/2721059.html">ubuntu下设置开机自启动项</a></div>
<div id="cnblogs_post_body">
<div>这里说明，<strong>Ubuntu</strong> 中系统没有了<strong>RH</strong>系统中的<em> chkconfig </em>命令 ！可用一些小工具来管理<strong> Ubuntu</strong> 的启动选项：
<em>小工具</em> <strong>rcconf</strong>：
<strong>#sudo apt-get rcconf</strong>
<strong>#sudo apt-get install rcconf</strong>
root 下运行: <strong>#sudo rcconf</strong>
<em><strong>功能更全的工具</strong></em>：<strong>sysv-rc-conf</strong>
<strong>#sudo apt-get update</strong>
<strong>#sudo apt-get install sysv-rc-conf</strong>
运行：<strong>#sudo sysv-rc-conf</strong>
也可以直接加入启动程序，例如把 /etc/init.d/red5 加入到系统自动启动列表中：
<strong><code>#sudo sysv-rc-conf red5 on</code></strong>
其他使用方法见: google::Ubuntu::sysv-rc-conf 命令用法

也可以直接修改
直接改 /etc/rc0.d ~ /etc/rc6.d 和 /etc/rcS.d 下的东西，<strong>S</strong>开头的表示启动，<strong>K</strong>开头的表示不启动，
例如：想关闭 Red5 的开机自动启动，只需 #sudo mv /etc/rc2.d/S20red5 /etc/rc2.d/K20red5 就可以了。

<strong>Ubuntu自动启动程序</strong>

首 先，linux随机启动的服务程序都在/etc/init.d这个文件夹里，里面的文件全部都是脚本文件（脚本程序简单的说就是把要运行的程序写 到一个 文件里让系统能够按顺序执行，类似windows下的autorun.dat文件），另外在/etc这个文件夹里还有诸如名为rc1.d, rc2.d一直到rc6.d的文件夹，这些都是linux不同的runlevel，我们一般进入的X windows多用户的运行级别是第5级，也就是rc5.d，在这个文件夹下的脚本文件就是运行第5级时要随机启动的服务程序。需要注意的是，在每个rc (1-6).d文件夹下的文件其实都是/etc/init.d文件夹下的文件的一个软连接（类似windows中的快捷方式），也就是说，在 /etc/init.d文件夹下是全部的服务程序，而每个rc(1-6).d只链接它自己启动需要的相应的服务程序！

要 启动scim (某一程序)，我们首先要知道scim程序在哪里，用locate命令可以找到，scim在/usr/bin/scim这里，其中usr表 示是 属于用户的，bin在linux里表示可以执行的程序。这样，我就可以编写一个脚本程序，把它放到/etc/init.d里，然后在rc5.d里做一个相 应的软链接就可以了。

这个脚本其实很简单，就两行：

#!/bin/bash

/usr/bin/scim

第一行是声明用什么终端运行这个脚本，第二行就是要运行的命令。

还 需要注意的一点是，在rc5.d里，每个链接的名字都是以S或者K开头的，S开头的表示是系统启动是要随机启动的，K开头的是不随机启动的。这 样，你就可以知道，如果我要哪个服务随机启动，就把它名字第一个字母K改成S就可以了，当然，把S改成K后，这个服务就不能随机启动了。因此，我这个链接 还要起名为SXXX，这样系统才能让它随机启动。
<pre>在RH下，rc.local是默认启动的最后一个脚本文件，所以，</pre>
<pre>如果你想要随机启动，还有一种方法就是在rc.local的尾部加入/usr/bin/scim，这样就可以了。</pre>
<strong>Linux 自动启动程序</strong>

<strong>1．开机启动时自动运行程序</strong>

Linux 加载后, 它将初始化硬件和设备驱动, 然后运行第一个进程init。init根据配置文件继续引导过程，启动其它进程。通常情况下，修改放置在 /etc/rc或 /etc/rc.d 或 /etc/rc?.d 目录下的脚本文件，可以使init自动启动其它程序。例如：编辑 /etc/rc.d/rc.local 文件(该文件通常是系统最后启动的脚本)，在文件最末加上一行“xinit”或“startx”，可以在开机启动后直接进入X－Window。

<strong>2．登录时自动运行程序</strong>

用 户登录时，bash首先自动执行系统管理员建立的全局登录script ：/ect/profile。然后bash在用户起始目录下按顺序查找三个特殊文件中的一个：/.bash_profile、/.bash_login、 /.profile，但只执行最先找到的一个。
因此，只需根据实际需要在上述文件中加入命令就可以实现用户登录时自动运行某些程序（类似于DOS下的Autoexec.bat）。

<strong>3．退出登录时自动运行程序</strong>

退出登录时，bash自动执行个人的退出登录脚本/.bash_logout。例如，在/.bash_logout中加入命令“tar －cvzf c.source.tgz ＊.c”，则在每次退出登录时自动执行 “tar” 命令备份 ＊.c 文件。

<strong>4．定期自动运行程序</strong>

Linux有一个称为crond的守护程序，主要功能是周期性地检查 /var/spool/cron目录下的一组命令文件的内容，并在设定的时间执行这些文件中的命令。用户可以通过crontab 命令来建立、修改、删除这些命令文件。

例如，建立文件crondFile，内容为“00 9 23 Jan ＊ HappyBirthday”，运行“crontab cronFile”命令后，每当元月23日上午9:00系统自动执行“HappyBirthday”的程序（“＊”表示不管当天是星期几）。

<strong>5．定时自动运行程序一次</strong>

定时执行命令at 与crond 类似（但它只执行一次）：命令在给定的时间执行，但不自动重复。at命令的一般格式为：at [ －f file ] time ，在指定的时间执行file文件中所给出的所有命令。也可直接从键盘输入命令：

＄ at 12:00
at&gt;mailto Roger －s ″Have a lunch″ &lt; plan.txt
at&gt;Ctr－D
Job 1 at 2000－11－09 12:00
2000－11－09 12:00时候自动发一标题为“Have a lunch”，内容为plan.txt文件内容的邮件给Roger。?9 12:00
2000－11－09 12:00时候自动发一标题为“Have a lunch”，内容为plan.txt文件内容的邮件给Roger。er。ger。er。

<strong>Ubuntu 开机自动挂载windows分区</strong>

要挂载NTFS格式分区，需要NTFS-3g这个软件。它短小精悍，而且功能强大。
NTFS-3g是一个开源软件，它支持在Windows下面读写NTFS格式的分区。它非常的快速，同时也很安全。它支持Windows 2000、XP和2003，并且支持所有的符合POSIX标准的磁盘操作。

<strong>首先要编辑sources.list</strong>
<strong>#sudo gedit /etc/apt/sources.list</strong>

<strong>Ubuntu Drapper添加：</strong>
deb http://givre.cabspace.com/ubuntu/ dapper main main-all
deb http://ntfs-3g.sitesweetsite.info/ubuntu/ dapper main main-all
deb http://flomertens.keo.in/ubuntu/ dapper main main-all

<strong>Ubuntu Edgy添加：</strong>
deb http://givre.cabspace.com/ubuntu/ edgy main
deb http://ntfs-3g.sitesweetsite.info/ubuntu/ edgy main
deb http://flomertens.keo.in/ubuntu/ edgy main

<strong>同时必须导入GPG-Key，可以这样：</strong>
<strong>#wget http://flomertens.keo.in/ubuntu/givre_key.asc -O- | sudo apt-key add -
#wget http://givre.cabspace.com/ubuntu/givre_key.asc -O- | sudo apt-key add -
</strong>
<strong>现在更新一下源：</strong>
<strong>#sudo aptitude update</strong>

<strong>正式安装</strong>

在“终端”下面运行：
<strong>#sudo apt-get install ntfs-3g</strong>

<strong>配置NTFS-3g</strong>

首先看一些硬盘分区的分区类型
<strong>#sudo fdisk -l</strong>

现在就可以修改 /etc/fstab，来让Ubuntu启动的时候自动挂载NTFS分区了。但是首先请备份一下这个文件：
<strong>#sudo cp /etc/fstab /etc/fstab.bak</strong>

建立挂载点，譬如挂载在 /media/windows 下面
<strong>#sudo mkdir /media/windows</strong>

现在可以在 /etc/fstab 的后面添加
/dev/hda1 /media/ ntfs-3g defaults,locale=zh_CN.utf8 0 0
根据自己的情况进行修改。

<strong>一些示例</strong>

挂载 /dev/hda3
添加 /dev/hda3 /media/windows ntfs-3g ro,locale=zh_CN.utf8,uid=1000 0 0

<strong>关于自己的locale</strong>

可以用下面的命令查看所有的locale
<strong>#locale -a</strong>

如果不想重新启动，就可以
<strong>#sudo umount -a
#sudo mount -a
</strong>
<strong>最后一个挂载FAT分区的命令</strong>
<strong>#sudo mount /dev/hda3 /media/windows/ -t vfat -o iocharset=utf8,umask=000</strong>

<strong>当然可以在/etc/fstab里面添加</strong>
/dev/hda3 /media/windows vfat iocharset=utf8,umask=000 0 0

<strong>Openfire随着Ubuntu自动启动</strong>

openfire缺省情况下，是不随机启动的。为了解决每次都要手工启动的麻烦，我编写了一个脚本，放在/etc/init.d目录里面
<strong>#sudo vim /etc/init.d/openfire</strong>
内容如下：

#!/bin/sh

openfire_start(){
/etc/openfire/bin/openfire start
}

openfire_stop(){
/etc/openfire/bin/openfire stop
}

case $1 in
start)
openfire_start
;;
stop)
openfrie_stop
;;
*)
echo ‘Usage:openfire start|stop’
;;
esac

</div>
</div>
