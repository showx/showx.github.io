---
layout: post
title: php,curl获取header信息
date: 2014-02-11 20:37
author: admin
comments: true
categories: []
---
function get_header($url){
    $ch  = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_HEADER, true);
    curl_setopt($ch, CURLOPT_NOBODY,true);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER,true);
    //curl_setopt($ch, CURLOPT_FOLLOWLOCATION,true);
    curl_setopt($ch, CURLOPT_AUTOREFERER,true);
    curl_setopt($ch, CURLOPT_TIMEOUT,30);
    curl_setopt($ch, CURLOPT_HTTPHEADER, array(
    'Accept: */*',
    'User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1)',
    'Connection: Keep-Alive'));
    $header = curl_exec($ch);
    return $header;
}
