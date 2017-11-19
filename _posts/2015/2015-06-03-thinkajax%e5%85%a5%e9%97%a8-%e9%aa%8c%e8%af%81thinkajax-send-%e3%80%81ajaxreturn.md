---
layout: post
title: thinkajax入门------验证ThinkAjax.send 、ajaxReturn
date: 2015-06-03 10:32
author: admin
comments: true
categories: []
---
在动作模版页面添加代码：
<load href="__PUBLIC__/js/Base.js" /> 
<load href="__PUBLIC__/js/prototype.js" /> 
<load href="__PUBLIC__/js/mootools.js" /> 
<load href="__PUBLIC__/js/ThinkAjax.js" /> 
<!--<load href="__PUBLIC__/js/change.js" />  --> 
<script type="text/javascript"> 
function change(){ 
var ids = document.getElementByIdx_x("name").value; 
ThinkAjax.send("__URL__/fun","ajax=1&ids="+ids,'','result'); 
} 
</script>
         解释:
                     第一个参数：在控制器里面的函数名称
                     第二个参数：需要传递的参数ajax=1好像不可少
                     第三个参数：提交成功执行的函数名称
                     第四个参数：显示返回值的id
假设动作为：
<input type="text" id="name" />
<input type="submit" onclick="change()" />
<div id="result">这里是返回信息</div>
控制器action代码：
public function fun(){ 
  $ids = $_REQUEST['ids'];   //接受参数 也可以用post方法接受
$majoy = new Model("majoy");
$data = $majoy->getBycollege_id($ids);
if(!empty($data)){ 
   $this->ajaxReturn("","已经存在",1);    //此处可以用$this->error("已经存在"); 
}
}
        解释：第一个是返回的数据变量，
                   第二个是返回的信息，
                  第三个是数据返回的状态。为0即使失败，显示字体为红色，为1则代表成功，显示字体为绿色

