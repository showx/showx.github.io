---
layout: post
title: Web: html_entity_decode、空格&nbsp以及乱码
date: 2014-10-09 14:47
author: admin
comments: true
categories: []
---
普通ASCII码空格为32，但是浏览器会对普通空格进行自动归并，

也就是如果你输入10个0x20的空格在HTML页面里面，可能会被合并成一个空格。

如果想要一致的呈现多个空格，就要用到&nbsp，这个空格的编码为160，为西欧ISO-8859-1编码标准。



为了让经过HTML编码的内容还原为原来的文本字符，可以使用html_entity_decode方法，

但这样问题就来了，通常HTML编码内容为UTF8格式的，html_entity_decode在浏览器UTF8编码环境下会把

&nbsp转为一个黑色四方形状的乱码。只有切换为ISO-8859-1才能正确显示为空格。



所以在使用html_entity_decode之前，需要先把&nbsp替换掉（str_replace），这样就可以避免乱码问题。
