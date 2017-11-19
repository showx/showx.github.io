---
layout: post
title: PHP的file_get_contents获取远程页面乱码的问题
date: 2014-01-22 09:10
author: admin
comments: true
categories: []
---
PHP的file_get_contents获取远程页面内容，如果是gzip编码过的，返回的字符串就是编码后的乱码


1、解决方法，找个ungzip的函数来转换下
2、给你的url加个前缀，这样调用
$content = file_get_contents("compress.zlib://".$url);
无论页面是否经过gzip压缩，上述代码都可以正常工作！


使用curl模块同样可解决问题
function curl_get($url, $gzip=false){
        $curl = curl_init($url);
        curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
        curl_setopt($curl, CURLOPT_CONNECTTIMEOUT, 10);
        if($gzip) curl_setopt($curl, CURLOPT_ENCODING, "gzip"); // 关键在这里
        $content = curl_exec($curl);
        curl_close($curl);
        return $content;
}

相关信息
http://bbs.phpchina.com/forum.php?mod=viewthread&tid=229436
