---
layout: post
title: CSS：scrollbar的属性及样式分类
date: 2013-08-26 22:41
author: admin
comments: true
categories: []
---

CSS：scrollbar的属性及样式分类

1．overflow内容溢出时的设置（设定被设定对象是否显示滚动条）
    overflow-x水平方向内容溢出时的设置
    overflow-y垂直方向内容溢出时的设置
    以上三个属性设置的值为visible(默认值)、scroll、hidden、auto。
2．scrollbar-3d-light-color立体滚动条亮边的颜色（设置滚动条的颜色）
    scrollbar-arrow-color上下按钮上三角箭头的颜色
    scrollbar-base-color滚动条的基本颜色
    scrollbar-dark-shadow-color立体滚动条强阴影的颜色
    scrollbar-face-color立体滚动条凸出部分的颜色
    scrollbar-highlight-color滚动条空白部分的颜色
    scrollbar-shadow-color立体滚动条阴影的颜色 86oo精彩教程
我们通过几个实例来讲解上述的样式属性：
1.让浏览器窗口永远都不出现滚动条
    没有水平滚动条
    <body style="overflow-x:hidden">
    没有垂直滚动条
    <body style="overflow-y:hidden">
    没有滚动条
    <body style="overflow-x:hidden;overflow-y:hidden">或<body 
    style="overflow:hidden"> http://www.o.com
2.设定多行文本框的滚动条
  没有水平滚动条
   <textarea style="overflow-x:hidden"></textarea> 86oo精彩教程
   没有垂直滚动条
   <textarea style="overflow-y:hidden"></textarea> 欢迎各位访问86oo.com
   没有滚动条
   <textarea style="overflow-x:hidden;overflow-y:hidden"></textarea>
   或<textarea style="overflow:hidden"></textarea>
3.设定窗口滚动条的颜色
设置窗口滚动条的颜色为红色<body style="scrollbar-base-color:red">
scrollbar-base-color设定的是基本色，一般情况下只需要设置这一个属性就可以达到改变滚动条颜色的目的。
加上一点特别的效果：
<body style="scrollbar-arrow-color:yellow;scrollbar-base-color:lightsalmon">
 
 
4.在样式表文件中定义好一个类，调用样式表。
 
 
<style>
.coolscrollbar{scrollbar-arrow-color:yellow;scrollbar-base-color:lightsalmon;}
</style>
这样调用：
<textarea class="coolscrollbar"></textarea>
Scrollbar-Face-Color为滚动条表面颜色设定；
Scrollbar-Highlight-Color为滚动条上斜面和左斜面颜色设定；
Scrollbar-Shadow-Color为滚动条下斜面和右斜面颜色设定；
Scrollbar-3Dlight-Color为滚动条上边和左边的边沿颜色设定；
Scrollbar-Arrow-Color为滚动条两端箭头颜色设定。
Scrollbar-Track-Color为滚动条底板颜色设定；
Scrollbar-Darkshadow为滚动条下边和右边边沿颜色设定。
