---
layout: post
title: 判断网页是否在iframe中 frame
date: 2013-11-06 09:13
author: admin
comments: true
categories: []
---
网上提供的很多方法都是判断当前窗口与顶部窗口是否相同来实现。代码如下 if(top!=this){
　// 在frame中时处理
}
　　但这个脚本并没有区分frame和iframe。

　　在使用脚本时IE下遇到奇怪的问题:页面只有在iframe有问题frame中是正常的。而且在Firefox和Chrome中都是正常的。已经不想对IE发表什么意见了，最终自己在MSDN找到了可以判断当前页面是否在iframe中的方法，脚本如下：

if(self.frameElement.tagName=="IFRAME"){
// 页面在iframe中时处理
}
