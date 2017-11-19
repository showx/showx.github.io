---
layout: post
title: File already exists: filesystem '/path/file', transaction svn常见错误解决方法
date: 2014-07-18 17:26
author: admin
comments: true
categories: []
---
前言
　　多人任务基本都会用到SVN，于是提交的时候如果不先更新在提交或者操作顺序不对，会经常出现错误，其中File already exists: filesystem这个就是个常见问题，上网找了半天没找到解决办法，经过摸索，经解决办法分享于此。

解决方法
　　不同情况对应不同的解决方法：
　　1、通用的。直接先备份，然后将本地删除，然后充仓库里面checkout出最新的文件，然后将备份的修改加入最新的文件，然后提交就搞定啦 。。

　　2、localy new，本地新建。这写内容在被commit之前，可以做任何改变，包括删除，比如你新建一个目录，然后删除，那么下次commit的时候就不会体现这个过程，当没有发生过一样。所以说当你看到下面的错误代码时：File already exists: filesystem '/path/db', transaction '9-1', path  '/path/trunk/vendor/plugins/classic_pagination'  Failed to add directory object of the same name already exists[/code]是因为remote repository已经有人commit了一个目录，而你本地有一个同名的目录，很简单，你只要重命名，或者删除本地目录，就可以顺利的update了。  

　　3、如果一个目录或者文件已经是在svn控制之下（比如是checkout而来），那么你在本地对于它的任何操作都会被svn所记录，比如你删除它，然后再建立它，这些动作在commit的时候都会被远程的执行。对于删除又建立的情况，实际上你必须进行两次commit，一次是删除，另一次是新建。

总结
　　其实SVN的相关问题错误解决办法就是关于数据的一致性问题，所以没啥好纠结，想个能避免脏数据，然后将数据弄成一致的就可以了。
　　全文完。。
