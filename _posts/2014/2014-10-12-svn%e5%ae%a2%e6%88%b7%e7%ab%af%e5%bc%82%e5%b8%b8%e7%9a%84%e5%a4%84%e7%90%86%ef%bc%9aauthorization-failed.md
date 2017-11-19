---
layout: post
title: svn客户端异常的处理：authorization failed
date: 2014-10-12 11:22
author: admin
comments: true
categories: []
---

出现该问题基本都是三个配置文件的问题，下面把这个文件列出来。

svnserve.conf:
[general]
anon-access = read
auth-access = write
password-db = passwd
authz-db = authz

passwd:
[users]
harry = harryssecret

authz:
[groups]
[/]
harry = rw

出现authorization failed异常，一般都是authz文件里，用户组或者用户权限没有配置好，只要设置[/]就可以，代表根目录下所有的资源，如果要限定资源，可以加上子目录即可。
