---
layout: post
title: PHP中$_FILES的使用及注意事项
date: 2015-03-01 09:47
author: admin
comments: true
categories: []
---
$_FILES: 经由 HTTP POST 文件上传而提交至脚本的变量。类似于旧数组 $HTTP_POST_FILES 数组（依然有效，但反对使用）。详细信息可参阅 POST 方法上传。
$_FILES数组内容如下:
<wbr />$_FILES['myFile']['name'] <wbr /> <wbr /> 客户端文件的原名称。
<wbr />$_FILES['myFile']['type'] <wbr /> <wbr /> 文件的 MIME 类型，需要浏览器提供该信息的支持，例如"image/gif"。
<wbr />$_FILES['myFile']['size'] <wbr /> <wbr /> 已上传文件的大小，单位为字节。
<wbr />$_FILES['myFile']['tmp_name'] <wbr /> <wbr /> 文件被上传后在服务端储存的临时文件名，一般是系统默认。可以在php.ini的upload_tmp_dir 指定，但用 putenv() 函数设置是不起作用的。
<wbr />$_FILES['myFile']['error'] <wbr /> <wbr /> 和该文件上传相关的错误代码。['error'] 是在 PHP 4.2.0 版本中增加的。下面是它的说明：(它们在PHP3.0以后成了常量)
<wbr /> <wbr />UPLOAD_ERR_OK <wbr /> <wbr /> <wbr /> <wbr /> <wbr /> <wbr /> <wbr /> <wbr /> <wbr /> <wbr /> <wbr /> <wbr /> 值：0; 没有错误发生，文件上传成功。
<wbr /> <wbr />UPLOAD_ERR_INI_SIZE <wbr /> <wbr /> <wbr /> <wbr /> <wbr /> 值：1; 上传的文件超过了 php.ini 中 upload_max_filesize 选项限制的值。
<wbr /> <wbr />UPLOAD_ERR_FORM_SIZE <wbr /> <wbr />值：2; 上传文件的大小超过了 HTML 表单中 MAX_FILE_SIZE 选项指定的值。
<wbr /> <wbr />UPLOAD_ERR_PARTIAL <wbr /> <wbr /> <wbr /> <wbr /> <wbr /> <wbr /> <wbr /> <wbr /> <wbr /> 值：3; 文件只有部分被上传。
<wbr /> <wbr />UPLOAD_ERR_NO_FILE <wbr /> <wbr /> <wbr /> <wbr /> <wbr /> <wbr /> <wbr /> <wbr /> <wbr /> 值：4; 没有文件被上传。 <wbr /> <wbr /> <wbr /> 值：5; 上传文件大小为0.
<wbr />
注:
1. 文件被上传结束后，默认地被存储在了临时目录中，这时必须将它从临时目录中删除或移动到其它地方，如果没有，则会被删除。也就是不管是否上传成功，脚本执行完后临时目录里的文件肯定会被删除。所以在删除之前要用PHP的 copy() 函数将它复制到其它位置，此时，才算完成了上传文件过程。
2. 在 PHP 4.1.0 版本以前该数组的名称为 $HTTP_POST_FILES，它并不像 $_FILES 一样是自动全局变量。PHP 3 不支持 $HTTP_POST_FILES 数组。
3. 用form上传文件时，一定要加上属性内容 enctype="multipart/form-data"，否则用$_FILES[filename]获取文件信息时会报异常。
&lt;form enctype="multipart/form-data" action="URL" method="post"&gt;
<wbr />&lt;input name="myFile" type="file"&gt;
<wbr />&lt;input type="submit" value="上传文件"&gt;
&lt;/form&gt;
