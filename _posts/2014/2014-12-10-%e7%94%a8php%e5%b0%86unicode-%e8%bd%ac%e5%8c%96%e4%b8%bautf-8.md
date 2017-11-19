---
layout: post
title: 用PHP将Unicode 转化为UTF-8
date: 2014-12-10 11:04
author: admin
comments: true
categories: []
---
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>
<div>
<div><code>function</code> <code>unescape(</code><code>$str</code><code>) {</code></div>
<div><code>    </code><code>$str</code> <code>= rawurldecode(</code><code>$str</code><code>);</code></div>
<div><code>    </code><code>preg_match_all(</code><code>"/(?:%u.{4})|&amp;#x.{4};|&amp;#\d+;|.+/U"</code><code>,</code><code>$str</code><code>,</code><code>$r</code><code>);</code></div>
<div><code>    </code><code>$ar</code> <code>= </code><code>$r</code><code>[0];</code></div>
<div><code>    </code><code>//print_r($ar);</code></div>
<div><code>    </code><code>foreach</code><code>(</code><code>$ar</code> <code>as</code> <code>$k</code><code>=&gt;</code><code>$v</code><code>) {</code></div>
<div><code>        </code><code>if</code><code>(</code><code>substr</code><code>(</code><code>$v</code><code>,0,2) == </code><code>"%u"</code><code>){</code></div>
<div><code>            </code><code>$ar</code><code>[</code><code>$k</code><code>] = iconv(</code><code>"UCS-2BE"</code><code>,</code><code>"UTF-8"</code><code>,pack(</code><code>"H4"</code><code>,</code><code>substr</code><code>(</code><code>$v</code><code>,-4)));</code></div>
<div><code>  </code><code>}</code></div>
<div><code>        </code><code>elseif</code><code>(</code><code>substr</code><code>(</code><code>$v</code><code>,0,3) == </code><code>"&amp;#x"</code><code>){</code></div>
<div><code>            </code><code>$ar</code><code>[</code><code>$k</code><code>] = iconv(</code><code>"UCS-2BE"</code><code>,</code><code>"UTF-8"</code><code>,pack(</code><code>"H4"</code><code>,</code><code>substr</code><code>(</code><code>$v</code><code>,3,-1)));</code></div>
<div><code>  </code><code>}</code></div>
<div><code>        </code><code>elseif</code><code>(</code><code>substr</code><code>(</code><code>$v</code><code>,0,2) == </code><code>"&amp;#"</code><code>) {</code></div>
<div><code>            </code></div>
<div><code>            </code><code>$ar</code><code>[</code><code>$k</code><code>] = iconv(</code><code>"UCS-2BE"</code><code>,</code><code>"UTF-8"</code><code>,pack(</code><code>"n"</code><code>,</code><code>substr</code><code>(</code><code>$v</code><code>,2,-1)));</code></div>
<div><code>        </code><code>}</code></div>
<div><code>    </code><code>}</code></div>
<div><code>    </code><code>return</code> <code>join(</code><code>""</code><code>,</code><code>$ar</code><code>);</code></div>
<div><code>}</code></div>
<div><code>echo</code> <code>unescape(</code><code>"紫星蓝"</code><code>);</code></div>
<div><code>今天有用户反馈，表单系统用户提交的数据中文会乱码。测试发现问题出在 iconv 转换上。</code></div>
<div><code>iconv(</code><code>'UCS-2'</code><code>, </code><code>'GBK'</code><code>, </code><code>'中文'</code><code>)</code></div>
<div><code>Google 搜索发现，原因是 Linux 服务器上 UCS-2 编码方式与 Winodws 不一致。</code></div>
<div><code>于是，我改成  iconv(</code><code>'UCS-2BE'</code><code>, </code><code>'GBK'</code><code>, </code><code>'中文'</code><code>) 试试，中文正常了</code></div>
<div><code> </code></div>
<div><code>以下是有关两个平台 UCS-2 编码的潜规则：</code></div>
<div></div>
<div><code>1, UCS-2 不等于 UTF-16。 UTF-16 每个字节使用 ASCII 字符范围编码，而 UCS-2 对每个字节的编码可以超出 ASCII 字符范围。UCS-2 和 UTF-16 对每个字符至多占两个字节，但是他们的编码是不一样的。</code></div>
<div></div>
<div><code>2, 对于 UCS-2, windows 下默认是 UCS-2LE。用 MultibyteToWidechar（或者A2W）生成的是 UCS-2LE 的 unicode。windows记事本可以将文本保存为 UCS-2BE，相当于多了层转换。</code></div>
<div></div>
<div><code>3, 对于 UCS-2, linux 下默认是 UCS-2BE。用iconv(指定UCS-2)来转换生成的是 UCS-2BE 的 unicode。如果转换windows平台过来的 UCS-2, 需要指定 UCS-2LE。</code></div>
<div></div>
<div><code>4, 鉴于windows和linux等多个平台对 UCS-2 的理解不同（UCS-2LE,UCS-2BE）。MS 主张 unicode 有个引导标志(UCS-2LE FFFE, UCS-2BE FEFF)，以表明下面的字符是 unicode 并且判别 big-endian 或 little-endian。 所以从 windows 平台过来的数据发现有这个前缀，不用慌张。</code></div>
<div></div>
<div><code>5, linux 的编码输出，比如从文件输出，从 printf 输出，需要控制台做适当的编码匹配（如果编码不匹配，一般和该程序编译时的编码有若干关系），而控制台的转换输入需要查看当前的系统编码。比如控制台当前的编码是 UTF-8, 那么 UTF-8 编码的东西能正确显示，GBK 就不能；同样，当前编码是 GBK, 就能显示 GBK 编码，后来的系统应该更智能的处理好更多的转换了。不过通过 putty 等终端还是需要设置好终端的编码转换以解除乱码的烦恼。</code></div>
<div><code>PHP中对汉字进行UNICODE编码和解码的实现</code></div>
<div><code> </code><code>//将内容进行UNICODE编码</code></div>
<div><code>function</code> <code>unicode_encode(</code><code>$name</code><code>)</code></div>
<div><code>{</code></div>
<div><code>    </code><code>$name</code> <code>= iconv(</code><code>'UTF-8'</code><code>, </code><code>'UCS-2'</code><code>, </code><code>$name</code><code>);</code></div>
<div><code>    </code><code>$len</code> <code>= </code><code>strlen</code><code>(</code><code>$name</code><code>);</code></div>
<div><code>    </code><code>$str</code> <code>= </code><code>''</code><code>;</code></div>
<div><code>    </code><code>for</code> <code>(</code><code>$i</code> <code>= 0; </code><code>$i</code> <code>&lt; </code><code>$len</code> <code>- 1; </code><code>$i</code> <code>= </code><code>$i</code> <code>+ 2)</code></div>
<div><code>    </code><code>{</code></div>
<div><code>        </code><code>$c</code> <code>= </code><code>$name</code><code>[</code><code>$i</code><code>];</code></div>
<div><code>        </code><code>$c2</code> <code>= </code><code>$name</code><code>[</code><code>$i</code> <code>+ 1];</code></div>
<div><code>        </code><code>if</code> <code>(ord(</code><code>$c</code><code>) &gt; 0)</code></div>
<div><code>        </code><code>{    </code><code>// 两个字节的文字</code></div>
<div><code>            </code><code>$str</code> <code>.= </code><code>'\u'</code><code>.</code><code>base_convert</code><code>(ord(</code><code>$c</code><code>), 10, 16).</code><code>base_convert</code><code>(ord(</code><code>$c2</code><code>), 10, 16);</code></div>
<div><code>        </code><code>}</code></div>
<div><code>        </code><code>else</code></div>
<div><code>        </code><code>{</code></div>
<div><code>            </code><code>$str</code> <code>.= </code><code>$c2</code><code>;</code></div>
<div><code>        </code><code>}</code></div>
<div><code>    </code><code>}</code></div>
<div><code>    </code><code>return</code> <code>$str</code><code>;</code></div>
<div><code>}</code></div>
<div><code>$name</code> <code>= </code><code>'MY,你大爷的'</code><code>;</code></div>
<div><code>$unicode_name</code><code>=unicode_encode(</code><code>$name</code><code>);</code></div>
<div><code>echo</code> <code>'&lt;h3&gt;'</code><code>.</code><code>$unicode_name</code><code>.</code><code>'&lt;/h3&gt;'</code><code>;</code></div>
<div><code>// 将UNICODE编码后的内容进行解码</code></div>
<div><code>function</code> <code>unicode_decode(</code><code>$name</code><code>)</code></div>
<div><code>{</code></div>
<div><code>    </code><code>// 转换编码，将Unicode编码转换成可以浏览的utf-8编码</code></div>
<div><code>    </code><code>$pattern</code> <code>= </code><code>'/([\w]+)|(\\\u([\w]{4}))/i'</code><code>;</code></div>
<div><code>    </code><code>preg_match_all(</code><code>$pattern</code><code>, </code><code>$name</code><code>, </code><code>$matches</code><code>);</code></div>
<div><code>    </code><code>if</code> <code>(!</code><code>empty</code><code>(</code><code>$matches</code><code>))</code></div>
<div><code>    </code><code>{</code></div>
<div><code>        </code><code>$name</code> <code>= </code><code>''</code><code>;</code></div>
<div><code>        </code><code>for</code> <code>(</code><code>$j</code> <code>= 0; </code><code>$j</code> <code>&lt; </code><code>count</code><code>(</code><code>$matches</code><code>[0]); </code><code>$j</code><code>++)</code></div>
<div><code>        </code><code>{</code></div>
<div><code>            </code><code>$str</code> <code>= </code><code>$matches</code><code>[0][</code><code>$j</code><code>];</code></div>
<div><code>            </code><code>if</code> <code>(</code><code>strpos</code><code>(</code><code>$str</code><code>, </code><code>'\\u'</code><code>) === 0)</code></div>
<div><code>            </code><code>{</code></div>
<div><code>                </code><code>$code</code> <code>= </code><code>base_convert</code><code>(</code><code>substr</code><code>(</code><code>$str</code><code>, 2, 2), 16, 10);</code></div>
<div><code>                </code><code>$code2</code> <code>= </code><code>base_convert</code><code>(</code><code>substr</code><code>(</code><code>$str</code><code>, 4), 16, 10);</code></div>
<div><code>                </code><code>$c</code> <code>= </code><code>chr</code><code>(</code><code>$code</code><code>).</code><code>chr</code><code>(</code><code>$code2</code><code>);</code></div>
<div><code>                </code><code>$c</code> <code>= iconv(</code><code>'UCS-2'</code><code>, </code><code>'UTF-8'</code><code>, </code><code>$c</code><code>);</code></div>
<div><code>                </code><code>$name</code> <code>.= </code><code>$c</code><code>;</code></div>
<div><code>            </code><code>}</code></div>
<div><code>            </code><code>else</code></div>
<div><code>            </code><code>{</code></div>
<div><code>                </code><code>$name</code> <code>.= </code><code>$str</code><code>;</code></div>
<div><code>            </code><code>}</code></div>
<div><code>        </code><code>}</code></div>
<div><code>    </code><code>}</code></div>
<div><code>    </code><code>return</code> <code>$name</code><code>;</code></div>
<div><code>}</code></div>
<div><code>echo</code> <code>'MY,\u4f60\u5927\u7237\u7684 -&gt; '</code><code>.unicode_decode(</code><code>$unicode_name</code><code>);</code></div>
</div></td>
</tr>
</tbody>
</table>
&nbsp;

===================
<pre id="recommend-content-1278215037">以 \u 开头，后跟4个十六进制数（即范围在 0－9 A－F的字符），是unicode编码格式的字符。

在PHP中，如果想要进行两者之间的转换，可以使用以下的函数：

&lt;meta charset="UTF-8"/&gt;
&lt;?php
// 转换编码，将可以浏览的utf-8字符串转换成Unicode编码
function unicode_encode($uStr)
{
   $uStr = iconv('UTF-8', 'UCS-2', $uStr);
   $len = strlen($uStr);
   $str = '';
   //for ($i = 0; $i &lt; $len – 1; $i = $i + 2){
   for ($i = 0; $i &lt; $len - 1; $i = $i + 2) {
      $c = $uStr[$i];
      $c2 = $uStr[$i + 1];
      if (ord($c) &gt; 0) { // 两个字节的文字
         $str .= '\u' . base_convert(ord($c), 10, 16) . base_convert(ord($c2), 10, 16);
      } else {
         $str .= $c2;
      }
   }
   return $str;
}
// 转换编码，将Unicode编码转换成可以浏览的utf-8字符串
function unicode_decode($uStr)
{
   $pattern = '/([\w]+)|(\\\u([\w]{4}))/i';
   preg_match_all($pattern, $uStr, $matches);
   if (!empty($matches)) {
      $uStr = '';
      for ($j = 0; $j &lt; count($matches[0]); $j++) {
         $str = $matches[0][$j];
         if (strpos($str, '\\u') === 0) {
            $code = base_convert(substr($str, 2, 2), 16, 10);
            $code2 = base_convert(substr($str, 4), 16, 10);
            $c = chr($code) . chr($code2);
            $c = iconv('UCS-2', 'UTF-8', $c);
            $uStr .= $c;
         } else {
            $uStr .= $str;
         }
      }
   }
   return $uStr;
}
$uStr = '大家好';
echo '原始字符: ', $uStr , '  Undicode编码：', unicode_encode($uStr);
?&gt;</pre>
