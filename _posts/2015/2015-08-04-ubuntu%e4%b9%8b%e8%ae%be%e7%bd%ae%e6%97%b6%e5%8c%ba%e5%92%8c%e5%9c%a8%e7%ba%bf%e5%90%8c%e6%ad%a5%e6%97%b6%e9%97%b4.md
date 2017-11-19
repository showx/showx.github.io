---
layout: post
title: ubuntu之设置时区和在线同步时间
date: 2015-08-04 11:24
author: admin
comments: true
categories: []
---

Linux默认情况下使用UTC格式作为标准时间格式，如果在Linux下运行程序，且在程序中指定了与系统不一样的时区的时候，可能会造成时间错误。如果是Ubuntu的桌面版，则可以直接在图形模式下修改时区信息，但如果是在Server版呢，则需要通过tzconfig来修改时区信息了。使用方式(如将时区设置成Asia/Chongqing)：
 
sudo tzconfig   #如果命令不存在请使用:dpkg-reconfigure tzdata
然后按照提示选择 Asia对应的序号，选完后会显示一堆新的提示—输入城市名，如Shanghai或Chongqing，最后再用 sudo date -s “” 来修改本地时间。
按照提示进行选择时区，然后：
sudo cp /usr/share/zoneinfo/Asia/Chongqing /etc/localtime
上面的命令是防止系统重启后时区改变。
 
     网上同步时间
    1.安装ntpdate工具
    # sudo apt-get install ntpdate
    2.设置系统时间与网络时间同步
    # ntpdate cn.pool.ntp.org
    3.将系统时间写入硬件时间
    # hwclock -w
 
这里公布2个NTP服务器地址:
cn.pool.ntp.org
ntp.api.bz
