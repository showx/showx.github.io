---
layout: post
title: imagecreatefromstring获取宽高
date: 2015-08-13 17:45
author: admin
comments: true
categories: []
---
平常使用imagecreatefromjpg($url);

使用getimagesize()函数可获取宽高，

但使用imagecreatefromstring要使用imagesx和imagesy才行

size = 150; // new image width

&nbsp;

$src = imagecreatefromstring($data);

$width = imagesx($src);

$height = imagesy($src);
