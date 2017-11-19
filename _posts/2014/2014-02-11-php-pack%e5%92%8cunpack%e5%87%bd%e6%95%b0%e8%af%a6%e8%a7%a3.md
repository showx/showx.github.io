---
layout: post
title: PHP pack和unpack函数详解
date: 2014-02-11 17:04
author: admin
comments: true
categories: []
---
pack
压缩资料到位字符串之中。
语法: string pack(string format, mixed [args]...);
返回值: 字符串
函数种类: 资料处理
内容说明
本函数用来将资料压缩打包到位的字符串之中。本函数和 Perl 的同名函数功能用法完全相同。参数 format 为压缩的格式，见下表
a 将字符串空白以 NULL 字符填满 A 将字符串空白以 SPACE 字符 (空格) 填满 h 十六进位字符串，低位在前 H 十六进位字符串，高位在前 c 有号字符 C 无号字符 s 有号短整数 (十六位，依计算机的位顺序) S 无号短整数 (十六位，依计算机的位顺序) n 无号短整数 (十六位, 高位在后的顺序) v 无号短整数 (十六位, 低位在后的顺序) i 有号整数 (依计算机的顺序及范围) I 无号整数 (依计算机的顺序及范围) l 有号长整数 (卅二位，依计算机的位顺序) L 无号长整数 (卅二位，依计算机的位顺序) N 无号短整数 (卅二位, 高位在后的顺序) V 无号短整数 (卅二位, 低位在后的顺序) f 单精确浮点数 (依计算机的范围) d 倍精确浮点数 (依计算机的范围) x 空位 X 倒回一位 @ 填入 NULL 字符到绝对位置
例子 1
<?php
echo pack("C3",80,72,80);
?>
输出：
PHP
例子 2
<?php
echo pack("C*",80,72,80);
?>
输出：
PHP
unpack
解压缩位字符串资料。
语法: string pack(string format, mixed [args]...);
返回值: 数组
函数种类: 资料处理
内容说明
本函数用来将位的字符串的资料解压缩。本函数和 Perl 的同名函数功能用法完全相同。参数 format 为压缩的格式，参见 pack 的格式表。
例子 1
<?php
$data = "PHP";
print_r(unpack("C*",$data));
?>
输出：
Array
(
[1] => 80
[2] => 72
[3] => 80
)
例子 2
<?php
$data = "PHP";
print_r(unpack("C*myint",$data));
?>
输出：
Array
(
[myint1] => 80
[myint2] => 72
[myint3] => 80
)
例子 3
<?php
$bin = pack("c2n2",0x1234,0x5678,65,66);
print_r(unpack("c2chars/n2int",$bin));
?>
输出：
Array
(
[chars1] => 52
[chars2] => 120
[int1] => 65
[int2] => 66
)
玩了玩 PHP 的 pack/unpack ！还是蛮简单的
2007-07-04 16:34
这几天要做双向的加解密，因为加密后的结果都是二进制的，不得不了解一下它了
小try了一把pack/unpack，基本用法还是蛮简单的，下面的例子基本上是给了个做数据库的原型（嘻嘻，不能用PHP5了，只好用PHP4的语法写）
<?php
/**
* 这是一个测试pack/unpack操作的东西
* @author 张心灵
* Email: zhangsilly@gmail.com
*  
* 一下文件存储如下几个字段
* 姓名: 长度 8 字节
* 年龄: unsigned int
* Email: 长度 30 字节
*/
class Person_Data
{
     /**
      * 数据库文件路径
      *
      * @var string
      */
     var $_database = './wps.db';
     /**
      * 打开一个数据库文件
      *
      * @param string $file     数据库文件名
      */
     function openDb($file = './wps.db')
     {
         $this->_database = $file;
         $this->_database = fopen($this->_database, 'ab+');
     }
     /**
      * 向数据库中写入一条记录
      *
      * @param array $data    PHP4真的丑死了
      * @return void
      */
     function writeRecord($data)
     {
         $name = pack('a8', $data['name']);
         $age = pack('S', $data['age']);
         $email = pack('a30', $data['email']);
         fwrite($this->_database, $name . $age . $email);
     }
     /**
      * 读取一条记录
      *
      * @param int $count optional default to 0   记录id数
      * @return array
      */
     function read($count = 0)
     {
         rewind($this->_database);
         fseek($this->_database, 40 * $count);
         $return = array();
         $return['name'] = unpack('a8', fread($this->_database, 8));
         $return['name'] = $return['name'][1];
         $return['age'] = unpack('S', fread($this->_database, 2));
         $return['age'] = $return['age'][1];
         $return['email'] = unpack('a30', fread($this->_database, 30));
         $return['email'] = $return['email'][1];
         return $return;
     }
}
$me = array(     'name' => '张心灵',  
                 'age'   => 23,
                 'email' => 'zhangsilly@gmail.com');
$data = new Person_Data();
$data->openDb('./wps.db');
//$data->writeRecord($me);
print_r($data->read(1));
以上文件运行了两侧，写入了两行记录，最后我读取第二行记录（索引自然从 1 开始）
运行一切正常：
Array
(
     [name] => 张心灵
     [age] => 23
     [email] => zhangsilly@gmail.com
)
双就一个字！
翻译：
pack()函数的作用是：将数据压缩成一个二进制字符串。
a - NUL-padded string
a - NUL- 字符串填满[padded string]
A - SPACE-padded string
A - SPACE- 字符串填满[padded string]
h - Hex string, low nibble first
h – 十六进制字符串，低“四位元”[low nibble first]
H - Hex string, high nibble first
H - 十六进制字符串，高“四位元”[high nibble first]
c - signed char
c – 带有符号的字符
C - unsigned char
C – 不带有符号的字符
s - signed short (always 16 bit, machine byte order)
s – 带有符号的短模式[short]（通常是16位，按机器字节顺序）
S - unsigned short (always 16 bit, machine byte order)
S – 不带有符号的短模式[short]（通常是16位，按机器字节排序）
n - unsigned short (always 16 bit, big endian byte order)
n -不带有符号的短模式[short]（通常是16位，按大endian字节排序）
v - unsigned short (always 16 bit, little endian byte order)
v -不带有符号的短模式[short]（通常是16位，按小endian字节排序）
i - signed integer (machine dependent size and byte order)
i – 带有符号的整数（由大小和字节顺序决定）
I - unsigned integer (machine dependent size and byte order)
I – 不带有符号的整数（由大小和字节顺序决定）
l - signed long (always 32 bit, machine byte order)
l– 带有符号的长模式[long]（通常是32位，按机器字节顺序）
L - unsigned long (always 32 bit, machine byte order)
L – 不带有符号的长模式[long]（通常是32位，按机器字节顺序）
N - unsigned long (always 32 bit, big endian byte order)
N – 不带有符号的长模式[long]（通常是32位，按大edian字节顺序）
V - unsigned long (always 32 bit, little endian byte order)
V– 不带有符号的长模式[long]（通常是32位，按小edian字节顺序）
f - float (machine dependent size and representation)
f –浮点（由大小和字节顺序决定）
d - double (machine dependent size and representation)
d – 双精度（由大小和字节顺序决定）
x - NUL byte
x – 空字节[NUL byte]
X - Back up one byte
X- 后面一个字节[Back up one byte]
@ - NUL-fill to absolute position
@ - NUL- 添加到一个绝对位置[absolute position]
php里的pack及unpack--转
2009-03-15 22:11
用了很久php了却很少有机会用php进行一些二进制操作。 最近用php写一个socket客户端连接一个用C++语言开发的游戏服务端。 服务器端开发人员使用了二进制的形式来定义协议的格式。协议格式如下：
   包头(2bytes)+加密(1byte)+命令码(2bytes)+帧内容
1.包头的内容是记录帧内容的长度;
2. 加密:0表示不加密，1表示加密；
3. 命令码为服务端命令识别符号；
    一开始不了解php原来有pack可以来组装二进制包, 走了弯路，让服务端开发人员用C语言帮忙开发了的几个内存操作函数，按照协议规则返回二进制包，然后我将这几个方法编译成一组扩展函数供php使用。
   
    话归正题，本文是介绍如何使用pack和unpack这两个方法的。php官方手册举例太少，不能很容易理解，特别是那些格式化参数的使用。
转摘的参数中文说明：
pack/unpack 的摸板字符字符 含义
a 一个填充空的字节串
A 一个填充空格的字节串
b 一个位串，在每个字节里位的顺序都是升序
B 一个位串，在每个字节里位的顺序都是降序
c 一个有符号 char（8位整数）值
C 一个无符号 char（8位整数）值；关于 Unicode 参阅 U
d 本机格式的双精度浮点数
f 本机格式的单精度浮点数
h 一个十六进制串，低四位在前
H 一个十六进制串，高四位在前
i 一个有符号整数值，本机格式
I 一个无符号整数值，本机格式
l 一个有符号长整形，总是 32 位
L 一个无符号长整形，总是 32 位
n 一个 16位短整形，“网络”字节序（大头在前）
N 一个 32 位短整形，“网络”字节序（大头在前）
p 一个指向空结尾的字串的指针
P 一个指向定长字串的指针
q 一个有符号四倍（64位整数）值
Q 一个无符号四倍（64位整数）值
s 一个有符号短整数值，总是 16 位
S 一个无符号短整数值，总是 16 位，字节序跟机器芯片有关
u 一个无编码的字串
U 一个 Unicode 字符数字
v 一个“VAX”字节序（小头在前）的 16 位短整数
V 一个“VAX”字节序（小头在前）的 32 位短整数
w 一个 BER 压缩的整数
x 一个空字节（向前忽略一个字节）
X 备份一个字节
Z 一个空结束的（和空填充的）字节串
@ 用空字节填充绝对位置
string pack ( string $format [, mixed $args [, mixed $...]] )
一些规则：
1.每个字母后面都可以跟着一个数字，表示 count（计数），如果 count 是一个 * 表示剩下的所有东西。
2.如果你提供的参数比 $format 要求的少，pack 假设缺的都是空值。如果你提供的参数比 $format 要求的多，那么多余的参数被忽略。
下面还是用例子来说明用法会容易理解一点：
关于Pack:
下面的第一部分把数字值包装成字节：
$out = pack("CCCC", 65, 66, 67, 68);      # $out 等于"ABCD"
$out = pack("C4", 65, 66, 67, 68);         # 一样的东西
下面的对 Unicode 的循环字母做同样的事情：
$foo = pack("U4", 0x24b6, 0x24b7, 0x24b8, 0x24b9);
下面的做类似的事情，增加了一些空：
$out = pack("CCxxCC", 65, 66, 67, 68);      # $out 等于 "AB\0\0CD"
打包你的短整数并不意味着你就可移植了：
$out = pack("s2", 1, 2);        
# 在小头在前的机器上是 "\1\0\2\0"
# 在大头在前的机器上是 "\0\1\0\2"
在二进制和十六进制包装上，count 指的是位或者半字节的数量，而不是生成的字节数量：
$out = pack("B32", "...");
    $out = pack("H8", "5065726c");         # 都生成“Perl”
a 域里的长度只应用于一个字串：
$out = pack("a4", "abcd", "x", "y", "z");      # "abcd"
要绕开这个限制，使用多倍声明：
$out = pack("aaaa",    "abcd", "x", "y", "z");   # "axyz"
   $out = pack("a" x 4,   "abcd", "x", "y", "z");   # "axyz"
a 格式做空填充：
$out = pack("a14", "abcdefg");         # " abcdefg\0\0\0\0\0\0"
关于unpack:
array unpack ( string $format, string $data )
$data = "010000020007";
unpack("Sint1/Cchar1/Sint2/Cchar2",$data);
## array('int1'=&gt;1, 'char1'=&gt;'0','int2'=&gt;2,'char2'=&gt;7);
      最后本文开头讲到的协议使用pack/unpack 举例程序代码为 ：
$lastact   = pack('SCSa32a32',0x0040, 0x00, 0x0006, $username, $passwd )；
unpack('Sint1/Cchar1/Sint2/Cchar2/',$lastmessage)；
