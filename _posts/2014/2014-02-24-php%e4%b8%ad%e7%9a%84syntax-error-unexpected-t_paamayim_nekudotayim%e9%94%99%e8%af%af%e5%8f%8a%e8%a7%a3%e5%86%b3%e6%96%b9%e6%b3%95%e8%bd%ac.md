---
layout: post
title: PHP中的“syntax error, unexpected T_PAAMAYIM_NEKUDOTAYIM”错误及解决方法[转]
date: 2014-02-24 13:50
author: admin
comments: true
categories: []
---
<div>PHP中的“syntax error, unexpected T_PAAMAYIM_NEKUDOTAYIM”错误</div>
<div>class Test{</div>
<div> <wbr />  <wbr />  <wbr />  <wbr /> static function test_c(){</div>
<div> <wbr />  <wbr />  <wbr />  <wbr />  <wbr />  <wbr />  <wbr />  <wbr /> echo "test";</div>
<div> <wbr />  <wbr />  <wbr />  <wbr /> }</div>
<div>}</div>
<div>$class="Test";</div>
<div>$method="test_c";</div>
<div>$class::$method();</div>
<div></div>
<div>上面类似的代码在php5.3之前会报错，就是php版本不支持$变量做类名函数名。php5.3之后是支持的。</div>
<div>php5.3之前可以这样写:</div>
<div>
<div>class Test{</div>
<div> <wbr />  <wbr />  <wbr />  <wbr /> static function test_c(){</div>
<div> <wbr />  <wbr />  <wbr />  <wbr />  <wbr />  <wbr />  <wbr />  <wbr /> echo "test";</div>
<div> <wbr />  <wbr />  <wbr />  <wbr /> }</div>
<div>}</div>
<div>$class="Test";</div>
<div>$method="test_c";</div>
<div>e v a l("$class::$method();");</div>
</div>
