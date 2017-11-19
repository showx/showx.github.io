---
layout: post
title: proc_open php多进程利器
date: 2014-12-16 16:07
author: admin
comments: true
categories: []
---
popen——php多进程利器
2012-11-16 15:26 1334人阅读 评论(0) 收藏 举报
偷笑标题有点儿夸张，

 

我（们，本来想用们的，还会去掉了）运行系统命令常常用exec，system之类的，

但是今天发现了proc_open和popen，proc_open自称比popen多一些功能，确实，proc_open有很多功能，可以与程序交互，

——但是，他是同步的，就是说一个程序没有运行结束，不能运行下一个！就没办法异步多进程了。

——但，popen是异步的

 

上代码：

pro.php

[php] view plaincopy
<?php  
$process = array();  
for($i=0;$i<5;$i++)  
{  
    echo $i.' opening ... '.chr(10);  
    $process[$i] = popen('php '.dirname(__FILE__).'/run.php', 'r');  
    sleep(1);  
}  
  
echo 'OK>>'.chr(10);  
sleep(3);  
for($i=0;$i<5;$i++)  
{  
    $read = fread($process[$i], 64);  
    echo $read;  
    pclose($process[$i]);  
    echo $i.' closed'.chr(10);  
    sleep(1);  
}  
run.php

[php] view plaincopy
<?php  
while(1)  
{  
    $echo =getmypid() .'->'. date("YmHis").chr(10);  
    echo $echo ;  
    system('echo '.$echo.'> logp');  
    sleep(5);  
}  

执行php pro.php
