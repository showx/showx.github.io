---
layout: post
title: Iframe高度自适应（兼容IE/Firefox、同域/跨域）
date: 2013-11-04 14:25
author: admin
comments: true
categories: []
---
在实际的项目进行中，很多地方可能由于历史原因不得不去使用iframe，包括目前正火热的应用开发也是如此。

随之而来的就是在实际使用iframe中，会遇到iframe高度的问题，由于被嵌套的页面长度不固定而显示出来的滚动条，不仅影响美观，还会对用户操作带来不便。于是自动调整iframe的高度就成为本文的重点。

采用JavaScript来控制iframe元素的高度是iframe高度自适应的关键，同时由于JavaScript对不同域名下权限的控制，引发出同域、跨域两种情况。

同域时Iframe高度自适应
下面的代码兼容IE/Firefox浏览器，控制id为“iframeid”的iframe的高度，通过JavaScript取得被嵌套页面最终高度，然后在主页面进行设置来实现。

代码如下，可复制。另外，请注意此解决方案仅供同域名下使用。

<script type="text/javascript">// <![CDATA[
 function SetCwinHeight(){
  var iframeid=document.getElementById("iframeid"); //iframe id
  if (document.getElementById){
   if (iframeid &#038;& !window.opera){
    if (iframeid.contentDocument &#038;& iframeid.contentDocument.body.offsetHeight){
     iframeid.height = iframeid.contentDocument.body.offsetHeight;
    }else if(iframeid.Document &#038;& iframeid.Document.body.scrollHeight){
     iframeid.height = iframeid.Document.body.scrollHeight;
    }
   }
  }
 }
// ]]></script>
<iframe id="iframeid" src="kimi.php" height="1" width="100%" frameborder="0"></iframe>
跨域时Iframe高度自适应
在主页面和被嵌套的iframe为不同域名的时候，就稍微麻烦一些，需要避开JavaScript的跨域限制。

原理：现有iframe主页面main.html、被iframe嵌套页面iframe.html、iframe中介页面agent.html三个，通过main.html（域名为http://www.ccvita.com）嵌套iframe.html（域名为：http://www.phpq.net），当用户浏览时执行iframe.html中的JavaScript代码设置iframeC的scr地址中加入iframe页面的高度，agent.html（域名为：http://www.ccvita.com）取得传递的高度，通过JavaScript设置main.html中iframe的高度。最终实现预期的目标。

演示地址：http://www.ccvita.com/usr/uploads/demo/iframe/main.html
代码下载：http://www.ccvita.com/usr/uploads/demo/iframe/iframe.zip

iframe主页面main.html

&lt; !DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"&gt;
<div style="border: 1px solid #ccc; padding: 10px;"><iframe id="frame_content" name="frame_content" src="iframe.html" height="0" width="100%" frameborder="0" scrolling="no"></iframe></div>
尾部


iframe嵌套页面iframe.html

&lt; !DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"&gt;

文字

文字

文字

文字

<iframe id="iframeC" style="display: none;" name="iframeC" src="" height="0" width="0"></iframe>

<script type="text/javascript">// <![CDATA[

function sethash(){
    hashH = document.documentElement.scrollHeight; 
    urlC = "agent.html"; 
    document.getElementById("iframeC").src=urlC+"#"+hashH; 
}
window.onload=sethash;
// ]]></script>

iframe中介页面agent.html

&lt; !DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"&gt;

&nbsp;

<script type="text/javascript">// <![CDATA[
function  pseth() { 
    var iObj = parent.parent.document.getElementById('frame_content');
    iObjH = parent.parent.frames["frame_content"].frames["iframeC"].location.hash; 
    iObj.style.height = iObjH.split("#")[1]+"px"; 
}
pseth();
// ]]></script>

&nbsp;

UPDATE：长期以来一直有网友说方案不能跨域，今天我重新又测试了下，确定在IE6、IE7、IE8、IE9、Firefox全系列、Chrome全系列均可以成功跨域控制高度。请注意以下要点
第一，修改main.html文件中iframe的src地址为需要跨域的域名（比如ccvita.sinaapp.com）
第二，修改iframe.html文件中的urlC值为源域名（比如www.ccvita.com）这点最重要

&nbsp;

总结：

同域情况下：

&lt;script type="text/javascript"&gt;
//同域自适应高度处理
function SetCwinHeight(){
var iframeid=document.getElementById("mainf"); //iframe id
if (document.getElementById){
if (iframeid &amp;&amp; !window.opera){
if (iframeid.contentDocument &amp;&amp; iframeid.contentDocument.body.offsetHeight){
$(".main-app-box").css("height",iframeid.contentDocument.body.offsetHeight);
iframeid.height = iframeid.contentDocument.body.offsetHeight;
}else if(iframeid.Document &amp;&amp; iframeid.Document.body.scrollHeight){
$(".main-app-box").attr("height",iframeid.Document.body.offsetHeight);
iframeid.height = iframeid.Document.body.scrollHeight;
}
}
}
}
&lt;/script&gt;

&nbsp;

不同域情况下：

agent.html是放在main.html同域的。 iframe的跨域中要iframeagent.html
