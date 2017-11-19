---
layout: post
title: jQuery对select操作小结
date: 2015-01-14 09:19
author: admin
comments: true
categories: []
---
//遍历option和添加、移除option
function changeShipMethod(shipping){
 var len = $("select[@name=ISHIPTYPE] option").length
 if(shipping.value != "CA"){
  $("select[@name=ISHIPTYPE] option").each(function(){
   if($(this).val() == 111){
    $(this).remove();
   }
  });
 }else{
  $("<option value='111'>UPS Ground</option>").appendTo($("select[@name=ISHIPTYPE]"));
 }
}


//取得下拉选单的选取值

$('#testSelect option:selected').text();
或$("#testSelect").find('option:selected').text();
或$("#testSelect").val();
//////////////////////////////////////////////////////////////////
记性不好的可以收藏下：
1,下拉框:

var cc1   = $(".formc select[@name='country'] option[@selected]").text(); //得到下拉菜单的选中项的文本(注意中间有空格)
var cc2 = $('.formc select[@name="country"]').val();   //得到下拉菜单的选中项的值
var cc3 = $('.formc select[@name="country"]').attr("id"); //得到下拉菜单的选中项的ID属性值
$("#select").empty();//清空下拉框//$("#select").html('');
$("<option value='1'>1111</option>").appendTo("#select")//添加下拉框的option

稍微解释一下:
1.select[@name='country'] option[@selected] 表示具有name 属性，
并且该属性值为'country' 的select元素 里面的具有selected 属性的option 元素；
可以看出有@开头的就表示后面跟的是属性。

2,单选框:
$("input[@type=radio][@checked]").val();   //得到单选框的选中项的值(注意中间没有空格)
$("input[@type=radio][@value=2]").attr("checked",'checked'); //设置单选框value=2的为选中状态.(注意中间没有空格)

3,复选框:
$("input[@type=checkbox][@checked]").val(); //得到复选框的选中的第一项的值
$("input[@type=checkbox][@checked]").each(function(){ //由于复选框一般选中的是多个,所以可以循环输出
   alert($(this).val());
   });

$("#chk1").attr("checked",'');//不打勾
$("#chk2").attr("checked",true);//打勾
if($("#chk1").attr('checked')==undefined){} //判断是否已经打勾


当然jquery的选择器是强大的. 还有很多方法.

<script src="jquery-1.2.1.js" type="text/javascript"></script>
<script language="javascript" type="text/javascript">
$(document).ready(function(){
$("#selectTest").change(function()
{
       //alert("Hello");
       //alert($("#selectTest").attr("name"));
       //$("a").attr("href","xx.html");
       //window.location.href="xx.html";
       //alert($("#selectTest").val());
       alert($("#selectTest option[@selected]").text());
       $("#selectTest").attr("value", "2");

});
});
</script>


<a href="#">aaass</a>

<!--下拉框-->
<select id="selectTest" name="selectTest">
<option value="1">11</option>
<option value="2">22</option>
<option value="3">33</option>
<option value="4">44</option>
<option value="5">55</option>
<option value="6">66</option>
</select>
jquery radio取值，checkbox取值，select取值，radio选中，checkbox选中，select选中，及其相关获取一组radio被选中项的值
var item = $('input[@name=items][@checked]').val();
获取select被选中项的文本
var item = $("select[@name=items] option[@selected]").text();
select下拉框的第二个元素为当前选中值
$('#select_id')[0].selectedIndex = 1;
radio单选组的第二个元素为当前选中值
$('input[@name=items]').get(1).checked = true;
获取值：
文本框，文本区域：$("#txt").attr("value")；
多选框checkbox：$("#checkbox_id").attr("value")；
单选组radio： $("input[@type=radio][@checked]").val();
下拉框select： $('#sel').val();
控制表单元素：
文本框，文本区域：$("#txt").attr("value",'');//清空内容
                $("#txt").attr("value",'11');//填充内容
多选框checkbox： $("#chk1").attr("checked",'');//不打勾
                $("#chk2").attr("checked",true);//打勾
                if($("#chk1").attr('checked')==undefined) //判断是否已经打勾
单选组radio： $("input[@type=radio]").attr("checked",'2');//设置value=2的项目为当前选中项
下拉框select： $("#sel").attr("value",'-sel3');//设置value=-sel3的项目为当前选中项
            $("<optionvalue='1'>1111</option><optionvalue='2'> 2222</option>").appendTo("#sel")//添加下拉框的option
            $("#sel").empty()；//清空下拉框

获取一组radio被选中项的值
var item = $('input[@name=items][@checked]').val();
获取select被选中项的文本
var item = $("select[@name=items] option[@selected]").text();
select下拉框的第二个元素为当前选中值
$('#select_id')[0].selectedIndex = 1;
radio单选组的第二个元素为当前选中值
$('input[@name=items]').get(1).checked = true;
获取值：
文本框，文本区域：$("#txt").attr("value")；
多选框checkbox：$("#checkbox_id").attr("value")；
单选组radio： $("input[@type=radio][@checked]").val();
下拉框select： $('#sel').val();
控制表单元素：
文本框，文本区域：$("#txt").attr("value",'');//清空内容
$("#txt").attr("value",'11');//填充内容
多选框checkbox： $("#chk1").attr("checked",'');//不打勾
$("#chk2").attr("checked",true);//打勾
if($("#chk1").attr('checked')==undefined) //判断是否已经打勾
单选组radio： $("input[@type=radio]").attr("checked",'2');//设置value=2的项目为当前选中项
下拉框select： $("#sel").attr("value",'-sel3');//设置value=-sel3的项目为当前选中项
$("<option value='1'>1111</option><option value='2'>2222</option>").appendTo("#sel")//添加下拉框的option
$("#sel").empty()；//清空下拉框
