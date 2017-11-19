---
layout: post
title: js 判断字符串是否包含某字符串,String对象中查找子字符,indexOf
date: 2015-08-13 18:00
author: admin
comments: true
categories: []
---
var Cts = "bblText";
 
if(Cts.indexOf("Text") > 0 )
{
    alert('Cts中包含Text字符串');
}
indexOf用法: 

返回 String 对象内第一次出现子字符串的字符位置。 
   
   strObj.indexOf(subString[, startIndex]) 
   
   参数 
   strObj 
   
   必选项。String 对象或文字。 
   
   subString 
   
   必选项。要在 String 对象中查找的子字符串。 
   
   starIndex 
   
   可选项。该整数值指出在 String 对象内开始查找的索引。如果省略，则从字符串的开始处查找。 
   
   说明 
   indexOf 方法返回一个整数值，指出 String 对象内子字符串的开始位置。如果没有找到子字符串，则返回 -1。 
   
   如果 startindex 是负数，则 startindex 被当作零。如果它比最大的字符位置索引还大，则它被当作最大的可能索引。 
   
   从左向右执行查找。否则，该方法与 lastIndexOf 相同。 
   
   示例 
   下面的示例说明了 indexOf 方法的用法。 
   
   function IndexDemo(str2){ 
    var str1 = "BABEBIBOBUBABEBIBOBU" 
    var s = str1.indexOf(str2); 
    return(s); 
   } 

对于JavaScript的indexOf忽略大小写 

JavaScript中indexOf函数方法返回一个整数值，指出 String 对象内子字符串的开始位置。如果没有找到子字符串，则返回 -1。如果 startindex 是负数，则 startindex 被当作零。如果它比最大的字符位置索引还大，则它被当作最大的可能索引。 

indexOf函数是从左向右执行查找。否则，该方法与 lastIndexOf 相同。 

下面的示例说明了indexOf函数方法的用法。

function IndexDemo(str2){
   var str1 = "BABEBIBOBUBABEBIBOBU"
   var s = str1.indexOf(str2);
   return(s);
}
