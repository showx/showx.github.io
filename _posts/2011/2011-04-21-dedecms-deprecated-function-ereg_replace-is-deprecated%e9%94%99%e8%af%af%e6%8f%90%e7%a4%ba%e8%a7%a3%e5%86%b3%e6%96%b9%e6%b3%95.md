---
layout: post
title: DEDECMS Deprecated: Function ereg_replace() is deprecated错误提示解决方法
date: 2011-04-21 12:12
author: admin
comments: true
categories: []
---
使用最新版的wamp，内置的是5.3版本的php，安装dedecms5.5以后，出现了
Deprecated: Function ereg_replace() is deprecated in D:\wamp\www\dedecms\dede\config.php on line 2
 
于是百度一下，果然有前辈已经遇到了这个问题，原来ereg_replace是php5.3中废弃的标签，不推进使用了。解决方法很简单，就是将dede\config.php文件的第二行替换成
define(’DEDEADMIN’, preg_replace(”/[\/\\\\]{1,}/”, ‘/’, dirname(__FILE__) ) );
这样就不会报错了。遇到同样问题的朋友们不妨试一试。
