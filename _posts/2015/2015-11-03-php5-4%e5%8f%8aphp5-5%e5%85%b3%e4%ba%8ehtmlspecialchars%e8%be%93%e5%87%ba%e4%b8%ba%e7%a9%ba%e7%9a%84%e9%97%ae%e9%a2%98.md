---
layout: post
title: PHP5.4及PHP5.5关于htmlspecialchars输出为空的问题
date: 2015-11-03 11:11
author: admin
comments: true
categories: []
---
终于单位更换新的服务器，全部采用的windows server 2008 r2 64位系统，在艰难的配置环境之后，却发现IIS7.5应用池不断报错，后来更换了PHP5.5的64位版本，就解决了问题，看来在64位win2008下，还是64位PHP才是最佳搭配啊。

但是，，，苦逼的事情总是那么多，本人制作的网站是dede系统的，在登陆后台时却遇到了麻烦，登陆后提示HTTp500错误，一番搜索解决之后，又遇到发表新文章提示标题不能为空的问题，明明有标题，却不行？这是因为在PHP5.4及以后的版本中htmlspecialchars默认为UTF8，你是中文，当然检测不到你了。真纠结啊！只好按照解决DEDE标题为空的办法替换一了个遍，发现DEDE有十几处用到了htmlspecialchars属性，还好啦。

另外转来一位技术大牛写的文章，希望对您有帮助。原文如下：

从旧版升级到php5.4，恐怕最麻烦的就是htmlspecialchars这个问题了！当然，htmlentities也会受影响，不过，对于中文站来说一般用htmlspecialchars比较常见，htmlentities非常少用到。

可能老外认为网页普遍应该是utf-8编码的，于是苦了那些用GB2312，GBK编码的中文站......！

具体表现：

$str = "9enjoy.com的php版本是5.2.10";
echo htmlspecialchars($str);

gbk字符集下输出为空...utf-8下，输出正常。

为什么呢，原因在于5.4.0对这个函数的变化：

5.4.0   The default value for the encoding parameter was changed to UTF-8.

原来是什么呢？

string htmlspecialchars ( string $string [, int $flags = ENT_COMPAT | ENT_HTML401 [, string $encoding = 'UTF-8' [, bool $double_encode = true ]]] )

Defines encoding used in conversion. If omitted, the default value for this argument is ISO-8859-1 in versions of PHP prior to 5.4.0, and UTF-8 from PHP 5.4.0 onwards.

原来是ISO-8859-1，5.4后默认变成utf-8！然后中文使用这个函数就输出为空白了。

国内一堆开源程序在5.4下都会有这样的问题，DISCUZ官方也建议用户不要升级到5.4。

解决方案：

1.苦逼的修改所有用到htmlspecialchars地方的程序

1.1 其第二个$flags参数，默认是ENT_COMPAT，因此改成
htmlspecialchars($str,ENT_COMPAT,'GB2312');
为什么不是GBK？因为没有GBK这个参数，如果强行使用GBK，则报错给你看：
Warning: htmlspecialchars(): charset `gbk' not supported, assuming utf-8
为了能使用GBK，则改成：
htmlspecialchars($str,ENT_COMPAT,'ISO-8859-1');

1.2.一样是改程序，但可以省略一个参数。
可以在网页头部加
ini_set('default_charset','gbk');
然后改成
htmlspecialchars($str,ENT_COMPAT,'');
文档中有写：An empty string activates detection from script encoding (Zend multibyte), default_charset and current locale (see nl_langinfo() and setlocale()), in this order. Not recommended.
大概意思就是：传入空字符串则使用default_charset的编码

1.3.封装一个函数吧...本来htmlspecialchars这个单词一直不好记。
function htmlout($str) {
    return htmlspecialchars($str,ENT_COMPAT,'ISO-8859-1');
}
然后去批量替换。

2.直接修改源码，重编译！这也是目前我在线上做的方案。
修改ext/standard/html.c
大概在372行
/* Default is now UTF-8 */
if (charset_hint == NULL)
return cs_utf_8;
把cs_utf_8改成 cs_8859_1
/* Default is now UTF-8 */
if (charset_hint == NULL)
return cs_8859_1;
编译后，原程序就不用做任何调整了。
