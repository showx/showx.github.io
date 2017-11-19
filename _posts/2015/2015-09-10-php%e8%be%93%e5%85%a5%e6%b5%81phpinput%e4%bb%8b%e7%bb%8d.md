---
layout: post
title: PHP输入流php://input介绍
date: 2015-09-10 13:40
author: admin
comments: true
categories: []
---
对一php://input介绍，PHP官方手册文档有一段话对它进行了很明确地概述。

“php://input allows you to read raw POST data. It is a less memory intensive alternative to $HTTP_RAW_POST_DATA and does not need any special php.ini directives. php://input is not available with enctype=”multipart/form-data”.
翻译过来，是这样：
“php://input可以读取没有处理过的POST数据。相较于$HTTP_RAW_POST_DATA而言，它给内存带来的压力较小，并且不需要特殊的php.ini设置。php://input不能用于enctype=multipart/form-data”

我们应该怎么去理解这段概述呢?!我把它划分为三部分，逐步去理解。
读取POST数据
不能用于multipart/form-data类型
php://input VS $HTTP_RAW_POST_DATA
读取POST数据

PHPer们一定很熟悉$_POST这个内置变量。$_POST与 php://input存在哪些关联与区别呢?另外，客户端向服务端交互数据，最常用的方法除了POST之外，还有GET。既然php://input作 为PHP输入流，它能读取GET数据吗？这二个问题正是我们这节需要探讨的主要内容。
经验告诉我们，从测试与观察中总结，会是一个很凑效的方法。这里，我写了几个脚本来帮助我们测试。

@file 192.168.0.6:/phpinput_server.php 打印出接收到的数据
@file 192.168.0.8:/phpinput_post.php 模拟以POST方法提交表单数据
@file 192.168.0.8:/phpinput_xmlrpc.php 模拟以POST方法发出xmlrpc请求.
@file 192.168.0.8:/phpinput_get.php 模拟以GET方法提交表单表数
phpinput_server.php与phpinput_post.php



&lt;?php
//@file phpinput_server.php
$raw_post_data = file_get_contents('php://input', 'r');
echo "-------\$_POST------------------\n";
echo var_dump($_POST) . "\n";
echo "-------php://input-------------\n";
echo $raw_post_data . "\n";
?&gt;

&lt;?php
//@file phpinput_post.php
$http_entity_body = 'n=' . urldecode('perfgeeks') . '&amp;p=' . urldecode('7788');
$http_entity_type = 'application/x-www-form-urlencoded';
$http_entity_length = strlen($http_entity_body);
$host = '192.168.0.6';
$port = 80;
$path = '/phpinput_server.php';
$fp = fsockopen($host, $port, $error_no, $error_desc, 30);
if ($fp) {
fputs($fp, "POST {$path} HTTP/1.1\r\n");
fputs($fp, "Host: {$host}\r\n");
fputs($fp, "Content-Type: {$http_entity_type}\r\n");
fputs($fp, "Content-Length: {$http_entity_length}\r\n");
fputs($fp, "Connection: close\r\n\r\n");
fputs($fp, $http_entity_body . "\r\n\r\n");

while (!feof($fp)) {
$d .= fgets($fp, 4096);
}
fclose($fp);
echo $d;
}
?&gt;
我们可以通过使用工具ngrep抓取http请求包（因为我们需要探知的是php://input，所以我们这里只抓取http Request数据包）。我们来执行测试脚本phpinput_post.php

@php /phpinput_post.php
HTTP/1.1 200 OK
Date: Thu, 08 Apr 2010 03:23:36 GMT
Server: Apache/2.2.3 (CentOS)
X-Powered-By: PHP/5.1.6
Content-Length: 160
Connection: close
Content-Type: text/html; charset=UTF-8
-------$_POST------------------
array(2) {
["n"]=&gt; string(9) "perfgeeks"
["p"]=&gt; string(4) "7788"
}
-------php://input-------------
n=perfgeeks&amp;p=7788
通过ngrep抓到的http请求包如下:

T 192.168.0.8:57846 -&gt; 192.168.0.6:80 [AP]
POST /phpinput_server.php HTTP/1.1..
Host: 192.168.0.6..Content-Type: application/x-www-form-urlencoded..Co
ntent-Length: 18..Connection: close....n=perfgeeks&amp;p=7788....
仔细观察，我们不难发现
1，$_POST数据,php://input 数据与httpd entity body数据是“一致”的
2，http请求中的Content-Type是application/x-www-form-urlencoded ，它表示http请求body中的数据是使用http的post方法提交的表单数据，并且进行了urlencode()处理。
(注:注意加粗部分内容，下文不再提示).

我们再来看看脚本phpinput_xmlrpc.php的原文件内容，它模拟了一个POST方法提交的xml-rpc请求。



&lt;?php
//@file phpinput_xmlrpc.php
$http_entity_body = "\n\n jt_userinfo\n";
$http_entity_type = 'text/html';
$http_entity_length = strlen($http_entity_body);
$host = '192.168.0.6';
$port = 80;
$path = '/phpinput_server.php';
$fp = fsockopen($host, $port, $error_no, $error_desc, 30);
if ($fp) {
fputs($fp, "POST {$path} HTTP/1.1\r\n");
fputs($fp, "Host: {$host}\r\n");
fputs($fp, "Content-Type: {$http_entity_type}\r\n");
fputs($fp, "Content-Length: {$http_entity_length}\r\n");
fputs($fp, "Connection: close\r\n\r\n");
fputs($fp, $http_entity_body . "\r\n\r\n");
while (!feof($fp)) {
$d .= fgets($fp, 4096);
}

fclose($fp);
echo $d;
}
?&gt;
同样地，让我们来执行这个测试脚本

@php /phpinput_xmlrcp.php
HTTP/1.1 200 OK
Date: Thu, 08 Apr 2010 03:47:18 GMT
Server: Apache/2.2.3 (CentOS)
X-Powered-By: PHP/5.1.6
Content-Length: 154
Connection: close
Content-Type: text/html; charset=UTF-8

-------$_POST------------------
array(0) {
}

-------php://input-------------
&lt;?xml version="1.0"&gt;
&lt;methodcall&gt;
&lt;name&gt;jt_userinfo&lt;/name&gt;
&lt;/methodcall&gt;
执行这个脚本的时候，我们通过ngrep抓取的http请求数据包如下

T 192.168.0.8:45570 -&gt; 192.168.0.6:80 [AP]
POST /phpinput_server.php HTTP/1.1..
Host: 192.168.0.6..Content-Type: text/html..Content-Length: 75..Connec
tion: close....&lt;?xml version="1.0"&gt;.&lt;methodcall&gt;. &lt;name&gt;jt_userinfo&lt;
/name&gt;.&lt;/methodcall&gt;....
同样，我样也可以很容易地发现:
1，http请求中的Content-Type是text/xml。它表示http请求中的body数据是xml数据格式。
2，服务端$_POST打印出来的是一个空数组，即与http entity body不一致了。这跟上个例子不一样了，这里的Content-Type是text/xml,而不是application/x-www-form-urlencoded
3，而php://input数据还是跟http entity body数据一致。也就是php://input数据和$_POST数据不一致了。

我们再来看看通过GET方法提交表单数据的情况，php://input能不能读取到GET方法的表单数据？在这里，我们稍加改动一下phpinput_server.php文件，将$_POST改成$_GET。
<div class="codetitle"><a id="copybut95495" class="copybut"></a><u>复制代码</u>代码如下:</div>
<div id="code95495" class="codebody">
&lt;?php
//@file phpinput_server.php
$raw_post_data = file_get_contents('php://input', 'r');
echo "-------\$_GET------------------\n";
echo var_dump($_GET) . "\n";
echo "-------php://input-------------\n";
echo $raw_post_data . "\n";
?&gt;
&lt;?php
//@file phpinput_get.php
$query_path = 'n=' . urldecode('perfgeeks') . '&amp;p=' . urldecode('7788');
$host = '192.168.0.6';
$port = 80;
$path = '/phpinput_server.php';
$d = '';
$fp = fsockopen($host, $port, $error_no, $error_desc, 30);
if ($fp) {
fputs($fp, "GET {$path}?{$query_path} HTTP/1.1\r\n");
fputs($fp, "Host: {$host}\r\n");
fputs($fp, "Connection: close\r\n\r\n");

while (!feof($fp)) {
$d .= fgets($fp, 4096);
}
fclose($fp);
echo $d;
}
?&gt;</div>
同样，我们执行下一phpinput_get.php测试脚本，它模拟了一个通常情况下的GET方法提交表单数据。

@php /phpinput_get.php
HTTP/1.1 200 OK
Date: Thu, 08 Apr 2010 07:38:15 GMT
Server: Apache/2.2.3 (CentOS)
X-Powered-By: PHP/5.1.6
Content-Length: 141
Connection: close
Content-Type: text/html; charset=UTF-8

-------$_GET------------------
array(2) {
["n"]=&gt;
string(9) "perfgeeks"
["p"]=&gt;
string(4) "7788"
}

-------php://input-------------
在这个时候，使用ngrep工具，捕获的相应的http请求数据包如下

T 192.168.0.8:36775 -&gt; 192.168.0.6:80 [AP]
GET /phpinput_server.php?n=perfgeeks&amp;p=7788 HTTP/1.1..
Host: 192.168.0.6..Connection: close....
比较POST方法提交的http请求，通常GET方法提交的请求中，entity body为空。同时，不会指定Content-Type和Content-Length。但是，如果强硬数据http entity body，并指明正确地Content-Type和Content-Length,那么php://input还可是读取得到http entity body数据，但不是$_GET数据。

所根据，上面几个探测，我们可以作出以下总结:
1，Content- Type取值为application/x-www-form-urlencoded时，php会将http请求body相应数据会填入到数 组$_POST，填入到$_POST数组中的数据是进行urldecode()解析的结果。（其实，除了该Content-Type，还有 multipart/form-data表示数据是表单数据，稍后我们介绍）
2，php://input数据，只要Content-Type不为 multipart/form-data(该条件限制稍后会介绍)。那么php://input数据与http entity body部分数据是一致的。该部分相一致的数据的长度由Content-Length指定。
3，仅当Content-Type为application/x-www-form-urlencoded且提交方法是POST方法时，$_POST数据与php://input数据才是”一致”（打上引号，表示它们格式不一致，内容一致）的。其它情况，它们都不一致。
4，php://input读取不到$_GET数据。是因为$_GET数据作为query_path写在http请求头部(header)的PATH字段，而不是写在http请求的body部分。

这也帮助我们理解了，为什么xml_rpc服务端读取数据都是通过file_get_contents(‘php://input', ‘r')。而不是从$_POST中读取，正是因为xml_rpc数据规格是xml，它的Content-Type是text/xml。
php://input碰到了multipart/form-data

上传文件的时候，表单的写法是这样的
<div class="codetitle"><a id="copybut87209" class="copybut"></a><u>复制代码</u>代码如下:</div>
<div id="code87209" class="codebody">
&lt;form enctype="multipart/form-data" action="phpinput_server.php" method="POST" &gt;
&lt;input type="text" name="n" /&gt;
&lt;input type="file" name="f" /&gt;
&lt;input type="submit" value="upload now" /&gt;
&lt;/form&gt;</div>
那么，enctype=multipart/form-data这里的意义，就是将该次http请求头部(head)中的Content-Type设置为multipart/form-data。请查阅RFC1867对 它的描述。multipart/form-data也表示以POST方法提交表单数据，它还伴随了文件上传，所以会跟application/x- www-form-urlencoded数据格式不一样。它会以一更种更合理的，更高效的数据格式传递给服务端。我们提交该表单数据，并且打印出响应结 果，如下:

-------$_POST------------------
array(1) { ["n"]=&gt; string(9) "perfgeeks" }
-------php://input-------------
同时，我们通过ngrep抓取的相应的http请求数据包如下:

########
T 192.168.0.8:3981 -&gt; 192.168.0.6:80 [AP]
POST /phpinput_server.php HTTP/1.1..Host: 192.168.0.6..Connection: kee
p-alive..User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US) A
ppleWebKit/533.2 (KHTML, like Gecko) Chrome/5.0.342.3 Safari/533.2..Re
ferer: http://192.168.0.6/phpinput_server.php..Content-Length: 306..Ca
che-Control: max-age=0..Origin: http://192.168.0.6..Content-Type: mult
ipart/form-data; boundary=----WebKitFormBoundarybLQwkp4opIEZn1fA..Acce
pt: application/xml,application/xhtml+xml,text/html;q=0.9,text/plain;q
=0.8,image/png,*/*;q=0.5..Accept-Encoding: gzip,deflate,sdch..Accept-L
anguage: zh-CN,zh;q=0.8..Accept-Charset: GBK,utf-8;q=0.7,*;q=0.3..Cook
ie: SESS3b0e658f87cf58240de13ab43a399df6=lju6o5bg8u04lv1ojugm2ccic6...
.
##
T 192.168.0.8:3981 -&gt; 192.168.0.6:80 [AP]
------WebKitFormBoundarybLQwkp4opIEZn1fA..Content-Disposition: form-da
ta; name="n"....perfgeeks..------WebKitFormBoundarybLQwkp4opIEZn1fA..C
ontent-Disposition: form-data; name="f"; filename="test.txt"..Content-
Type: text/plain....i am file..multipart/form-data..------WebKitFormBo
undarybLQwkp4opIEZn1fA--..
##
从响应输出来比对，$_POST数据跟请求提交数据相符，即$_POST = array(‘n' =&gt; ‘perfgeeks')。这也跟http请求body中的数据相呼应，同时说明PHP把相应的数据填入$_POST全局变量。而php://input 输出为空，没有输出任何东西，尽管http请求数据包中body不为空。这表示，当Content-Type为multipart/form-data的 时候，即便http请求body中存在数据，php://input也为空，PHP此时，不会把数据填入php://input流。所以，可以确定: php://input不能用于读取enctype=multipart/form-data数据。

我们再比较这次通过ngrep抓取的http请求数据包，我们会发现，最大不同的一 点是Content-Type后面跟了boundary定义了数据的分界符，bounday是随机生成的。另外一个大不一样的，就是http entity body中的数据组织结构不一样了。

上一节，我们概述了，当Content-Type为application/x- www-form-urlencoded时，php://input和$_POST数据是“一致”的，为其它Content-Type的时候，php: //input和$_POST数据数据是不一致的。因为只有在Content-Type为application/x-www-form- urlencoded或者为multipart/form-data的时候，PHP才会将http请求数据包中的body相应部分数据填入$_POST全 局变量中,其它情况PHP都忽略。而php://input除了在数据类型为multipart/form-data之外为空外，其它情况都可能不为空。 通过这一节，我们更加明白了php://input与$_POST的区别与联系。所以，再次确认，php://input无法读取 enctype=multipart/form-data数据，当php://input遇到它时，永远为空，即便http entity body有数据。
php://input VS $http_raw_post_data

相信大家对php://input已经有一定深度地了解了。那 么$http_raw_post_data是什么呢？$http_raw_post_data是PHP内置的一个全局变量。它用于，PHP在无法识别的 Content-Type的情况下，将POST过来的数据原样地填入变量$http_raw_post_data。它同样无法读取Content- Type为multipart/form-data的POST数据。需要设置php.ini中的 always_populate_raw_post_data值为On，PHP才会总把POST数据填入变量$http_raw_post_data。

把脚本phpinput_server.php改变一下，可以验证上述内容
<div class="codetitle"><a id="copybut65183" class="copybut"></a><u>复制代码</u>代码如下:</div>
<div id="code65183" class="codebody">
&lt;?php
$raw_post_data = file_get_contents('php://input', 'r');
$rtn = ($raw_post_data == $HTTP_RAW_POST_DATA) ? 1 : 0;
echo $rtn;
?&gt;</div>
执行测试脚本

@php phpinput_post.php
@php phpinput_get.php
@php phpinput_xmlrpc.php

得出的结果输出都是一样的，即都为1，表示php://input和$HTTP_RAW_POST_DATA是相同的。至于对内存的压力，我们这里就不做细致地测试了。有兴趣的，可以通过xhprof进行测试和观察。

以此，我们这节可以总结如下:
1, php://input 可以读取http entity body中指定长度的值,由Content-Length指定长度,不管是POST方式或者GET方法提交过来的数据。但是，一般GET方法提交数据 时，http request entity body部分都为空。
2,php://input 与$HTTP_RAW_POST_DATA读取的数据是一样的，都只读取Content-Type不为multipart/form-data的数据。
学习笔记

1，Coentent-Type仅在取值为application/x-www-data-urlencoded和multipart/form-data两种情况下，PHP才会将http请求数据包中相应的数据填入全局变量$_POST
2，PHP不能识别的Content-Type类型的时候，会将http请求包中相应的数据填入变量$HTTP_RAW_POST_DATA
3, 只有Coentent-Type不为multipart/form-data的时候，PHP不会将http请求数据包中的相应数据填入php://input，否则其它情况都会。填入的长度，由Coentent-Length指定。
4，只有Content-Type为application/x-www-data-urlencoded时，php://input数据才跟$_POST数据相一致。
5，php://input数据总是跟$HTTP_RAW_POST_DATA相同，但是php://input比$HTTP_RAW_POST_DATA更凑效，且不需要特殊设置php.ini
6，PHP会将PATH字段的query_path部分，填入全局变量$_GET。通常情况下，GET方法提交的http请求，body为空。
