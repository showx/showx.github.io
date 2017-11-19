---
layout: post
title: evercookie 简介信息
date: 2015-09-03 15:33
author: admin
comments: true
categories: []
---
evercookie是一个用于尽量将Cookies持久化在浏览器中的JavaScript API。目的是为了当用户删除标准Cookies，Flash Cookies等之后还能识别客户端。evercookie采用在本地浏览器中可用的各种不同存储机制来存储cookie数据。当evercookie发现用某种机制存储的cookie被数据将删除之后，它将利用其它机制创建的cookie数据来重新创建，让用户几乎不可能删除cookie。当前evercookie支持的存储机制包括：
     - Standard HTTP Cookies
     - Local Shared Objects (Flash Cookies)
     - Storing cookies in RGB values of auto-generated, force-cached 
        PNGs using HTML5 Canvas tag to read pixels (cookies) back out
     - Storing cookies in and reading out Web History
     - Storing cookies in HTTP ETags
     - Internet Explorer userData storage
     - HTML5 Session Storage
     - HTML5 Local Storage
     - HTML5 Global Storage
     - HTML5 Database Storage via SQLite
