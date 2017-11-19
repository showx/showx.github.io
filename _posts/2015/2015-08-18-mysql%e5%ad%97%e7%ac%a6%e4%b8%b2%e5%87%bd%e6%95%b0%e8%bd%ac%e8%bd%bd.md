---
layout: post
title: mysql字符串函数(转载)
date: 2015-08-18 14:10
author: admin
comments: true
categories: []
---
对于针对字符串位置的操作，第一个位置被标记为1。
<b>ASCII(str)</b>
<b>返回字符串</b><b>str</b><b>的 最左面字符的ASCII代码值。</b>如果str是空字符串， 返回0。如果str是NULL，返回NULL。
mysql&gt; select ASCII('2');
<span class="Apple-converted-space">        </span>-&gt; 50
mysql&gt; select ASCII(2);
<span class="Apple-converted-space">        </span>-&gt; 50
mysql&gt; select ASCII('dx');
<span class="Apple-converted-space">        </span>-&gt; 100
也可参见ORD()函数。
<b>ORD(str)</b>
如果字符串str最左面字符是一个多字节字符，通过以格式((first byte ASCII code)*256+(second byte ASCII code))[*256+third byte ASCII code...]返 回字符的ASCII代码值来返回多字节字符代码。如果最左面的字符不是一个多字节字符。返回与ASCII()函 数返回的相同值。
mysql&gt; select ORD('2');
<span class="Apple-converted-space">        </span>-&gt; 50

<b>CONV(N,from_base,to_base)</b>
在不同的数字基之间变换数字。返回数字N的字符串数字， 从from_base基变换为to_base基，如果任何参数是NULL， 返回NULL。参数N解 释为一个整数，但是可以指定为一个整数或一个字符串。最小基是2且最大的基 是36。如果to_base是 一个负数，N被认为是一个有符号数，否则，N被当作无符号数。 CONV以 64位点精度工作。
mysql&gt; select CONV("a",16,2);
<span class="Apple-converted-space">        </span>-&gt; '1010'
mysql&gt; select CONV("6E",18,8);
<span class="Apple-converted-space">        </span>-&gt; '172'
mysql&gt; select CONV(-17,10,-18);
<span class="Apple-converted-space">        </span>-&gt; '-H'
mysql&gt; select CONV(10+"10"+'10'+0xa,10,10);
<span class="Apple-converted-space">        </span>-&gt; '40'

<b>BIN(N)</b>
返回二进制值N的一个字符串表示，在此N是一个长整数(BIGINT) 数字，这等价于CONV(N,10,2)。如果N是NULL，返回NULL。
mysql&gt; select BIN(12);
<span class="Apple-converted-space">        </span>-&gt; '1100'
<b>OCT(N)</b>
返回八进制值N的一个字符串的表示，在此N是一个长整型数字，这等价于CONV(N,10,8)。 如果N是NULL，返回NULL。
mysql&gt; select OCT(12);
<span class="Apple-converted-space">        </span>-&gt; '14'

<b>HEX(N)</b>
返回十六进制值N一个字符串的表示，在此N是一个长整型(BIGINT) 数字，这等价于CONV(N,10,16)。如果N是NULL，返回NULL。
mysql&gt; select HEX(255);
<span class="Apple-converted-space">        </span>-&gt; 'FF'

<b>CHAR(N,...)</b>
<b>CHAR()</b><b>将参数解释为整数并且返回 由这些整数的ASCII代码字符组成的一个字符串。</b>NULL值 被跳过。
mysql&gt; select CHAR(77,121,83,81,'76');
<span class="Apple-converted-space">        </span>-&gt; 'MySQL'
mysql&gt; select CHAR(77,77.3,'77.3');
<span class="Apple-converted-space">        </span>-&gt; 'MMM'

<b>CONCAT(str1,str2,...)</b>
<b>返回来自于参数连结的字符串</b>。如果任何参数是NULL， 返回NULL。可以有超过2个的参数。一个数字参数被变换为等价的字符串形 式。
mysql&gt; select CONCAT('My', 'S', 'QL');
<span class="Apple-converted-space">        </span>-&gt; 'MySQL'
mysql&gt; select CONCAT('My', NULL, 'QL');
<span class="Apple-converted-space">        </span>-&gt; NULL
mysql&gt; select CONCAT(14.3);
<span class="Apple-converted-space">        </span>-&gt; '14.3'
<b>LENGTH(str)</b>
<b>　</b>
<b>OCTET_LENGTH(str)</b>
<b>　</b>
<b>CHAR_LENGTH(str)</b>
<b>　</b>
<b>CHARACTER_LENGTH(str)</b>
返回字符串str的长度。
mysql&gt; select LENGTH('text');
<span class="Apple-converted-space">        </span>-&gt; 4
mysql&gt; select OCTET_LENGTH('text');
<span class="Apple-converted-space">        </span>-&gt; 4
注意，对于多字节字符，其CHAR_LENGTH()仅计算一次。
<b>LOCATE(substr,str)</b>

<b>POSITION(substr IN str)</b>
返回子串substr在字符串str第一个出现的位置，如果substr不 是在str里面，返回0.
mysql&gt; select LOCATE('bar', 'foobarbar');
<span class="Apple-converted-space">        </span>-&gt; 4
mysql&gt; select LOCATE('xbar', 'foobar');
<span class="Apple-converted-space">        </span>-&gt; 0
该函数是多字节可靠的。
<b>LOCATE(substr,str,pos)</b>
返回子串substr在字符串str第一个出现的位置，从位置pos开 始。如果substr不是在str里 面，返回0。
mysql&gt; select LOCATE('bar', 'foobarbar',5);
<span class="Apple-converted-space">        </span>-&gt; 7
这函数是多字节可靠的。
<b>INSTR(str,substr)</b>
返回子串substr在字符串str中的第一个出现的位置。这与有2个参数形式的LOCATE()相 同，除了参数被颠倒。
mysql&gt; select INSTR('foobarbar', 'bar');
<span class="Apple-converted-space">        </span>-&gt; 4
mysql&gt; select INSTR('xbar', 'foobar');
<span class="Apple-converted-space">        </span>-&gt; 0
这函数是多字节可靠的。
<b>LPAD(str,len,padstr)</b>
返回字符串str，左面用字符串padstr填补直到str是len个字符长。
mysql&gt; select LPAD('hi',4,'??');
<span class="Apple-converted-space">        </span>-&gt; '??hi'

<b>RPAD(str,len,padstr)</b>
返回字符串str，右面用字符串padstr填补直到str是len个字符长。
mysql&gt; select RPAD('hi',5,'?');
<span class="Apple-converted-space">        </span>-&gt; 'hi???'
<b>LEFT(str,len)</b>
返回字符串str的最左面len个字符。
mysql&gt; select LEFT('foobarbar', 5);
<span class="Apple-converted-space">        </span>-&gt; 'fooba'
该函数是多字节可靠的。
<b>RIGHT(str,len)</b>
返回字符串str的最右面len个字符。
mysql&gt; select RIGHT('foobarbar', 4);
<span class="Apple-converted-space">        </span>-&gt; 'rbar'
该函数是多字节可靠的。
<b>SUBSTRING(str,pos,len)</b>

<b>SUBSTRING(str FROM pos FOR len)</b>

<b>MID(str,pos,len)</b>
从字符串str返回一个len个字符的子串，从位置pos开 始。使用FROM的变种形式是ANSI SQL92语法。
mysql&gt; select SUBSTRING('Quadratically',5,6);
<span class="Apple-converted-space">        </span>-&gt; 'ratica'
该函数是多字节可靠的。
<b>SUBSTRING(str,pos)</b>

<b>SUBSTRING(str FROM pos)</b>
从字符串str的起始位置pos返回一个子串。
mysql&gt; select SUBSTRING('Quadratically',5);
<span class="Apple-converted-space">        </span>-&gt; 'ratically'
mysql&gt; select SUBSTRING('foobarbar' FROM 4);
<span class="Apple-converted-space">        </span>-&gt; 'barbar'
该函数是多字节可靠的。
<b>SUBSTRING_INDEX(str,delim,count)</b>
返回从字符串str的第count个出现的分 隔符delim之后的子串。如果count是正数，返回最后的分隔符到左边(从左边数) 的所有字符。如果count是负数，返回最后的分隔符到右边的所有字符(从右边数)。
mysql&gt; select SUBSTRING_INDEX('www.mysql.com', '.', 2);
<span class="Apple-converted-space">        </span>-&gt; 'www.mysql'
mysql&gt; select SUBSTRING_INDEX('www.mysql.com', '.', -2);
<span class="Apple-converted-space">        </span>-&gt; 'mysql.com'
该函数对多字节是可靠的。
<b>LTRIM(str)</b>
返回删除了其前置空格字符的字符串str。
mysql&gt; select LTRIM('<span class="Apple-converted-space">  </span>barbar');
<span class="Apple-converted-space">        </span>-&gt; 'barbar'
<b>RTRIM(str)</b>
返回删除了其拖后空格字符的字符串str。
mysql&gt; select RTRIM('barbar <span class="Apple-converted-space">  </span>');
<span class="Apple-converted-space">        </span>-&gt; 'barbar'
该函数对多字节是可靠的。
<b>TRIM([[BOTH | LEADING | TRAILING] [remstr] FROM] str)</b>
返回字符串str，其所有remstr前缀或后缀被删除了。如果没有修饰符BOTH、LEADING或TRAILING给 出，BOTH被假定。如果remstr没 被指定，空格被删除。
mysql&gt; select TRIM('<span class="Apple-converted-space">  </span>bar <span class="Apple-converted-space">  </span>');
<span class="Apple-converted-space">        </span>-&gt; 'bar'
mysql&gt; select TRIM(LEADING 'x' FROM 'xxxbarxxx');
<span class="Apple-converted-space">        </span>-&gt; 'barxxx'
mysql&gt; select TRIM(BOTH 'x' FROM 'xxxbarxxx');
<span class="Apple-converted-space">        </span>-&gt; 'bar'
mysql&gt; select TRIM(TRAILING 'xyz' FROM 'barxxyz');
<span class="Apple-converted-space">        </span>-&gt; 'barx'
该函数对多字节是可靠的。
<b>SOUNDEX(str)</b>
返回str的一个同音字符串。听起来“大致相同”的2个 字符串应该有相同的同音字符串。一个“标准”的同音字符串长是4个字符，但是SOUNDEX()函 数返回一个任意长的字符串。你可以在结果上使用SUBSTRING()得到 一个“标准”的 同音串。所有非数字字母字符在给定的字符串中被忽略。所有在A-Z之外的字符国际字母被当作元音。
mysql&gt; select SOUNDEX('Hello');
<span class="Apple-converted-space">        </span>-&gt; 'H400'
mysql&gt; select SOUNDEX('Quadratically');
<span class="Apple-converted-space">        </span>-&gt; 'Q36324'

<b>SPACE(N)</b>
返回由N个空格字符组成的一个字符串。
mysql&gt; select SPACE(6);
<span class="Apple-converted-space">        </span>-&gt; '<span class="Apple-converted-space">      </span>'

<b>REPLACE(str,from_str,to_str)</b>
返回字符串str，其字符串from_str的所有出现由字符串to_str代 替。
mysql&gt; select REPLACE('www.mysql.com', 'w', 'Ww');
<span class="Apple-converted-space">        </span>-&gt; 'WwWwWw.mysql.com'
该函数对多字节是可靠的。
<b>REPEAT(str,count)</b>
返回由重复countTimes次的字符串str组成的一个字符串。如果count &lt;= 0，返回一个空字符串。如果str或count是NULL， 返回NULL。
mysql&gt; select REPEAT('MySQL', 3);
<span class="Apple-converted-space">        </span>-&gt; 'MySQLMySQLMySQL'

<b>REVERSE(str)</b>
返回颠倒字符顺序的字符串str。
mysql&gt; select REVERSE('abc');
<span class="Apple-converted-space">        </span>-&gt; 'cba'
该函数对多字节可靠的。
<b>INSERT(str,pos,len,newstr)</b>
返回字符串str，在位置pos起始的子串且len个 字符长得子串由字符串newstr代替。
mysql&gt; select INSERT('Quadratic', 3, 4, 'What');
<span class="Apple-converted-space">        </span>-&gt; 'QuWhattic'
该函数对多字节是可靠的。
<b>ELT(N,str1,str2,str3,...)</b>
如果N= 1，返回str1，如 果N= 2， 返回str2，等等。如果N小 于1或大于参数个数，返回NULL。ELT()是FIELD()反 运算。
mysql&gt; select ELT(1, 'ej', 'Heja', 'hej', 'foo');
<span class="Apple-converted-space">        </span>-&gt; 'ej'
mysql&gt; select ELT(4, 'ej', 'Heja', 'hej', 'foo');
<span class="Apple-converted-space">        </span>-&gt; 'foo'
<b>FIELD(str,str1,str2,str3,...)</b>
返回str在str1, str2, str3, ...清 单的索引。如果str没找到，返回0。FIELD()是ELT()反运算。
mysql&gt; select FIELD('ej', 'Hej', 'ej', 'Heja', 'hej', 'foo');
<span class="Apple-converted-space">        </span>-&gt; 2
mysql&gt; select FIELD('fo', 'Hej', 'ej', 'Heja', 'hej', 'foo');
<span class="Apple-converted-space">        </span>-&gt; 0
<b>FIND_IN_SET(str,strlist)</b>
如果字符串str在由N子串组成的表strlist之 中，返回一个1到N的 值。一个字符串表是被“,”分隔的子串组成的一个字符串。如果第一个参数是 一个常数字符串并且第二个参数是一种类型为SET的列，FIND_IN_SET()函数被优化而使用位运算！如果str不是在strlist里 面或如果strlist是空字符串，返回0。如果任何一个参数是NULL， 返回NULL。如果第一个参数包含一个“,”，该函数将工作不正常。
mysql&gt; SELECT FIND_IN_SET('b','a,b,c,d');
<span class="Apple-converted-space">        </span>-&gt; 2

<b>MAKE_SET(bits,str1,str2,...)</b>
返回一个集合 (包含由“,”字符分隔的子串组成的一个 字符串)，由相应的位在bits集合中的的字符串组成。str1对应于位0，str2对 应位1，等等。在str1, str2, ...中 的NULL串不添加到结果中。
mysql&gt; SELECT MAKE_SET(1,'a','b','c');
<span class="Apple-converted-space">        </span>-&gt; 'a'
mysql&gt; SELECT MAKE_SET(1 | 4,'hello','nice','world');
<span class="Apple-converted-space">        </span>-&gt; 'hello,world'
mysql&gt; SELECT MAKE_SET(0,'a','b','c');
<span class="Apple-converted-space">        </span>-&gt; ''
<b>EXPORT_SET(bits,on,off,[separator,[number_of_bits]])</b>
返回一个字符串，在这里对于在“bits”中设定每一位，你得到一个“on”字符串，并且对于每个复位(reset)的位，你得到一个 “off”字符串。每个字符串用“separator”分隔(缺省“,”)，并且只有“bits”的“number_of_bits” (缺省64)位被使用。
mysql&gt; select EXPORT_SET(5,'Y','N',',',4)
<span class="Apple-converted-space">        </span>-&gt; Y,N,Y,N
<b>LCASE(str)</b>
<b>　</b>
<b>LOWER(str)</b>
返回字符串str，根据当前字符集映射(缺省是ISO- 8859-1 Latin1)把所有的字符改变成小写。该函数对多字节是可靠的。
mysql&gt; select LCASE('QUADRATICALLY');
<span class="Apple-converted-space">        </span>-&gt; 'quadratically'

<b>UCASE(str)</b>
<b>　</b>
<b>UPPER(str)</b>
返回字符串str，根据当前字符集映射(缺省是ISO- 8859-1 Latin1)把所有的字符改变成大写。该函数对多字节是可靠的。
mysql&gt; select UCASE('Hej');
<span class="Apple-converted-space">        </span>-&gt; 'HEJ'
该函数对多字节是可靠的。
<b>LOAD_FILE(file_name)</b>
读入文件并且作为一个字符串返回文件内容。文件必须在服务器上，你必须指定到文件的完整路径名，而且你必须有<b>file</b>权 限。文件必须所有内容都是可读的并且小于max_allowed_packet。 如果文件不存在或由于上面原因之一不能被读出，函数返回NULL。
mysql&gt; UPDATE table_name
<span class="Apple-converted-space">           </span>SET blob_column=LOAD_FILE("/tmp/picture")
<span class="Apple-converted-space">           </span>WHERE id=1;

<b>MySQL</b>必要时自动变换数字为字符串，并且反过来也如此：
mysql&gt; SELECT 1+"1";
<span class="Apple-converted-space">        </span>-&gt; 2
mysql&gt; SELECT CONCAT(2,' test');
<span class="Apple-converted-space">        </span>-&gt; '2 test'
如果你想要明确地变换一个数字到一个字符串，把它作为参数传递到CONCAT()。
如果字符串函数提供一个二进制字符串作为参数，结果字符串也是一个二进制字符串。被变换到一个字符串的数字被当作是一个二进制字符串。这仅影响比 较。
