---
layout: post
title: PHP写入文件用file_put_contents代替fwrite优点多多
date: 2015-07-23 08:46
author: admin
comments: true
categories: []
---
使用php有一段时间了，之前一直用fwrite写入文件，不过当我知道file_put_contents这个函数之后，fwrite就比较少用了，file_put_contents比fwrite代码更简洁。具体来说，fwrite至少要3行代码完成一次写入时间，而file_put_contents只需要一行代码即可！

如下为file_put_contents的实例代码：
<?php
$filename = 'file.txt';
$word = "你好!\r\nwebkaka";  //双引号会换行 单引号不换行
file_put_contents($filename, $word);
 ?>
复制代码
同样的功能使用fwrite的实例代码：
<?php
$filename = 'file.txt';
$word = "你好!\r\nwebkaka";  //双引号会换行  单引号不换行
$fh = fopen($filename, "w"); //w从开头写入 a追加写入
echo fwrite($fh, $word);
fclose($fh);
 ?>
复制代码
从以上两个例子看出，其实file_put_contents是fopen、fwrite、fclose三合一的简化写法，这对程序代码的优化是有好处的，一方面在代码量上有所减少，另一方面不会出现fclose漏写的不严密代码，在调试、维护上方便很多。

上述例子里，file_put_contents是从头写入，如果要追加写入，怎么办呢？

在file_put_contents的语法里，有个参数FILE_APPEND，这是追加写入的声明。实例代码如下：
<?php
echo file_put_contents('file.txt', "This is another something.", FILE_APPEND);
 ?>
复制代码
FILE_APPEND就是追加写入的声明。在追加写入时，为了避免其他人同时操作，往往需要锁定文件，这时需要加多一个LOCK_EX的声明，写法如下：
<?php
echo file_put_contents('file.txt', "This is another something.", FILE_APPEND|LOCK_EX);
 ?>
复制代码
注意，以上代码中echo输出到显示器里的是写入文件字符串的长度。



常见问题：

Warning: fopen(file.txt) [function.fopen]: failed to open stream: Permission denied

当写入文件时，有时会遇到上述问题，这是因为文件没有写权限的原因。为了避免这个错误的出现，在写入文件时需要判断下文件是否可写，这需要用到is_writable()这个函数。实例代码如下：
<?php
$filename = 'file.txt';
 if (is_writable($filename)) {
echo file_put_contents($filename, "This is another something.", FILE_APPEND);
 } else {
    echo "文件 $filename 不可写";
复制代码
}
?>
