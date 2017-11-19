---
layout: post
title: php类库PHP QR Code 二维码  
date: 2014-06-06 09:48
author: admin
comments: true
categories: []
---
地址：http://phpqrcode.sourceforge.net/
下载：http://sourceforge.net/projects/phpqrcode/


只需这样应用就行了

<?
include "./phpqrcode/phpqrcode.php";
$value=‘http://www.sh-ow.com’;
$errorCorrectionLevel = 'L';
$matrixPointSize = 4;
QRcode::png($value, false, $errorCorrectionLevel, $matrixPointSize);
exit;
?>
