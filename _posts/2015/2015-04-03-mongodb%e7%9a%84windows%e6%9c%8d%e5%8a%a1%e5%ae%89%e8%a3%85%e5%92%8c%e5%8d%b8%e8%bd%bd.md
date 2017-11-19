---
layout: post
title: Mongodb的windows服务安装和卸载
date: 2015-04-03 10:08
author: admin
comments: true
categories: []
---
不用 InstallUtil.exe，直接用mongod.exe做就可以：

安装：mongod --dbpath "C:\mongodb\db" --logpath "C:\mongodb\log.txt" --install --serviceName "MongoDB"

卸载：mongod.exe --remove --serviceName "MongoDB"
