---
layout: post
title: linux下设置ssh无密码登录
date: 2013-08-26 22:34
author: admin
comments: true
categories: []
---

ssh配置　　
主机A：10.0.5.199
主机B：10.0.5.198 
需要配置主机A无密码登录主机A，主机B
先确保所有主机的防火墙处于关闭状态。
在主机A上执行如下：
　1.　$cd ~/.ssh
　2.　$ssh-keygen -t rsa  --------------------然后一直按回车键，就会按照默认的选项将生成的密钥保存在.ssh/id_rsa文件中。
　3.　$cp id_rsa.pub authorized_keys 
         这步完成后，正常情况下就可以无密码登录本机了，即ssh localhost，无需输入密码。
　4.　$scp authorized_keys summer@10.0.5.198:/home/summer/.ssh   ------把刚刚产生的authorized_keys文件拷一份到主机B上.　　
　5.　$chmod 600 authorized_keys       
　　   进入主机B的.ssh目录，改变authorized_keys文件的许可权限。
　　 (4和5可以合成一步，执行:  $ssh-copy-id -i summer@10.0.5.198 )
 
正常情况下上面几步执行完成后，从主机A所在机器向主机A、主机B所在机器发起ssh连接，只有在第一次登录时需要输入密码，以后则不需要。
 
可能遇到的问题：
1.进行ssh登录时，出现：”Agent admitted failure to sign using the key“ .
   执行： $ssh-add
   强行将私钥 加进来。
2.如果无任何错误提示，可以输密码登录，但就是不能无密码登录，在被连接的主机上（如A向B发起ssh连接，则在B上）执行以下几步：
　　$chmod o-w ~/
   $chmod 700 ~/.ssh
   $chmod 600 ~/.ssh/authorized_keys

3.如果执行了第2步，还是不能无密码登录，再试试下面几个
　　$ps -Af | grep agent 
        检查ssh代理是否开启，如果有开启的话，kill掉该代理，然后执行下面，重新打开一个ssh代理，如果没有开启，直接执行下面：
       $ssh-agent
　　还是不行的话，执行下面，重启一下ssh服务
       $sudo service sshd restart
4. 执行ssh-add时提示“Could not open a connection to your authenticationh agent”而失败
执行： ssh-agent bash 
