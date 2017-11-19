---
layout: post
title: 强大的矢量图形库：Raphael JS 中文帮助文档及教程
date: 2013-12-30 16:53
author: admin
comments: true
categories: []
---
Raphael 是一个用于在网页中绘制矢量图形的 Javascript 库。它使用 SVG W3C 推荐标准和 VML 作为创建图形的基础，你可以通过 JavaScript 操作 DOM 来轻松创建出各种复杂的柱状图、饼图、曲线图等各种图表，还可以绘制任意形状的图形，可以进行图表或图像的裁剪和旋转等复杂操作。
Raphaël 是跨浏览器的矢量图形库，目前支持的浏览器包括： Firefox 3.0+，Safari 3.0+，Chrome 5.0+，Opera 9.5+ 以及 Internet Explorer 6.0+。
如何使用？
　　在页面中引入 raphael.js 文件，然后就可以绘制任意的矢量图形了：
// 在坐标（10,50）创建宽320，高200的画布
var paper = Raphael(10, 50, 320, 200);
 
// 在坐标（x = 50, y = 40）绘制半径为 10 的圆
var circle = paper.circle(50, 40, 10);
 
// 给绘制的圆圈填充红色 (#f00)
circle.attr("fill", "#f00");
 
// 设置画笔（stroke）的颜色为白色
circle.attr("stroke", "#fff");

http://raphaeljs.com/
