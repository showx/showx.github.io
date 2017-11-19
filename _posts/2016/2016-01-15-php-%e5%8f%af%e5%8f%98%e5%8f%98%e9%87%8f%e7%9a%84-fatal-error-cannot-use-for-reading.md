---
layout: post
title: php 可变变量的 Fatal error: Cannot use [] for reading
date: 2016-01-15 18:09
author: admin
comments: true
categories: []
---
&lt;?php $nums = array(1, 2, 3); $arr_name = 'nums'; $$arr_name[] = 4; print_r($nums); ?&gt;
执行上面程序会出现Fatal error: Cannot use [] for reading，因为语义模糊，要这样改：

&lt;?php $nums = array(1, 2, 3); $arr_name = 'nums'; ${$arr_name}[] = 4; print_r($nums); ?&gt;

$a = $b = array("1");
$t = array("a","b");

foreach($t as $k =&gt;$v)
{
${$v}[] = 1;
}
var_dump($a,$b);
exit();
