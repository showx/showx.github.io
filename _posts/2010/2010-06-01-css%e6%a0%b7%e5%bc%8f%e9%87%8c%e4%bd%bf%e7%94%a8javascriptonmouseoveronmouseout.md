---
layout: post
title: CSS样式里使用JavaScript(onmouseover/onmouseout)
date: 2010-06-01 03:11
author: admin
comments: true
categories: []
---
如何在CSS样式中利用expression实现JavaScript中的onmouseover/onmouseout事件/*当鼠标移动到这里(比如表格里的一行)的时候背景色变化为黄色*/
onmouseover: expression(onmouseover=function (){this.style.backgroundColor ='yellow'});/*当鼠标移动到这里的时候背景色还原为原来的颜色*/
onmouseout: expression(onmouseout=function (){this.style.backgroundColor =''});下面的实例在CSS样式表里使用onmouseover/onmouseout事件;当移动到单元格的时候该单元格的背景色/边框/文字颜色将改变。根据需要可以加入其它的属性,实现一些简单的网页特效。&lt;!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN"&gt;
&lt;HTML&gt;
&lt;HEAD&gt;
&lt;TITLE&gt; CSS样式里使用JavaScript(onmouseover/onmouseout)2&lt;/TITLE&gt;
&lt;META NAME="风云岁月" CONTENT=""&gt;
&lt;style type="text/css"&gt;
table
{background-color:#000000;
cursor:hand;
}td
{
/*设置onmouseover事件*/
onmouseover: expression(onmouseover=function (){this.style.borderColor ='blue';this.style.color='red';this.style.backgroundColor ='yellow'}); /*设置onmouseout事件*/
onmouseout: expression(onmouseout=function (){this.style.borderColor='';this.style.color='';this.style.backgroundColor =''});
background-color:#ffffff;
}
&lt;/style&gt;
&lt;/HEAD&gt;&lt;BODY&gt;
&lt;TABLE cellspacing='1px' border='1'&gt;
&lt;TR &gt;
&lt;TD &gt;测试表格…… &lt;/TD&gt;
&lt;TD&gt;测试表格…… &lt;/TD&gt;
&lt;TD&gt;测试表格…… &lt;/TD&gt;
&lt;/TR&gt;
&lt;TR &gt;
&lt;TD &gt;测试表格…… &lt;/TD&gt;
&lt;TD&gt;测试表格…… &lt;/TD&gt;
&lt;TD&gt;测试表格…… &lt;/TD&gt;
&lt;/TR&gt;
&lt;/TABLE&gt;
&lt;/BODY&gt;
&lt;/HTML&gt;
