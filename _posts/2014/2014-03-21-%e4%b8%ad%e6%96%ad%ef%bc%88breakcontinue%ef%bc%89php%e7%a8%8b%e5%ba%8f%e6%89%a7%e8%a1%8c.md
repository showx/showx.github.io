---
layout: post
title: 中断（break/continue）PHP程序执行  
date: 2014-03-21 10:42
author: admin
comments: true
categories: []
---
之前的程序中，在服务器 error_log 中一直有这么一个错误提示：
PHP Fatal error: <wbr /> Cannot break/continue 1 level in /home/filename.php on line 160
但程序还是可以继续执行下去。
经查阅资料，有这么一说法：
当不在 LOOP 或 SELECT 逻辑条件中时，请不要用 break/continue 来中断程序的执行。请试着用 return/exit(); 来替换break/continue 语句。
