---
layout: post
title: python UnicodeDecodeError: 'ascii' codec can't decode byte 0xe6 in position 0: ordinal
date: 2015-08-21 09:12
author: admin
comments: true
categories: []
---
<u><b>UnicodeDecodeError: 'ascii' codec can't decode byte 0xe6 in position 0: ordinal</b></u>

&nbsp;

python 默认的编码是ascii，只能标识128个字符，要使用其他编码需要转换，比如：‘hhhhhh'.encode('utf8')

&nbsp;

错误信息：'ascii' codec can't decode byte 0xb8 in position 1: ordinal not in range(128)
