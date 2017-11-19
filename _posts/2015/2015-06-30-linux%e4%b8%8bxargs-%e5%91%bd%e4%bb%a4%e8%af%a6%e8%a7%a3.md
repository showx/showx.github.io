---
layout: post
title: Linux下xargs 命令详解
date: 2015-06-30 16:07
author: admin
comments: true
categories: []
---
&nbsp;

xargs [ -p ] [ -t ] [ -e [ EOFString ] ] [ -E EOFString ] [ -i [
ReplaceString ] ] [ -I ReplaceString ] [ -l [ Number ] ] [ -L Number ] [ -n
Number [ -x ] ] [ -s Size ] [ Command [ Argument ... ] ]
<strong>注： 不要在小写的符号和参数之间安排空格。</strong><strong>
</strong>

描述

生成的命令行长度是 Command 和每个作为字符串对待的Argument（包括每个字符串的空字节终结符）的大小的总和（以字节为单位）。xargs 命令限制命令行的长度。当构造的命令行运行时，组合的Argument 和环境列表不能超出 ARG_MAX 字节。在这个约束下，如果不指定 -n 或 -s 标志，缺省命令行长度至少是由LINE_MAX 指定的值。

标志

<strong>-e[EOFString]</strong>

废弃的标志。请使用 -E 标志。

将 EOFString 参数用作逻辑 EOF 字符串。如果不指定 -e 或 -E 标志，则假定下划线（_）为逻辑 EOF 字符串。如果不指定EOFString 参数，则禁用逻辑 EOF 字符串能力，且下划线按照字面含义使用。xargs 命令读取标准输入直到达到 EOF或指定的字符串。

<strong>-E EOFString</strong>

指定逻辑 EOF 字符串以替换缺省的下划线（_）。 xargs 命令读取标准输入直到达到 EOF 或指定的字符串。

<strong>-i[ReplaceString]</strong>

废弃的标志。请使用 -I（大写 i）标志。

如果没有指定 ReplaceString 参数，则使用字符串 <strong>"{}"</strong>。

注：-I（大写 i）和 -i 标志是互相排斥的；最后指定的标志生效。

<strong>-I ReplaceString</strong>

（大写 i）。插入标准输入的每一行作为 Command 参数的自变量，把它插入每个发生 ReplaceString 的 Argument中。ReplaceString 不能在超过 5 个自变量中使用。在每个标准输入行开始的空字符被忽略。每个 Argument 能包含一个或多个ReplaceString，但不能大于 255 字节。-I 标志同样打开 -x 标志。

注：-I（大写 i）和 -i 标志是互相排斥的；最后指定的标志生效。

<strong>eg:find /root –name *ts.c | xargs –I {} command {}</strong>

<strong>-l[Number]</strong>

（小写的 L）。废弃的标志。请使用 -L 标志。

如果没有指定 Number 参数，使用缺省值 1。-l 标志同样打开 -x 标志。

注： -L、-I（小写的 L）和 -n 标志是互相排斥的；最后指定的标志生效。

<strong>-L Number</strong>

用从标准输入读取的指定行数的非空参数运行 Command 命令。如果保留少于指定的 Number，Command 参数的最后调用可以有少数几个参数行。行以第一个换行字符结束，除非该行的最后一个字符是一个空格或制表符。后续的空格表示延续至下一个非空行。

注： -L、-I（小写的 L）和 -n 标志是互相排斥的；最后指定的标志生效。

<strong>-n Number</strong>

运行 Command 参数，且使用尽可能多的标准输入自变量，直到 Number 参数指定的最大值。如果满足以下条件，则 xargs 命令使用更少的自变量：

如果积累的命令行长度超出了由 -s Size 标志指定的字节。

最后的迭代有少于 Number（但是非零）的自变量保留。

注： -L、-I（小写的 L）和 -n 标志是互相排斥的；最后指定的标志生效。

<strong>-p</strong>

询问是否运行 Command 参数。它显示构造的命令行，后跟一个 ?...（问号和省略号）提示。输入肯定的、特定于语言环境的响应以运行Command 参数。任何其它响应都会引起 xargs 命令跳过那个特定的参数调用。每个调用都将询问您。 -p 标志同样打开 -t 标志。

<strong>　-s Size</strong>

设置构造的 Command 行的最大总大小。Size 参数必须是正整数。如果满足以下条件，则使用更少的自变量：

自变量的总数超出 -n 标志指定的自变量数。

总行数超出 -L 或 -I（小写 L）标志指定的行数。

累积由 Size 参数指定的字节数之前达到 EOF。

<strong>　-t</strong>

启用跟踪方式，并在运行之前将构造的 Command 行回送到标准错误。

<strong>　-x</strong>

如果有任何 Command 行大于 -s Size 标志指定的字节数，停止运行 xargs 命令。如果指定 -I（大写 i）或 -l（小写L）标志，则打开 -x 标志。如果没有指定 -i、-I（大写 i）、-l（小写 L）、-L 或 -n 标志，则 Command行的总长度必须在 -s Size 标志指定的限制内。

&nbsp;

<span data-edit-id="2285387:2285387:4">退出状态</span>

该命令返回下列退出值：

0

所有 Command 参数的调用都返回退出状态 0。

1-125

不能汇编满足指定需求的命令行，一个或多个 Command 参数的调用返回一个非零的退出状态，或发生一些其它的错误。

126

Command 已找到但不能被调用。

127

找不到 Command。

如果不能汇编满足指定需求的命令行，则不能调用这个命令，命令的调用被一个信号终止，或以退出状态 255 退出。xargs 命令将写一条诊断消息并退出而不处理任何保留的输入。
