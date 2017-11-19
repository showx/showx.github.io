---
layout: post
title: 虚拟机安装xp出现：pxe-mof exiting intel pxe rom operating system not found
date: 2013-08-22 21:12
author: admin
comments: true
categories: []
---

在装虚拟机时vmware启动一个GhostVistaSp1总显示PXE-MOF:Exiting Intel PXE ROM. 。然后是Operating System not found ……安装盘是……
在分区进没有设置为作用分区，即活动分区。
 
主要分区跟活动分区是不同的.不设置活动分区,如果你是用GHOST来安装系统的话就会导致无法引导系统.除非你是一步步地安装系统,那样它就会自动帮你把系统盘设置成活动盘.活动分区只有主分区才能设置,逻辑分区无法设置.选择一个主分区,右键在菜单最下面一项右拉会看到设置为活动盘或是作用盘,如果你的是PQ是中文版就会这样显示,最后一项是进阶到>>设置作用盘.如果是英文版,则是set active这个是活动的英文
