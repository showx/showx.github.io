---
layout: post
title: svn export –导出 
date: 2013-10-15 17:56
author: admin
comments: true
categories: []
---
export: 产生一个无版本控制的目录树副本。
用法: 1、export [-r REV] URL[@PEGREV] [PATH]
2、export [-r REV] PATH1[@PEGREV] [PATH2]
1、从 URL 指定的版本库，导出一个干净的目录树到 PATH。如果有指定
REV 的话，内容即为该版本的，否则就是 HEAD 版本。如果 PATH
被省略的话，URL的最后部份会被用来当成本地的目录名称。
2、在工作副本中，从指定的 PATH1 导出一个干净的目录树到 PATH2。如果
有指定 REV 的话，会从指定的版本导出，否则从工作副本导出。如果
PATH2 被省略的话，PATH1 的最后部份会被用来当成本地的目录名称。
如果没有指定 REV 的话，所有的本地修改都保留，但是未纳入版本控制
的文件不会被复制。
如果指定了 PEGREV ，将从指定的版本本开始查找。
有效选项:
-r [--revision] ARG : ARG (一些命令也接受ARG1:ARG2范围)
版本参数可以是如下之一:
NUMBER 版本号
'{' DATE '}' 在指定时间以后的版本
'HEAD' 版本库中的最新版本
'BASE' 工作副本的基线版本
'COMMITTED' 最后提交或基线之前
'PREV' COMMITTED的前一版本
-q [--quiet] : 不打印信息，或只打印概要信息
-N [--non-recursive] : 过时；尝试 --depth=files 或 --depth=immediates
--depth ARG : 受深度参数 ARG(“empty”，“files”，“immediates”，或“infinity”) 约束的操作
--force : 强制操作运行
--native-eol ARG : 使用非标准的 EOL 标记
系统中立的文件标记 svn:eol-style 属性取值为 “native”。
ARG 可以是以下之一“LF”，“CR”，“CRLF”
--ignore-externals : 忽略外部项目
全局选项:
--username ARG : 指定用户名称 ARG
--password ARG : 指定密码 ARG
--no-auth-cache : 不要缓存用户认证令牌
--non-interactive : 不要交互提示
--trust-server-cert : 不提示的接受未知的 SSL 服务器证书(只用于选项 “--non-interactive”)
--config-dir ARG : 从目录 ARG 读取用户配置文件
--config-option ARG : 以下属格式设置用户配置选项：
FILE:SECTION:OPTION=[VALUE]
例如：
servers:global:http-library=serf

常用操作
1.从你的工作拷贝导出（不会打印每一个文件和目录）：
$ svn export a-wc my-export
2.从版本库导出目录（打印所有的文件和目录）：
$ svn export file:///tmp/repos my-export

&nbsp;

&nbsp;

最简单 svn export ./  ../新文件夹名称
