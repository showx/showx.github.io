---
layout: post
title: [转载]SVN错误中文对照
date: 2013-08-23 16:39
author: admin
comments: true
categories: []
---
#
# Simplified Chinese translation for subversion package
# This file is distributed under the same license as the subversion package.
#
# Update to new pot:
#    msgmerge --update zh_CN.po subversion.pot
#
# Check translation:
#    msgfmt --statistics -c -o zh_CN.mo zh_CN.po
#
# Please format your translation before commit:
#    msgmerge --sort-by-file --no-wrap -o zh_CN_new.po zh_CN.po subversion.pot
#    mv -f zh_CN_new.po zh_CN.po
#
# Dictionary:
# BASE revision     基础版本
# blame             追溯
# checkout          检出
# default           默认
# HEAD revision     最新版本
# overlay           重载
# remove            删除
# rename            改名
# repository        版本库
# revert            恢复
# revision          版本
# Subversion book   Subversion 手册
# undo              撤销
# unified diff      标准差异
# unversioned       未版本控制
# versioned         已版本控制
# working copy      工作副本
#
msgid ""
msgstr ""
"Project-Id-Version: subversion 1.5.0n"
"Report-Msgid-Bugs-To: dev@subversion.tigris.orgn"
"POT-Creation-Date: 2008-01-16 12:56+0800n"
"PO-Revision-Date: 2008-01-16 13:47+0800n"
"Last-Translator: Subversion Developers <dev@subversion.tigris.org>n"
"Language-Team: Simplified Chinese <dev@subversion.tigris.org>n"
"MIME-Version: 1.0n"
"Content-Type: text/plain; charset=UTF-8n"
"Content-Transfer-Encoding: 8bitn"
#: ../include/svn_error_codes.h:156
msgid "Bad parent pool passed to svn_make_pool()"
msgstr "无效的父内存池传递到 svn_make_pool()"
#: ../include/svn_error_codes.h:160
msgid "Bogus filename"
msgstr "非法的文件名"
#: ../include/svn_error_codes.h:164
msgid "Bogus URL"
msgstr "非法 URL"
#: ../include/svn_error_codes.h:168
msgid "Bogus date"
msgstr "非法日期"
#: ../include/svn_error_codes.h:172
msgid "Bogus mime-type"
msgstr "非法 mime-type"
#: ../include/svn_error_codes.h:182
msgid "Wrong or unexpected property value"
msgstr "错误或不期望的属性值"
#: ../include/svn_error_codes.h:186
msgid "Version file format not correct"
msgstr "版本文件格式不正确"
#: ../include/svn_error_codes.h:190
msgid "Path is not an immediate child of the specified directory"
msgstr "路径不是指定目录的直接子孙"
#: ../include/svn_error_codes.h:194
msgid "Bogus UUID"
msgstr "非法 UUID"
#: ../include/svn_error_codes.h:200
msgid "No such XML tag attribute"
msgstr "没有这种XML标签属性"
#: ../include/svn_error_codes.h:204
msgid "<delta-pkg> is missing ancestry"
msgstr "<delta-pkg>没有祖先"
#: ../include/svn_error_codes.h:208
msgid "Unrecognized binary data encoding; can't decode"
msgstr "无法识别的二进制数据编码: 无法解码"
#: ../include/svn_error_codes.h:212
msgid "XML data was not well-formed"
msgstr "XML 数据语法错误"
#: ../include/svn_error_codes.h:216
msgid "Data cannot be safely XML-escaped"
msgstr "XML 数据不能正确解码"
#: ../include/svn_error_codes.h:222
msgid "Inconsistent line ending style"
msgstr "不一致的行结束样式"
#: ../include/svn_error_codes.h:226
msgid "Unrecognized line ending style"
msgstr "无法识别的行结束样式"
#: ../include/svn_error_codes.h:231
msgid "Line endings other than expected"
msgstr "行意外结束"
#: ../include/svn_error_codes.h:235
msgid "Ran out of unique names"
msgstr "唯一名称耗尽"
#: ../include/svn_error_codes.h:240
msgid "Framing error in pipe protocol"
msgstr "管道协议中帧错误"
#: ../include/svn_error_codes.h:245
msgid "Read error in pipe"
msgstr "管道读取错误"
#: ../include/svn_error_codes.h:249 ../libsvn_subr/cmdline.c:308
#: ../libsvn_subr/cmdline.c:325 ../svn/util.c:882
#, c-format
msgid "Write error"
msgstr "写入错误"
#: ../include/svn_error_codes.h:255
msgid "Unexpected EOF on stream"
msgstr "流意外结束"
#: ../include/svn_error_codes.h:259
msgid "Malformed stream data"
msgstr "非法流数据"
#: ../include/svn_error_codes.h:263
msgid "Unrecognized stream data"
msgstr "无法识别的流数据"
#: ../include/svn_error_codes.h:269
msgid "Unknown svn_node_kind"
msgstr "未知的 svn_node_kind"
#: ../include/svn_error_codes.h:273
msgid "Unexpected node kind found"
msgstr "发现意外节点种类"
#: ../include/svn_error_codes.h:279
msgid "Can't find an entry"
msgstr "无法找到条目"
#: ../include/svn_error_codes.h:285
msgid "Entry already exists"
msgstr "条目已存在"
#: ../include/svn_error_codes.h:289
msgid "Entry has no revision"
msgstr "条目没有版本"
#: ../include/svn_error_codes.h:293
msgid "Entry has no URL"
msgstr "入口没有 URL"
#: ../include/svn_error_codes.h:297
msgid "Entry has an invalid attribute"
msgstr "条目有无效属性"
#: ../include/svn_error_codes.h:303
msgid "Obstructed update"
msgstr "更新阻塞"
#: ../include/svn_error_codes.h:308
msgid "Mismatch popping the WC unwind stack"
msgstr "不匹配的弹出工作副本展开堆栈"
#: ../include/svn_error_codes.h:313
msgid "Attempt to pop empty WC unwind stack"
msgstr "试图弹出空的工作副本展开堆栈"
#: ../include/svn_error_codes.h:318
msgid "Attempt to unlock with non-empty unwind stack"
msgstr "试图解锁非空展开堆栈"
#: ../include/svn_error_codes.h:322
msgid "Attempted to lock an already-locked dir"
msgstr "试图锁定已加锁的目录"
#: ../include/svn_error_codes.h:326
msgid "Working copy not locked; this is probably a bug, please report"
msgstr "工作副本没有锁定；这可能是一个漏洞，请报告"
#: ../include/svn_error_codes.h:331
msgid "Invalid lock"
msgstr "无效锁"
#: ../include/svn_error_codes.h:335
msgid "Path is not a working copy directory"
msgstr "路径不是工作副本目录"
#: ../include/svn_error_codes.h:339
msgid "Path is not a working copy file"
msgstr "路径不是工作副本文件"
#: ../include/svn_error_codes.h:343
msgid "Problem running log"
msgstr "执行日志出错"
#: ../include/svn_error_codes.h:347
msgid "Can't find a working copy path"
msgstr "找不到工作副本路径"
#: ../include/svn_error_codes.h:351
msgid "Working copy is not up-to-date"
msgstr "工作副本没有更新到最新版本"
#: ../include/svn_error_codes.h:355
msgid "Left locally modified or unversioned files"
msgstr "保留本地修改或未纳入版本控制的文件"
#: ../include/svn_error_codes.h:359
msgid "Unmergeable scheduling requested on an entry"
msgstr "条目有无法合并的调度"
#: ../include/svn_error_codes.h:363
msgid "Found a working copy path"
msgstr "找到一个工作副本路径"
#: ../include/svn_error_codes.h:367
msgid "A conflict in the working copy obstructs the current operation"
msgstr "工作副本中的冲突阻止了当前操作"
#: ../include/svn_error_codes.h:371
msgid "Working copy is corrupt"
msgstr "工作副本已损坏"
#: ../include/svn_error_codes.h:375
msgid "Working copy text base is corrupt"
msgstr "工作副本的参考文件损坏"
#: ../include/svn_error_codes.h:379
msgid "Cannot change node kind"
msgstr "无法修改节点类型"
#: ../include/svn_error_codes.h:383
msgid "Invalid operation on the current working directory"
msgstr "操作对当前工作目录无效"
#: ../include/svn_error_codes.h:387
msgid "Problem on first log entry in a working copy"
msgstr "操作工作副本的第一个日志条目出错"
#: ../include/svn_error_codes.h:391
msgid "Unsupported working copy format"
msgstr "不支持此工作副本格式"
#: ../include/svn_error_codes.h:395
msgid "Path syntax not supported in this context"
msgstr "此上下文不支持路径语法"
#: ../include/svn_error_codes.h:400
msgid "Invalid schedule"
msgstr "无效的调度"
#: ../include/svn_error_codes.h:405
msgid "Invalid relocation"
msgstr "无效重定位"
#: ../include/svn_error_codes.h:410
msgid "Invalid switch"
msgstr "无效的切换"
#: ../include/svn_error_codes.h:415
msgid "Changelist doesn't match"
msgstr "修改列表不匹配"
#: ../include/svn_error_codes.h:420
msgid "Conflict resolution failed"
msgstr "解决冲突失败"
#: ../include/svn_error_codes.h:424
msgid "Failed to locate 'copyfrom' path in working copy"
msgstr "在工作副本中定位 “copyfrom” 的路径失败。"
#: ../include/svn_error_codes.h:429
msgid "Moving a path from one changelist to another"
msgstr "将路径从一个修改列表移到另一个"
#: ../include/svn_error_codes.h:436
msgid "General filesystem error"
msgstr "普通文件系统错误"
#: ../include/svn_error_codes.h:440
msgid "Error closing filesystem"
msgstr "关闭文件系统出错"
#: ../include/svn_error_codes.h:444
msgid "Filesystem is already open"
msgstr "文件系统已经打开"
#: ../include/svn_error_codes.h:448
msgid "Filesystem is not open"
msgstr "文件系统尚未打开"
#: ../include/svn_error_codes.h:452
msgid "Filesystem is corrupt"
msgstr "文件系统损坏"
#: ../include/svn_error_codes.h:456
msgid "Invalid filesystem path syntax"
msgstr "无效文件系统路径语法"
#: ../include/svn_error_codes.h:460
msgid "Invalid filesystem revision number"
msgstr "无效文件系统版本号"
#: ../include/svn_error_codes.h:464
msgid "Invalid filesystem transaction name"
msgstr "无效的文件系统事务名称"
#: ../include/svn_error_codes.h:468
msgid "Filesystem directory has no such entry"
msgstr "文件系统目录没有此条目"
#: ../include/svn_error_codes.h:472
msgid "Filesystem has no such representation"
msgstr "文件系统没有此修订版"
#: ../include/svn_error_codes.h:476
msgid "Filesystem has no such string"
msgstr "文件系统没有此字符串"
#: ../include/svn_error_codes.h:480
msgid "Filesystem has no such copy"
msgstr "文件系统此副本"
#: ../include/svn_error_codes.h:484
msgid "The specified transaction is not mutable"
msgstr "指定的事务不可改变"
#: ../include/svn_error_codes.h:488
msgid "Filesystem has no item"
msgstr "文件系统没有条目"
#: ../include/svn_error_codes.h:492
msgid "Filesystem has no such node-rev-id"
msgstr "文件系统没有此 node-rev-id"
#: ../include/svn_error_codes.h:496
msgid "String does not represent a node or node-rev-id"
msgstr "字符串不是节点或 node-rev-id"
#: ../include/svn_error_codes.h:500
msgid "Name does not refer to a filesystem directory"
msgstr "文件系统无此目录"
#: ../include/svn_error_codes.h:504
msgid "Name does not refer to a filesystem file"
msgstr "文件系统无此文件"
#: ../include/svn_error_codes.h:508
msgid "Name is not a single path component"
msgstr "名称不是单一路径"
#: ../include/svn_error_codes.h:512
msgid "Attempt to change immutable filesystem node"
msgstr "试图修改不变的文件系统节点"
#: ../include/svn_error_codes.h:516
msgid "Item already exists in filesystem"
msgstr "文件系统已有此条目"
#: ../include/svn_error_codes.h:520
msgid "Attempt to remove or recreate fs root dir"
msgstr "试图删除或重建文件系统根目录"
#: ../include/svn_error_codes.h:524
msgid "Object is not a transaction root"
msgstr "对象不是事务的根"
#: ../include/svn_error_codes.h:528
msgid "Object is not a revision root"
msgstr "对象不是版本的根"
#: ../include/svn_error_codes.h:532
msgid "Merge conflict during commit"
msgstr "提交时发生合并冲突"
#: ../include/svn_error_codes.h:536
msgid "A representation vanished or changed between reads"
msgstr "读取时修订版消失或改变"
#: ../include/svn_error_codes.h:540
msgid "Tried to change an immutable representation"
msgstr "试图修改不变的修订版"
#: ../include/svn_error_codes.h:544
msgid "Malformed skeleton data"
msgstr "非法骨架数据"
#: ../include/svn_error_codes.h:548
msgid "Transaction is out of date"
msgstr "事务过时"
#: ../include/svn_error_codes.h:552
msgid "Berkeley DB error"
msgstr "BDB 错误"
#: ../include/svn_error_codes.h:556
msgid "Berkeley DB deadlock error"
msgstr "BDB 死锁"
#: ../include/svn_error_codes.h:560
msgid "Transaction is dead"
msgstr "事务已经结束"
#: ../include/svn_error_codes.h:564
msgid "Transaction is not dead"
msgstr "事务尚未结束"
#: ../include/svn_error_codes.h:569
msgid "Unknown FS type"
msgstr "未知的FS类型"
#: ../include/svn_error_codes.h:574
msgid "No user associated with filesystem"
msgstr "没有用户与文件系统关联"
#: ../include/svn_error_codes.h:579
msgid "Path is already locked"
msgstr "已经锁定路径"
#: ../include/svn_error_codes.h:584 ../include/svn_error_codes.h:736
msgid "Path is not locked"
msgstr "路径没有加锁"
#: ../include/svn_error_codes.h:589
msgid "Lock token is incorrect"
msgstr "不正确的锁定令牌"
#: ../include/svn_error_codes.h:594
msgid "No lock token provided"
msgstr "没有提供锁定令牌"
#: ../include/svn_error_codes.h:599
msgid "Username does not match lock owner"
msgstr "用户不是锁所有者"
#: ../include/svn_error_codes.h:604
msgid "Filesystem has no such lock"
msgstr "文件系统没有此锁"
#: ../include/svn_error_codes.h:609
msgid "Lock has expired"
msgstr "锁过期"
#: ../include/svn_error_codes.h:614 ../include/svn_error_codes.h:723
msgid "Item is out of date"
msgstr "条目过时"
#: ../include/svn_error_codes.h:626
msgid "Unsupported FS format"
msgstr "不支持的文件系统格式"
#: ../include/svn_error_codes.h:631
msgid "Representation is being written"
msgstr "正在写修订版"
#: ../include/svn_error_codes.h:636
msgid "The generated transaction name is too long"
msgstr "产生的事务名称太长"
#: ../include/svn_error_codes.h:641
msgid "SQLite error"
msgstr "SQLite 错误"
#: ../include/svn_error_codes.h:646
msgid "Filesystem has no such node origin record"
msgstr "文件系统没有此节点的原始记录"
#: ../include/svn_error_codes.h:651
msgid "Attempted to write to readonly SQLite db"
msgstr "试图写只读的 SQLite 数据库"
#: ../include/svn_error_codes.h:657
msgid "The repository is locked, perhaps for db recovery"
msgstr "版本库被锁，可能正在恢复"
#: ../include/svn_error_codes.h:661
msgid "A repository hook failed"
msgstr "版本库钩子错误"
#: ../include/svn_error_codes.h:665
msgid "Incorrect arguments supplied"
msgstr "提供了不正确的参数"
#: ../include/svn_error_codes.h:669
msgid "A report cannot be generated because no data was supplied"
msgstr "没有数据，无法产生报告"
#: ../include/svn_error_codes.h:673
msgid "Bogus revision report"
msgstr "版本报告非法"
#: ../include/svn_error_codes.h:682
msgid "Unsupported repository version"
msgstr "不支持此版本库版本"
#: ../include/svn_error_codes.h:686
msgid "Disabled repository feature"
msgstr "关闭版本库特性"
#: ../include/svn_error_codes.h:690
msgid "Error running post-commit hook"
msgstr "执行 post-commit 钩子错误"
#: ../include/svn_error_codes.h:695
msgid "Error running post-lock hook"
msgstr "执行 post-lock 钩子错误"
#: ../include/svn_error_codes.h:700
msgid "Error running post-unlock hook"
msgstr "执行 post-unlock 钩子错误"
#: ../include/svn_error_codes.h:707
msgid "Bad URL passed to RA layer"
msgstr "传递至 RA 层的 URL 错误"
#: ../include/svn_error_codes.h:711
msgid "Authorization failed"
msgstr "认证失败"
#: ../include/svn_error_codes.h:715
msgid "Unknown authorization method"
msgstr "未知认证"
#: ../include/svn_error_codes.h:719
msgid "Repository access method not implemented"
msgstr "版本库未实现存取方法"
#: ../include/svn_error_codes.h:727
msgid "Repository has no UUID"
msgstr "版本库没有 UUID"
#: ../include/svn_error_codes.h:731
msgid "Unsupported RA plugin ABI version"
msgstr "不支持此 RA 插件的 ABI 版本"
#: ../include/svn_error_codes.h:741
msgid "Inquiry about unknown capability"
msgstr "查询未知特性"
#: ../include/svn_error_codes.h:745
msgid "Server can only replay from the root of a repository"
msgstr "服务器只能从版本库的根重放"
#: ../include/svn_error_codes.h:751
msgid "RA layer failed to init socket layer"
msgstr "RA 层无法初始化 socket 层"
#: ../include/svn_error_codes.h:755
msgid "RA layer failed to create HTTP request"
msgstr "RA 层创建 HTTP 请求失败"
#: ../include/svn_error_codes.h:759
msgid "RA layer request failed"
msgstr "RA 层请求失败"
#: ../include/svn_error_codes.h:763
msgid "RA layer didn't receive requested OPTIONS info"
msgstr "RA 层无法取得请求的 OPTIONS 信息"
#: ../include/svn_error_codes.h:767
msgid "RA layer failed to fetch properties"
msgstr "RA 层无法取得属性"
#: ../include/svn_error_codes.h:771
msgid "RA layer file already exists"
msgstr "RA 层文件已经存在"
#: ../include/svn_error_codes.h:775
msgid "Invalid configuration value"
msgstr "无效的配置取值"
#: ../include/svn_error_codes.h:779
msgid "HTTP Path Not Found"
msgstr "找不到 HTTP 路径"
#: ../include/svn_error_codes.h:783
msgid "Failed to execute WebDAV PROPPATCH"
msgstr "执行 WebDAV PROPPATCH 失败"
#: ../include/svn_error_codes.h:788 ../include/svn_error_codes.h:829
#: ../libsvn_ra_svn/marshal.c:638 ../libsvn_ra_svn/marshal.c:756
#: ../libsvn_ra_svn/marshal.c:783
msgid "Malformed network data"
msgstr "非法网络数据"
#: ../include/svn_error_codes.h:793
msgid "Unable to extract data from response header"
msgstr "不能从响应的头信息中获取数据"
#: ../include/svn_error_codes.h:798
msgid "Repository has been moved"
msgstr "版本库已经移动"
#: ../include/svn_error_codes.h:804 ../include/svn_error_codes.h:833
msgid "Couldn't find a repository"
msgstr "无法找到版本库"
#: ../include/svn_error_codes.h:808
msgid "Couldn't open a repository"
msgstr "无法打开版本库"
#: ../include/svn_error_codes.h:813
msgid "Special code for wrapping server errors to report to client"
msgstr "用专用代码封装服务器错误以便报告客户端"
#: ../include/svn_error_codes.h:817
msgid "Unknown svn protocol command"
msgstr "未知的 svn 协议命令"
#: ../include/svn_error_codes.h:821
msgid "Network connection closed unexpectedly"
msgstr "网络连接意外关闭"
#: ../include/svn_error_codes.h:825
msgid "Network read/write error"
msgstr "网络读写错误"
#: ../include/svn_error_codes.h:837
msgid "Client/server version mismatch"
msgstr "客户端/服务器版本不匹配"
#: ../include/svn_error_codes.h:842
msgid "Cannot negotiate authentication mechanism"
msgstr "无法协商认证机制"
#: ../include/svn_error_codes.h:848
msgid "Initialization of SSPI library failed"
msgstr "初始化 SSPI 库失败"
#: ../include/svn_error_codes.h:856
msgid "Credential data unavailable"
msgstr "无法取得凭证数据"
#: ../include/svn_error_codes.h:860
msgid "No authentication provider available"
msgstr "没有可用的认证提供者"
#: ../include/svn_error_codes.h:864 ../include/svn_error_codes.h:868
msgid "All authentication providers exhausted"
msgstr "所有的认证提供者都不可用"
#: ../include/svn_error_codes.h:873
msgid "Authentication failed"
msgstr "认证失败"
#: ../include/svn_error_codes.h:879
msgid "Read access denied for root of edit"
msgstr "编辑根目录时拒绝读取"
#: ../include/svn_error_codes.h:884
msgid "Item is not readable"
msgstr "条目不可读"
#: ../include/svn_error_codes.h:889
msgid "Item is partially readable"
msgstr "条目部分可读"
#: ../include/svn_error_codes.h:893
msgid "Invalid authz configuration"
msgstr "认证配置无效"
#: ../include/svn_error_codes.h:898
msgid "Item is not writable"
msgstr "条目不能写"
#: ../include/svn_error_codes.h:904
msgid "Svndiff data has invalid header"
msgstr "svndiff 数据包含无效头"
#: ../include/svn_error_codes.h:908
msgid "Svndiff data contains corrupt window"
msgstr "Svndiff 数据包含损坏窗口"
#: ../include/svn_error_codes.h:912
msgid "Svndiff data contains backward-sliding source view"
msgstr "Svndiff 数据包含向后变化的资源视图"
#: ../include/svn_error_codes.h:916
msgid "Svndiff data contains invalid instruction"
msgstr "svndiff 数据包含无效指令"
#: ../include/svn_error_codes.h:920
msgid "Svndiff data ends unexpectedly"
msgstr "svndiff 数据意外结束"
#: ../include/svn_error_codes.h:924
msgid "Svndiff compressed data is invalid"
msgstr "非法 svndiff 压缩数据"
#: ../include/svn_error_codes.h:930
msgid "Diff data source modified unexpectedly"
msgstr "svndiff 数据被意外修改"
#: ../include/svn_error_codes.h:936
msgid "Apache has no path to an SVN filesystem"
msgstr "Apache 没有指向 SVN 文件系统的路径"
#: ../include/svn_error_codes.h:940
msgid "Apache got a malformed URI"
msgstr "Apache 得到非法 URI"
#: ../include/svn_error_codes.h:944
msgid "Activity not found"
msgstr "没有找到活动项"
#: ../include/svn_error_codes.h:948
msgid "Baseline incorrect"
msgstr "基线错误"
#: ../include/svn_error_codes.h:952
msgid "Input/output error"
msgstr "输出/输出错误"
#: ../include/svn_error_codes.h:958
msgid "A path under version control is needed for this operation"
msgstr "只能对纳入版本控制的路径执行此操作"
#: ../include/svn_error_codes.h:962
msgid "Repository access is needed for this operation"
msgstr "此操作需要存取版本库"
#: ../include/svn_error_codes.h:966
msgid "Bogus revision information given"
msgstr "给出的版本信息非法"
#: ../include/svn_error_codes.h:970
msgid "Attempting to commit to a URL more than once"
msgstr "试图对 URL 进行多次提交"
#: ../include/svn_error_codes.h:974
msgid "Operation does not apply to binary file"
msgstr "不能对二进制文件执行此操作"
#: ../include/svn_error_codes.h:980
msgid "Format of an svn:externals property was invalid"
msgstr "svn:externals 属性格式非法"
#: ../include/svn_error_codes.h:984
msgid "Attempting restricted operation for modified resource"
msgstr "试图对已修改的资源进行受限操作"
#: ../include/svn_error_codes.h:988
msgid "Operation does not apply to directory"
msgstr "不能对目录执行此操作"
#: ../include/svn_error_codes.h:992
msgid "Revision range is not allowed"
msgstr "不允许的版本范围"
#: ../include/svn_error_codes.h:996
msgid "Inter-repository relocation not allowed"
msgstr "不支持版本库之间的重新定位"
#: ../include/svn_error_codes.h:1000
msgid "Author name cannot contain a newline"
msgstr "作者名称不能换行"
#: ../include/svn_error_codes.h:1004
msgid "Bad property name"
msgstr "属性名称错误"
#: ../include/svn_error_codes.h:1009
msgid "Two versioned resources are unrelated"
msgstr "两个纳入版本控制的资源不相关"
#: ../include/svn_error_codes.h:1014
msgid "Path has no lock token"
msgstr "路径没有锁定令牌"
#: ../include/svn_error_codes.h:1019
msgid "Operation does not support multiple sources"
msgstr "此操作不支持多个源对象"
#: ../include/svn_error_codes.h:1024
msgid "No versioned parent directories"
msgstr "没有受版本控制的父目录"
#: ../include/svn_error_codes.h:1030
msgid "A problem occurred; see later errors for details"
msgstr "发生问题；请参阅随后的错误信息"
#: ../include/svn_error_codes.h:1034
msgid "Failure loading plugin"
msgstr "加载插件失败"
#: ../include/svn_error_codes.h:1038
msgid "Malformed file"
msgstr "文件格式错误"
#: ../include/svn_error_codes.h:1042
msgid "Incomplete data"
msgstr "数据不完整"
#: ../include/svn_error_codes.h:1046
msgid "Incorrect parameters given"
msgstr "参数不正确"
#: ../include/svn_error_codes.h:1050
msgid "Tried a versioning operation on an unversioned resource"
msgstr "试图对未纳入版本控制的资源进行版本操作"
#: ../include/svn_error_codes.h:1054
msgid "Test failed"
msgstr "测试失败"
#: ../include/svn_error_codes.h:1058
msgid "Trying to use an unsupported feature"
msgstr "试图使用不支持的特性"
#: ../include/svn_error_codes.h:1062
msgid "Unexpected or unknown property kind"
msgstr "意外或未知的属性类型"
#: ../include/svn_error_codes.h:1066
msgid "Illegal target for the requested operation"
msgstr "此请求操作的目标非法"
#: ../include/svn_error_codes.h:1070
msgid "MD5 checksum is missing"
msgstr "没有 MD5 校验和"
#: ../include/svn_error_codes.h:1074
msgid "Directory needs to be empty but is not"
msgstr "必须为空的目录有内容"
#: ../include/svn_error_codes.h:1078
msgid "Error calling external program"
msgstr "调用外部程序错误"
#: ../include/svn_error_codes.h:1082
msgid "Python exception has been set with the error"
msgstr "Python 异常被设为错误"
#: ../include/svn_error_codes.h:1086
msgid "A checksum mismatch occurred"
msgstr "校验和错误"
#: ../include/svn_error_codes.h:1090
msgid "The operation was interrupted"
msgstr "操作被中断"
#: ../include/svn_error_codes.h:1094
msgid "The specified diff option is not supported"
msgstr "不支持指定的 diff 选项"
#: ../include/svn_error_codes.h:1098
msgid "Property not found"
msgstr "找不到属性"
#: ../include/svn_error_codes.h:1102
msgid "No auth file path available"
msgstr "未提供 auth 文件路径"
#: ../include/svn_error_codes.h:1107
msgid "Incompatible library version"
msgstr "不兼容的库版本"
#: ../include/svn_error_codes.h:1112
msgid "Mergeinfo parse error"
msgstr "分析合并信息出错"
#: ../include/svn_error_codes.h:1117
msgid "Cease invocation of this API"
msgstr "停止调用此 API"
#: ../include/svn_error_codes.h:1122
msgid "Error parsing revision number"
msgstr "解析版本号出错"
#: ../include/svn_error_codes.h:1127
msgid "Iteration terminated before completion"
msgstr "迭代在完成前终止"
#: ../include/svn_error_codes.h:1131
msgid "Unknown changelist"
msgstr "未知修改列表"
#: ../include/svn_error_codes.h:1137
msgid "Error parsing arguments"
msgstr "解析参数出错"
#: ../include/svn_error_codes.h:1141
msgid "Not enough arguments provided"
msgstr "没有提供足够的参数"
#: ../include/svn_error_codes.h:1145
msgid "Mutually exclusive arguments specified"
msgstr "参数冲突"
#: ../include/svn_error_codes.h:1149
msgid "Attempted command in administrative dir"
msgstr "试图在管理目录中执行命令"
#: ../include/svn_error_codes.h:1153
msgid "The log message file is under version control"
msgstr "日志信息文件被纳入版本控制"
#: ../include/svn_error_codes.h:1157
msgid "The log message is a pathname"
msgstr "日志信息是路径名"
#: ../include/svn_error_codes.h:1161
msgid "Committing in directory scheduled for addition"
msgstr "在调度增加的目录中进行提交"
#: ../include/svn_error_codes.h:1165
msgid "No external editor available"
msgstr "没有外部编辑器可用"
#: ../include/svn_error_codes.h:1169
msgid "Something is wrong with the log message's contents"
msgstr "日志信息内容不妥"
#: ../include/svn_error_codes.h:1173
msgid "A log message was given where none was necessary"
msgstr "在不需要时给出日志信息"
#: ../include/svn_error_codes.h:1177
msgid "No external merge tool available"
msgstr "没有外部合并工具可用"
#: ../libsvn_client/add.c:348 ../libsvn_wc/copy.c:188
#, c-format
msgid "Can't close directory '%s'"
msgstr "无法关闭目录 “%s”"
#: ../libsvn_client/add.c:356
#, c-format
msgid "Error during add of '%s'"
msgstr "增加 “%s” 时出错"
#: ../libsvn_client/blame.c:391
#, c-format
msgid "Cannot calculate blame information for binary file '%s'"
msgstr "无法为二进制文件 “%s” 计算追溯信息"
#: ../libsvn_client/blame.c:621
msgid "blame of the WORKING revision is not supported"
msgstr "工作版本不支持追溯"
#: ../libsvn_client/blame.c:635
msgid "Start revision must precede end revision"
msgstr "起始版本必须小于结束版本"
#: ../libsvn_client/cat.c:74
#, c-format
msgid "'%s' refers to a directory"
msgstr "“%s” 引用一个目录"
#: ../libsvn_client/cat.c:126 ../libsvn_client/export.c:183
msgid "(local)"
msgstr "(本地)"
#: ../libsvn_client/cat.c:206
#, c-format
msgid "URL '%s' refers to a directory"
msgstr "URL “%s” 指向目录"
#: ../libsvn_client/checkout.c:91 ../libsvn_client/export.c:943
#, c-format
msgid "URL '%s' doesn't exist"
msgstr "URL “%s” 不存在"
#: ../libsvn_client/checkout.c:95
#, c-format
msgid "URL '%s' refers to a file, not a directory"
msgstr "URL “%s” 指向一个文件，不是目录"
#: ../libsvn_client/checkout.c:167
#, c-format
msgid "'%s' is already a working copy for a different URL"
msgstr "“%s” 已经是指向不同 URL 的工作副本"
#: ../libsvn_client/checkout.c:171
msgid "; run 'svn update' to complete it"
msgstr "；请执行 “svn update” 来完成操作。"
#: ../libsvn_client/checkout.c:181
#, c-format
msgid "'%s' already exists and is not a directory"
msgstr "“%s” 已经存在并且不是目录"
#: ../libsvn_client/commit.c:433
#, c-format
msgid "Unknown or unversionable type for '%s'"
msgstr "“%s” 是未知类型或不可纳入版本控制"
#: ../libsvn_client/commit.c:538
msgid "New entry name required when importing a file"
msgstr "导入文件时，需要一个新的条目名称"
#: ../libsvn_client/commit.c:573 ../libsvn_wc/adm_ops.c:1037
#: ../libsvn_wc/questions.c:93
#, c-format
msgid "'%s' does not exist"
msgstr "“%s” 不存在"
#: ../libsvn_client/commit.c:634 ../libsvn_client/copy.c:518
#: ../svnlook/main.c:1271
#, c-format
msgid "Path '%s' does not exist"
msgstr "路径 “%s” 不存在"
#: ../libsvn_client/commit.c:769 ../libsvn_client/copy.c:527
#: ../libsvn_client/copy.c:917 ../libsvn_client/copy.c:1158
#: ../libsvn_client/copy.c:1585
#, c-format
msgid "Path '%s' already exists"
msgstr "路径 “%s” 已经存在"
#: ../libsvn_client/commit.c:784
#, c-format
msgid "'%s' is a reserved name and cannot be imported"
msgstr "“%s” 是保留名称，无法被导入"
#: ../libsvn_client/commit.c:917 ../libsvn_client/copy.c:1325
msgid "Commit failed (details follow):"
msgstr "提交失败(细节如下): "
#: ../libsvn_client/commit.c:925
msgid "Commit succeeded, but other errors follow:"
msgstr "提交成功，但是发生了其它错误，细节如下: "
#: ../libsvn_client/commit.c:932
msgid "Error unlocking locked dirs (details follow):"
msgstr "解除锁定目录出错(细节如下): "
#: ../libsvn_client/commit.c:943
msgid "Error bumping revisions post-commit (details follow):"
msgstr "执行 post-commit 出错 (细节如下): "
#: ../libsvn_client/commit.c:954
msgid "Error in post-commit clean-up (details follow):"
msgstr "清理 post-commit 出错 (细节如下): "
#: ../libsvn_client/commit.c:1304
msgid "Are all the targets part of the same working copy?"
msgstr "是否所有目标都属于同一工作副本？"
#: ../libsvn_client/commit.c:1337
msgid "Cannot non-recursively commit a directory deletion"
msgstr "无法以非递归提交方式删除目录"
#: ../libsvn_client/commit.c:1381
#, c-format
msgid "'%s' is a URL, but URLs cannot be commit targets"
msgstr "“%s” 是 URL，但是 URL 不可作为提交目标"
#: ../libsvn_client/commit_util.c:264 ../libsvn_client/commit_util.c:275
#, c-format
msgid "Unknown entry kind for '%s'"
msgstr "“%s” 有未知的条目类型"
#: ../libsvn_client/commit_util.c:292
#, c-format
msgid "Entry '%s' has unexpectedly changed special status"
msgstr "条目 “%s” 意外改变状态"
#: ../libsvn_client/commit_util.c:347
#, c-format
msgid "Aborting commit: '%s' remains in conflict"
msgstr "提交终止: “%s” 处于冲突状态"
#: ../libsvn_client/commit_util.c:414
#, c-format
msgid "Did not expect '%s' to be a working copy root"
msgstr "不期望 “%s” 是工作副本根目录"
#: ../libsvn_client/commit_util.c:432 ../libsvn_client/commit_util.c:1111
#, c-format
msgid "Commit item '%s' has copy flag but no copyfrom URL"
msgstr "提交项目 “%s” 有复制标记，但是没有源地址(copyfrom URL)"
#: ../libsvn_client/commit_util.c:694
#, c-format
msgid "'%s' is not under version control and is not part of the commit, yet its child '%s' is part of the commit"
msgstr "“%s” 尚未纳入版本控制，也不是提交的一部份，但是它的子路径 “%s” 是提交的一部份"
#: ../libsvn_client/commit_util.c:782 ../libsvn_client/url.c:155
#, c-format
msgid "Entry for '%s' has no URL"
msgstr "条目 “%s” 没有URL"
#: ../libsvn_client/commit_util.c:815
#, c-format
msgid "'%s' is scheduled for addition within unversioned parent"
msgstr "在未纳入版本控制的父目录，“%s” 被加入增加调度"
#: ../libsvn_client/commit_util.c:835
#, c-format
msgid ""
"Entry for '%s' is marked as 'copied' but is not itself scheduledn"
"for addition. Perhaps you're committing a target that isn"
"inside an unversioned (or not-yet-versioned) directory?"
msgstr ""
"条目 “%s” 被标记为“已复制”，但是本身尚未加入增加调度。也许您提交的n"
"目标在未纳入版本控制的目录中？"
#: ../libsvn_client/commit_util.c:967
#, c-format
msgid "Cannot commit both '%s' and '%s' as they refer to the same URL"
msgstr "无法同时提交 “%s” 与 “%s”，因为它们都指向同一个 URL"
#: ../libsvn_client/commit_util.c:1116
#, c-format
msgid "Commit item '%s' has copy flag but an invalid revision"
msgstr "提交项目 “%s” 有复制标志，但是版本无效"
#: ../libsvn_client/commit_util.c:1875
msgid "Standard properties can't be set explicitly as revision properties"
msgstr "标准属性不能设置为版本属性"
#: ../libsvn_client/copy.c:543 ../libsvn_client/copy.c:1601
#: ../libsvn_client/update.c:140
#, c-format
msgid "Path '%s' is not a directory"
msgstr "路径 “%s” 不是目录"
#: ../libsvn_client/copy.c:783
#, c-format
msgid "Source and dest appear not to be in the same repository (src: '%s'; dst: '%s')"
msgstr "来源与目标似乎不在同一版本库 (来源: “%s”；目的: “%s”)"
#: ../libsvn_client/copy.c:898
#, c-format
msgid "Cannot move URL '%s' into itself"
msgstr "无法移动 URL “%s” 到本身"
#: ../libsvn_client/copy.c:907 ../libsvn_client/prop_commands.c:231
#, c-format
msgid "Path '%s' does not exist in revision %ld"
msgstr "路径 “%s” 不在版本 %ld 中"
#: ../libsvn_client/copy.c:1424
#, c-format
msgid "Source URL '%s' is from foreign repository; leaving it as a disjoint WC"
msgstr "源 URL “%s” 来自其它版本库；把它当作脱离的工作副本"
#: ../libsvn_client/copy.c:1572
#, c-format
msgid "Path '%s' not found in revision %ld"
msgstr "路径 “%s” 不在版本 %ld 中"
#: ../libsvn_client/copy.c:1577
#, c-format
msgid "Path '%s' not found in head revision"
msgstr "HEAD 版本中找不到路径 “%s”"
#: ../libsvn_client/copy.c:1627
#, c-format
msgid "Entry for '%s' exists (though the working file is missing)"
msgstr "“%s” 的入口存在(尽管丢失了工作文件)"
#: ../libsvn_client/copy.c:1719 ../libsvn_client/log.c:342
msgid "Revision type requires a working copy path, not a URL"
msgstr "版本类型需要工作目录，不是 URL"
#: ../libsvn_client/copy.c:1764
msgid "Cannot mix repository and working copy sources"
msgstr "不能在源中混合版本库和工作副本"
#: ../libsvn_client/copy.c:1807
#, c-format
msgid "Cannot copy path '%s' into its own child '%s'"
msgstr "无法复制路径 “%s” 到其子目录 “%s” 中"
#: ../libsvn_client/copy.c:1827
#, c-format
msgid "Cannot move path '%s' into itself"
msgstr "无法移动路径 “%s” 到本身"
#: ../libsvn_client/copy.c:1836
msgid "Moves between the working copy and the repository are not supported"
msgstr "不支持在工作副本和版本库之间移动"
#: ../libsvn_client/copy.c:1899
#, c-format
msgid "'%s' does not have a URL associated with it"
msgstr "“%s” 没有关联的 URL"
#: ../libsvn_client/copy.c:2269 ../svn/move-cmd.c:60
msgid "Cannot specify revisions (except HEAD) with move operations"
msgstr "移动操作不能指定版本"
#: ../libsvn_client/delete.c:62
#, c-format
msgid "'%s' is in the way of the resource actually under version control"
msgstr "“%s” 阻碍资源纳入版本控制"
#: ../libsvn_client/delete.c:67 ../libsvn_wc/adm_ops.c:2914
#: ../libsvn_wc/entries.c:1272 ../libsvn_wc/entries.c:2383
#: ../libsvn_wc/entries.c:3005 ../libsvn_wc/update_editor.c:2366
#, c-format
msgid "'%s' is not under version control"
msgstr "“%s” 尚未纳入版本控制"
#: ../libsvn_client/delete.c:77
#, c-format
msgid "'%s' has local modifications"
msgstr "“%s” 已有本地修改"
#: ../libsvn_client/diff.c:136
#, c-format
msgid "   Reverted %s:r%s%s"
msgstr "   已恢复 %s:r%s%s"
#: ../libsvn_client/diff.c:154
#, c-format
msgid "   Merged %s:r%s%s"
msgstr "   已合并 %s:r%s%s"
#: ../libsvn_client/diff.c:164 ../libsvn_diff/diff_file.c:1203
#: ../libsvn_diff/diff_file.c:1215
#, c-format
msgid "Path '%s' must be an immediate child of the directory '%s'"
msgstr "路径 “%s” 必须是目录 “%s” 的直接子孙"
#: ../libsvn_client/diff.c:195
#, c-format
msgid "%sProperty changes on: %s%s"
msgstr "%s 属性改变: %s%s"
#: ../libsvn_client/diff.c:224
#, c-format
msgid "Added: %s%s"
msgstr "已增加: %s%s"
#: ../libsvn_client/diff.c:226
#, c-format
msgid "Deleted: %s%s"
msgstr "已删除: %s%s"
#: ../libsvn_client/diff.c:228
#, c-format
msgid "Modified: %s%s"
msgstr "已修改: %s%s"
#: ../libsvn_client/diff.c:351
#, c-format
msgid "%st(revision %ld)"
msgstr "%st（版本 %ld）"
#: ../libsvn_client/diff.c:353
#, c-format
msgid "%st(working copy)"
msgstr "%st（工作副本）"
#: ../libsvn_client/diff.c:530
#, c-format
msgid "Cannot display: file marked as a binary type.%s"
msgstr "无法显示: 文件标记为二进制类型。%s"
#: ../libsvn_client/diff.c:890 ../libsvn_client/merge.c:3511
#: ../libsvn_client/merge.c:4630
msgid "Not all required revisions are specified"
msgstr "没有全部提供需要的版本"
#: ../libsvn_client/diff.c:905
msgid "At least one revision must be non-local for a pegged diff"
msgstr "对于 peg 差异，必须至少有一个非本地版本"
#: ../libsvn_client/diff.c:1017 ../libsvn_client/diff.c:1029
#, c-format
msgid "'%s' was not found in the repository at revision %ld"
msgstr "“%s” 不在版本库版本 %ld 中"
#: ../libsvn_client/diff.c:1088
msgid "Sorry, svn_client_diff4 was called in a way that is not yet supported"
msgstr "抱歉，svn_client_diff4 尚不支持这种调用方式"
#: ../libsvn_client/diff.c:1128
msgid "Only diffs between a path's text-base and its working files are supported at this time"
msgstr "当前 diff 只支持参考文件与其工作文件比较"
#: ../libsvn_client/diff.c:1273 ../libsvn_client/switch.c:126
#, c-format
msgid "Directory '%s' has no URL"
msgstr "目录 “%s” 没有 URL"
#: ../libsvn_client/diff.c:1483
msgid "Summarizing diff can only compare repository to repository"
msgstr "只有版本库和版本库才能比较差异概要"
#: ../libsvn_client/export.c:87
#, c-format
msgid "'%s' is not a valid EOL value"
msgstr "“%s” 不是一个有效的 EOL 值"
#: ../libsvn_client/export.c:261
msgid "Destination directory exists, and will not be overwritten unless forced"
msgstr "目的目录已存在，除非强迫为之，否则不会覆盖"
#: ../libsvn_client/export.c:407 ../libsvn_client/export.c:550
#, c-format
msgid "'%s' exists and is not a directory"
msgstr "“%s” 已存在且不是目录"
#: ../libsvn_client/export.c:411 ../libsvn_client/export.c:554
#, c-format
msgid "'%s' already exists"
msgstr "“%s” 已存在"
#: ../libsvn_client/export.c:725 ../libsvn_wc/adm_crawler.c:1092
#: ../libsvn_wc/update_editor.c:2050 ../libsvn_wc/update_editor.c:2716
#, c-format
msgid "Checksum mismatch for '%s'; expected: '%s', actual: '%s'"
msgstr "“%s” 的校验和不匹配；期望: “%s”，实际: “%s”"
#: ../libsvn_client/externals.c:315
#, c-format
msgid "URL '%s' does not begin with a scheme"
msgstr "URL “%s” 没有以方案开始"
#: ../libsvn_client/externals.c:364
#, c-format
msgid "Illegal parent directory URL '%s'."
msgstr "非法父目录 URL “%s”。"
#: ../libsvn_client/externals.c:394
#, c-format
msgid "Illegal repository root URL '%s'."
msgstr "非法版本库根 URL “%s”。"
#: ../libsvn_client/externals.c:436
#, c-format
msgid "The external relative URL '%s' cannot have backpaths, i.e. '..'."
msgstr "关联的外部 URL “%s” 不能包含后退路径，例如 “..”。"
#: ../libsvn_client/externals.c:468
#, c-format
msgid "Unrecognized format for the relative external URL '%s'."
msgstr "关联的外部 URL “%s” 的格式无法识别。"
#: ../libsvn_client/externals.c:690
#, c-format
msgid "Traversal of '%s' found no ambient depth"
msgstr "在 “%s” 中找不到深度"
#: ../libsvn_client/info.c:420
#, c-format
msgid "Server does not support retrieving information about the repository root"
msgstr "服务器不支持取得版本库根的信息"
#: ../libsvn_client/info.c:427 ../libsvn_client/info.c:442
#: ../libsvn_client/info.c:452
#, c-format
msgid "URL '%s' non-existent in revision %ld"
msgstr "URL “%s” 不在版本 %ld 中"
#: ../libsvn_client/list.c:235
#, c-format
msgid "URL '%s' non-existent in that revision"
msgstr "该版本没有 URL “%s”"
#: ../libsvn_client/locking_commands.c:199
msgid "No common parent found, unable to operate on disjoint arguments"
msgstr "没有找到公共父节点，不能在分离的参数上操作"
#: ../libsvn_client/locking_commands.c:261 ../libsvn_client/merge.c:4647
#: ../libsvn_client/merge.c:4653 ../libsvn_client/merge.c:4947
#: ../libsvn_client/ra.c:389 ../libsvn_client/ra.c:426
#: ../libsvn_client/ra.c:627
#, c-format
msgid "'%s' has no URL"
msgstr "“%s” 没有 URL"
#: ../libsvn_client/locking_commands.c:285
msgid "Unable to lock/unlock across multiple repositories"
msgstr "不能跨越多个版本库进行锁定/解锁"
#: ../libsvn_client/locking_commands.c:326
#, c-format
msgid "'%s' is not locked in this working copy"
msgstr "“%s” 没有在工作副本中锁定"
#: ../libsvn_client/locking_commands.c:374
#, c-format
msgid "'%s' is not locked"
msgstr "“%s” 没有被锁定"
#: ../libsvn_client/locking_commands.c:407 ../libsvn_fs/fs-loader.c:1021
#: ../libsvn_ra/ra_loader.c:1105
msgid "Lock comment contains illegal characters"
msgstr "锁定注释包含非法字符"
#: ../libsvn_client/log.c:329
msgid "Missing required revision specification"
msgstr "未提供请求的版本"
#: ../libsvn_client/log.c:378
msgid "When specifying working copy paths, only one target may be given"
msgstr "当指定工作副本路径时，只能给出一个目标"
#: ../libsvn_client/log.c:398 ../libsvn_client/status.c:284
#: ../libsvn_client/update.c:154
#, c-format
msgid "Entry '%s' has no URL"
msgstr "条目“%s”没有URL"
#: ../libsvn_client/log.c:650
msgid "No commits in repository"
msgstr "版本库中没有提交"
#: ../libsvn_client/merge.c:129
#, c-format
msgid "URLs have no scheme ('%s' and '%s')"
msgstr "URL 中没有方案(一般需要svn://，http://，file://等开头) (“%s”与“%s”)"
#: ../libsvn_client/merge.c:135 ../libsvn_client/merge.c:141
#, c-format
msgid "URL has no scheme: '%s'"
msgstr "URL 中没有方案(一般需要svn://，http://，file://等开头): “%s”"
#: ../libsvn_client/merge.c:148
#, c-format
msgid "Access scheme mixtures not yet supported ('%s' and '%s')"
msgstr "不支持存取混合方案 (“%s”与“%s”)"
#. xgettext: the '.working', '.merge-left.r%ld' and
#. '.merge-right.r%ld' strings are used to tag onto a file
#. name in case of a merge conflict
#: ../libsvn_client/merge.c:518
msgid ".working"
msgstr ".working"
#: ../libsvn_client/merge.c:520
#, c-format
msgid ".merge-left.r%ld"
msgstr ".merge-left.r%ld"
#: ../libsvn_client/merge.c:523
#, c-format
msgid ".merge-right.r%ld"
msgstr ".merge-right.r%ld"
#: ../libsvn_client/merge.c:1964
#, c-format
msgid ""
"One or more conflicts were produced while merging r%ld:%ld inton"
"'%s' --n"
"resolve all conflicts and rerun the merge to apply the remainingn"
"unmerged revisions"
msgstr ""
"当合并 r%ld:%ld 到 “%s” 时出现了一个或更多的冲突n"
"请解决所有冲突后，再次运行合并，处理剩余的未合并版本"
#: ../libsvn_client/merge.c:4404
msgid "Use of two URLs is not compatible with mergeinfo modification"
msgstr "使用两个 URL 与合并信息修改不兼容"
#: ../libsvn_client/prop_commands.c:73
#, c-format
msgid "'%s' is a wcprop, thus not accessible to clients"
msgstr "“%s” 是一个工作副本属性，应此不能被客户端存取"
#: ../libsvn_client/prop_commands.c:218
#, c-format
msgid "Property '%s' is not a regular property"
msgstr "属性 “%s” 不是常规属性"
#: ../libsvn_client/prop_commands.c:323
#, c-format
msgid "Revision property '%s' not allowed in this context"
msgstr "此上下文中不允许版本属性 “%s”"
#: ../libsvn_client/prop_commands.c:331 ../libsvn_client/prop_commands.c:462
#, c-format
msgid "Bad property name: '%s'"
msgstr "属性名称错误: “%s”"
#: ../libsvn_client/prop_commands.c:342
#, c-format
msgid "Setting property on non-local target '%s' needs a base revision"
msgstr "设定非本地目标 “%s” 的属性需要版本参数"
#: ../libsvn_client/prop_commands.c:348
#, c-format
msgid "Setting property recursively on non-local target '%s' is not supported"
msgstr "不支持递归设置非本地目标 “%s” 的属性"
#: ../libsvn_client/prop_commands.c:365
#, c-format
msgid "Setting property '%s' on non-local target '%s' is not supported"
msgstr "不支持设置属性 “%s” 于非本地目标 “%s”"
#: ../libsvn_client/prop_commands.c:458
msgid "Value will not be set unless forced"
msgstr "除非强迫为之，否则其值不会被设定"
#: ../libsvn_client/prop_commands.c:634
#, c-format
msgid "'%s' does not exist in revision %ld"
msgstr "没有路径 “%s” 在版本 %ld 中"
#: ../libsvn_client/prop_commands.c:641 ../libsvn_client/prop_commands.c:976
#, c-format
msgid "Unknown node kind for '%s'"
msgstr "“%s” 是未知的节点类型"
#: ../libsvn_client/ra.c:135
#, c-format
msgid "Attempt to set wc property '%s' on '%s' in a non-commit operation"
msgstr "试图在非提交操作中，设定工作副本属性 “%s” 于 “%s”"
#: ../libsvn_client/ra.c:696 ../libsvn_ra/compat.c:372
#, c-format
msgid "Unable to find repository location for '%s' in revision %ld"
msgstr "“%s” 的版本库位置不在版本 %ld 中"
#: ../libsvn_client/ra.c:703
#, c-format
msgid "The location for '%s' for revision %ld does not exist in the repository or refers to an unrelated object"
msgstr "“%s” 不在版本库的版本 %ld 中，或是指向一个无关对象"
#: ../libsvn_client/relocate.c:101
#, c-format
msgid "'%s' is not the root of the repository"
msgstr "“%s” 不是版本库的根"
#: ../libsvn_client/relocate.c:108
#, c-format
msgid "The repository at '%s' has uuid '%s', but the WC has '%s'"
msgstr "版本库 “%s” 的 uuid 是 “%s”，但是工作副本的是 “%s”"
#: ../libsvn_client/revisions.c:99
#, c-format
msgid "Path '%s' has no committed revision"
msgstr "路径 “%s” 没有提交过"
#: ../libsvn_client/revisions.c:126
#, c-format
msgid "Unrecognized revision type requested for '%s'"
msgstr "“%s” 的请求版本类型无法识别"
#: ../libsvn_client/switch.c:144 ../libsvn_ra_local/ra_plugin.c:195
#: ../libsvn_ra_local/ra_plugin.c:270 ../libsvn_wc/update_editor.c:3209
#, c-format
msgid ""
"'%s'n"
"is not the same repository asn"
"'%s'"
msgstr ""
"“%s”n"
"与n"
"“%s”n"
"并不在同一个版本库中"
#: ../libsvn_client/util.c:216
#, c-format
msgid "URL '%s' is not a child of repository root URL '%s'"
msgstr "URL “%s” 不是版本库根 URL “%s” 的子节点"
#: ../libsvn_delta/svndiff.c:144
msgid "Compression of svndiff data failed"
msgstr "压缩 svndiff 数据失败"
#: ../libsvn_delta/svndiff.c:407
msgid "Decompression of svndiff data failed"
msgstr "解压 svndiff 数据失败"
#: ../libsvn_delta/svndiff.c:414
msgid "Size of uncompressed data does not match stored original length"
msgstr "解压后的数据长度与原始长度不相等"
#: ../libsvn_delta/svndiff.c:484
#, c-format
msgid "Invalid diff stream: insn %d cannot be decoded"
msgstr "无效的差异流: insn %d 不能解码"
#: ../libsvn_delta/svndiff.c:488
#, c-format
msgid "Invalid diff stream: insn %d has non-positive length"
msgstr "无效的差异流: insn %d 出现非正长度"
#: ../libsvn_delta/svndiff.c:492
#, c-format
msgid "Invalid diff stream: insn %d overflows the target view"
msgstr "无效的差异流: insn %d 溢出目标视图"
#: ../libsvn_delta/svndiff.c:500
#, c-format
msgid "Invalid diff stream: [src] insn %d overflows the source view"
msgstr "无效的差异流: [src] insn %d 溢出源视图"
#: ../libsvn_delta/svndiff.c:507
#, c-format
msgid "Invalid diff stream: [tgt] insn %d starts beyond the target view position"
msgstr "无效的差异流: [tgt] insn %d 起点超过目标视图位置"
#: ../libsvn_delta/svndiff.c:514
#, c-format
msgid "Invalid diff stream: [new] insn %d overflows the new data section"
msgstr "无效的差异流: [new] insn %d 溢出新数据区域"
