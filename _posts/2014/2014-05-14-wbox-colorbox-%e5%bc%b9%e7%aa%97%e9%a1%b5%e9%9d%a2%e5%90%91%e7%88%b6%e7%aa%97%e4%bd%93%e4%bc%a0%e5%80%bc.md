---
layout: post
title: wbox /colorbox/ 弹窗页面向父窗体传值
date: 2014-05-14 13:53
author: admin
comments: true
categories: []
---
一：jquery wbox ——轻量级的弹出窗口jQuery插件，压缩后仅仅3.65Kb，基于jQuery1.4.2开发，主要实

现弹出框的效果，并且加入了很多有趣的功能，比如callback函数，显示隐藏层，Ajax页面，iframe嵌入

页面

$("#selectuser").wBox({
iframeWH: { width: 800

},
requestType: "iframe", // 此例在iframe里面使用
title: "选择司机",
target: "url"
});

二：从弹窗页面向父窗体传值，在a 标签注册 双击事件

&lt;a href="javascript:void(0)" id="Driver.UserId" ondblclick="selectDriverName('$!

{Driver.UserId}','$!{Driver.username}')"&gt; $!{Driver.username} &lt;/a&gt;

&lt;script type="text/javascript"&gt;
function selectDriverName(uid,dname) {

//将userid,驾驶员姓名传递到父窗体
parent.jQuery("#UserId").val(uid);
parent.jQuery("#DriverName").val(dname);

parent.$("#wBox").contents().find('.wBox_close').click();

}
&lt;/script&gt;

备注：关闭wbox 之前，将参数传递给父页面，可以通过parent.$("#pname").val($("#name").val()),如果是

新增的一个包括主键，需要后台先create 后，调用父页面的view里面的方法，将参数传递后，在拼接关

闭 wbox.
