---
layout: post
title: mysql order by | varchar
date: 2015-03-13 16:51
author: admin
comments: true
categories: []
---
msyql order by 
varchar 

flag  和 sum 是varchar还是int型的
如果是int型的那么：1、order by finish，2、order by sum， 3、order by id desc
如果是varchar型的那么：1、order by to_number搜索(finish)，2、order by to_number(sum)， 3、order by id desc
