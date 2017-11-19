---
layout: post
title: php curlopt_cookie引发406
date: 2015-03-24 17:27
author: admin
comments: true
categories: []
---
curl发送cookie请求引发的问题 
curlopt_cookie引发406

$strCookie = 'PHPSESSID=' . session_name() . '; path=/';
curl_setopt( $curl, CURLOPT_COOKIE, $strCookie );
要加上path=/就没事..
