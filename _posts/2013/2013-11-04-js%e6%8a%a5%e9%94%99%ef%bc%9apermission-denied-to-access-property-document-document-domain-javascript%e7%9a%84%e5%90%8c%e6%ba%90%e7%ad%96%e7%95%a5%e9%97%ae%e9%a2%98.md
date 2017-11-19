---
layout: post
title: JS报错：Permission denied to access property 'document'---document.domain - JavaScript的同源策略问题
date: 2013-11-04 14:25
author: admin
comments: true
categories: []
---

错误原因是因为父窗口与子窗口的域名不同，即便同一站点下，也需要写全引用地址：如同是xxx.com下的页面<iframe id="iframe_xxxx" name="iframe_xxxx" src ="/sub.html" frameborder="0" height="300" width="1000"></iframe>与<iframe id="iframe_xxxx" name="iframe_xxxx" src ="http://www.xxx.com/sub.html" frameborder="0" height="300" width="1000"></iframe>是有区别的
笔者在做第三方集成时，使用了<iframe>，通过子页面的js来调整父页面的元素时，会出现访问不到的情况。
错误信息：Permission denied to access property 'document'
造成这个问题的原因是js不属于同一个域，由于某些浏览器的安全问题，所以被禁止访问了。
参考资料：https://developer.mozilla.org/Cn/JavaScript的同源策略（如不能访问，改https为http）
文件：http://sub.xxx.com/index.html

<!doctype html>
<html>
<head>
  ...
</head>
<body>
  ...
  <iframe id="iframe_xxxx" name="iframe_xxxx" src ="http://www.xxx.com/sub.html" frameborder="0" height="300" width="1000"></iframe>
</body>
</html>
 
 文件：http://www.xxx.com/sub.html

<!doctype html>
<html>
<head>
<script type="text/javascript">
//设置域信息
document.domain = 'xxx.com';
//设置父级页面引用自身的iframe高度
function setHeight(){
   //判断是否为顶级页面
   if(window.top!=window.self){
      parent.document.getElementById('iframe_xxxx').height=document.body.scrollHeight+20
   }
}
</script>
</head>
<body onload="setHeight();">
  ...
</body>
</html>
============================================================

这个问题是浏览器的安全机制造成的，
但是这种情况是同一域名下的一级子域名和二级子域名的区别，还是可以解决的。
解决方法就是把两个页面的域信息进行修改，变为相同的即可。
因为默认页面的域信息是包含二级域名的，这样设置可以统一使用顶级域名作为域信息。
<script type="text/javascript">
document.domain = 'xxx.com';
</script>
 
注：如果两个页面不属于同一个域名下，此方法不可行。强行设置js会报错。

补充：如果我们不确定引用的页面是否和被引用的页面所在域相同，可通过获取引用页面的url地址来判断
<script type="text/javascript">
//document.referrer - 可返回载入当前文档的文档的 URL。
$p_url = document.referrer;
</script>
//============================================================
