---
layout: post
title: radtest指令格式 
date: 2014-10-17 15:55
author: admin
comments: true
categories: []
---
可以利用 radtest 這個指令，其格式為 radtest [帳號] [密碼] [認證位址:認證埠] [NAS port] [secret key]：
[root@localhost ~]# radtest user pass localhost 0 testing123
Sending Access-Request of id 75 to 127.0.0.1 port 1812
        User-Name = "user"
        User-Password = "pass"
        NAS-IP-Address = 255.255.255.255
        NAS-Port = 0
rad_recv: Access-Accept packet from host 127.0.0.1:1812, id=75, length=57
        Reply-Message = "Hello World!"
 
若收到 Access-Accep 訊息則表示認證成功，即完成簡單的 FreeRADIUS 設定。若發生問題可利用 radtest -X 來debug。
其實 FreeRADIUS 還有其他很多進階應用，例如設定黑名單來 block 使用者或是將帳號儲存在其他機器，並且使用SQL server、MySQL 或 LDAP ...等，將會在未來的幾篇說明。
