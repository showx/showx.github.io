---
layout: post
title: PHP 使用header函数设置HTTP头的示例方法 表头 （xlsx下载）
date: 2014-12-09 09:47
author: admin
comments: true
categories: []
---
<blockquote>//定义编码

header( 'Content-Type:text/html;charset=utf-8 ');

&nbsp;

//Atom

header('Content-type: application/atom+xml');

&nbsp;

//CSS

header('Content-type: text/css');

&nbsp;

//Javascript

header('Content-type: text/javascript');

&nbsp;

//JPEG Image

header('Content-type: image/jpeg');

&nbsp;

//JSON

header('Content-type: application/json');

&nbsp;

//PDF

header('Content-type: application/pdf');

&nbsp;

//RSS

header('Content-Type: application/rss+xml; charset=ISO-8859-1');

&nbsp;

//Text (Plain)

header('Content-type: text/plain');

&nbsp;

//XML

header('Content-type: text/xml');

&nbsp;

// ok

header('HTTP/1.1 200 OK');

&nbsp;

//设置一个404头:

header('HTTP/1.1 404 Not Found');

&nbsp;

//设置地址被永久的重定向

header('HTTP/1.1 301 Moved Permanently');

&nbsp;

//转到一个新地址

header('Location: http://www.example.org/');

&nbsp;

//文件延迟转向:

header('Refresh: 10; url=http://www.example.org/');

print 'You will be redirected in 10 seconds';

&nbsp;

//当然，也可以使用html语法实现

//

// override X-Powered-By: PHP:

header('X-Powered-By: PHP/4.4.0');

header('X-Powered-By: Brain/0.6b');

&nbsp;

//文档语言

header('Content-language: en');

&nbsp;

//告诉浏览器最后一次修改时间

$time = time() - 60; // or filemtime($fn), etc

header('Last-Modified: '.gmdate('D, d M Y H:i:s', $time).' GMT');

&nbsp;

//告诉浏览器文档内容没有发生改变

header('HTTP/1.1 304 Not Modified');

&nbsp;

//设置内容长度

header('Content-Length: 1234');

&nbsp;

//设置为一个下载类型

header('Content-Type: application/octet-stream');

header('Content-Disposition: attachment; filename="example.zip"');

header('Content-Transfer-Encoding: binary');

// load the file to send:

readfile('example.zip');

&nbsp;

// 对当前文档禁用缓存

header('Cache-Control: no-cache, no-store, max-age=0, must-revalidate');

header('Expires: Mon, 26 Jul 1997 05:00:00 GMT'); // Date in the past

header('Pragma: no-cache');

&nbsp;

//设置内容类型:

header('Content-Type: text/html; charset=iso-8859-1');

header('Content-Type: text/html; charset=utf-8');

header('Content-Type: text/plain'); //纯文本格式

header('Content-Type: image/jpeg'); //JPG***

header('Content-Type: application/zip'); // ZIP文件

header('Content-Type: application/pdf'); // PDF文件

header('Content-Type: audio/mpeg'); // 音频文件

header('Content-Type: application/x-shockw**e-flash'); //Flash动画

&nbsp;

//显示登陆对话框

header('HTTP/1.1 401 Unauthorized');

header('WWW-Authenticate: Basic realm="Top Secret"');

print 'Text that will be displayed if the user hits cancel or ';

print 'enters wrong login data';

======================

&nbsp;

$filename = rtrim($_SERVER['DOCUMENT_ROOT'],'/').'/app/files/payment_status.csv';

header('Content-Disposition: attachment; filename=payment_status.xlsx');

header('Content-Type: application/vnd.openxmlformats-officedocument.spreadsheetml.sheet');

header('Content-Length: ' . filesize($filename));

header('Content-Transfer-Encoding: binary');

header('Cache-Control: must-revalidate');

header('Pragma: public');

readfile($filename);</blockquote>
