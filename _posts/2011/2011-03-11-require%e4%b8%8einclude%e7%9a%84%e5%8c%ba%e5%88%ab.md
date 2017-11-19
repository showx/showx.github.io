---
layout: post
title: require与include的区别
date: 2011-03-11 16:01
author: admin
comments: true
categories: []
---
<strong>include和require的区别</strong>

两者没有太大的区别，如果要包含的文件不存在，include提示notice，然后继续执行下面的语句，require提示致命错误并且退出，据我测试，win32平台下它们都是先包含后执行，所以被包含文件里最好不要再有include或require语句，这样会造成目录混乱。或许*nux下情况不同，暂时还没测试

如果一个文件不想被包含多次可以使用include_once或require_once## 读取，写入文档数据：?
<table border="1" cellspacing="0" cellpadding="2" width="400" align="center">
<tbody>
<tr>
<td class="code" bgcolor="#e6e6e6">
<pre>function r($file_name) { 
$filenum=@fopen($file_name,"r"); 
@flock($filenum,LOCK_SH); 
$file_data=@fread($filenum,filesize($file_name)); 
@fclose($filenum); 
return $file_data; 
} 
function w($file_name,$data,$method="w"){ 
$filenum=@fopen($file_name,$method); 
flock($filenum,LOCK_EX); 
$file_data=fwrite($filenum,$data); 
fclose($filenum); 
return $file_data; 
}</pre>
</td>
</tr>
</tbody>
</table>
