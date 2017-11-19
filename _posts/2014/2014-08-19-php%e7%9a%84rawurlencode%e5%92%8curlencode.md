---
layout: post
title: PHP的rawurlencode和urlencode 
date: 2014-08-19 10:51
author: admin
comments: true
categories: []
---
问题:2个函数都是针对字符串转义使其适合做文件名。该用哪个？哪个更标准？
结论:
rawurlencode遵守是94年国际标准备忘录RFC 1738，
urlencode实现的是传统做法，和上者的主要区别是对空格的转义是'+'而不是'%20'
javascript的encodeURL也是94年标准，
而javascript的escape是另一种用"%xxx"标记unicode编码的方法。
推荐在PHP中使用用rawurlencode。弃用urlencode
 
样例
source:
超级无敌的人sadha sajdh数据样本sdls fhejrthcxzb.file.jpeg

PHP urlencode:
%E8%B6%85%E7%BA%A7%E6%97%A0%E6%95%8C%E7%9A%84%E4%BA%BAsadha+sajdh%E6%95%B0%E6%8D%AE%E6%A0%B7%E6%9C%ACsdls+fhejrthcxzb.file.jpeg

PHP rawurlencode:
%E8%B6%85%E7%BA%A7%E6%97%A0%E6%95%8C%E7%9A%84%E4%BA%BAsadha%20sajdh%E6%95%B0%E6%8D%AE%E6%A0%B7%E6%9C%ACsdls%20fhejrthcxzb.file.jpeg

Javascript encodeURI:
%E8%B6%85%E7%BA%A7%E6%97%A0%E6%95%8C%E7%9A%84%E4%BA%BAsadha%20sajdh%E6%95%B0%E6%8D%AE%E6%A0%B7%E6%9C%ACsdls%20fhejrthcxzb.file.jpeg

Javascript escape:
%u8D85%u7EA7%u65E0%u654C%u7684%u4EBAsadha%20sajdh%u6570%u636E%u6837%u672Csdls%20fhejrt


昨天看UCHome源码的时候，发现有些地方用urlencode，有些地方用rawurlencode。由于对这两个方法的差异不是很清楚，特意写了一段代码来测试。

 

请将下面的代码保存到一个PHP文件中：

[php] view plaincopy
<?php  
test_encode('http://www.baidu.com?a=search&k=eclipse');  
test_encode(':/?= &#');  
test_encode('中文');  
function test_encode($s)  
{  
  echo "<b>urlencode('$s')</b> = [<b>";  
  var_dump(urlencode($s));    
  echo "</b>]<br/>";  
  echo "<b>rawurlencode('$s')</b> = [<b>";  
  var_dump(rawurlencode($s));    
  echo "</b>]<br/>";  
}  
上面代码执行结果如下：

[php] view plaincopy
urlencode('http://www.baidu.com?a=search&k=eclipse') = [string(53) "http%3A%2F%2Fwww.baidu.com%3Fa%3Dsearch%26k%3Declipse" ]  
rawurlencode('http://www.baidu.com?a=search&k=eclipse') = [string(53) "http%3A%2F%2Fwww.baidu.com%3Fa%3Dsearch%26k%3Declipse" ]  
urlencode(':/?= &#') = [string(19) "%3A%2F%3F%3D+%26%23" ]  
rawurlencode(':/?= &#') = [string(21) "%3A%2F%3F%3D%20%26%23" ]  
urlencode('中文') = [string(18) "%E4%B8%AD%E6%96%87" ]  
rawurlencode('中文') = [string(18) "%E4%B8%AD%E6%96%87" ]  
 

从上面的执行结果可以看出，urlencode和rawurlencode两个方法在处理字母数字，特殊符号，中文的时候结果都是一样的，唯一的不同是对空格的处理，urlencode处理成“+”，rawurlencode处理成“%20”。

 

 

 

看看PHP Manual对两个函数的说明：

urlencode：返回字符串，此字符串中除了 -_. 之外的所有非字母数字字符都将被替换成百分号（%）后跟两位十六进制数，空格则编码为加号（+）。此编码与 WWW 表单 POST 数据的编码方式是一样的，同时与 application/x-www-form-urlencoded 的媒体类型编码方式一样。由于历史原因，此编码在将空格编码为加号（+）方面与 RFC1738 编码（参见 rawurlencode()）不同。

rawurlencode：返回字符串，此字符串中除了 -_. 之外的所有非字母数字字符都将被替换成百分号（%）后跟两位十六进制数。这是在 RFC 1738 中描述的编码，是为了保护原义字符以免其被解释为特殊的 URL 定界符，同时保护 URL 格式以免其被传输媒体（像一些邮件系统）使用字符转换时弄乱。
