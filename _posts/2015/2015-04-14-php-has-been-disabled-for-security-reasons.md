---
layout: post
title: php has been disabled for security reasons
date: 2015-04-14 17:40
author: admin
comments: true
categories: []
---
scandir() has been disabled for security reasons

出现这种情况，一般修改php.ini文件

一般禁用

disable_functions = exec,system,ini_alter,readlink,symlink,leak,proc_open,popepassthru,chroot,scandir,chgrp,chown,escapeshellcmd,escapeshellarg,proc_get_status,passth
ru,popen
