---
layout: post
title: shell 学习笔记
date: 2014-12-03 22:47
author: admin
comments: true
categories: []
---
  ($?) 为0的退出码

检查文件是否排序过
＃!/bin/bash
sort -C filename;
if [ $? -eq 0]; then
   echo Sort;
else
   echo nosort;
fi

file_jpg = "sample.jpg"
name=${file_jpg%.*}   # %.* 是从左到右 %%.*从右向左 #*.}从右到左  ##*.
echo $name #sample

-print 输出 
 
xargs 

sort

uniq

find -type f -name "*.mp3" 

-exe {}
{}代表匹配的

aspell拼写检查

md5sum加密系列

spawn 指定需要自动化哪一个命令
expect 等待的消息
send 发送的消息
expect eof 交互结束


split 分割文件
csplit

tr 替换


