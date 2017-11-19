---
layout: post
title: Sphinx 排序模式 SetSortMode
date: 2014-03-11 18:01
author: admin
comments: true
categories: []
---
Sphinx 排序模式 SetSortMode
可使用如下模式对搜索结果排序：
SPH_SORT_RELEVANCE 模式, 按相关度降序排列（最好的匹配排在最前面）
SPH_SORT_ATTR_DESC 模式, 按属性降序排列 （属性值越大的越是排在前面）
SPH_SORT_ATTR_ASC 模式, 按属性升序排列（属性值越小的越是排在前面）
SPH_SORT_TIME_SEGMENTS 模式, 先按时间段（最近一小时/天/周/月）降序，再按相关度降序
SPH_SORT_EXTENDED 模式, 按一种类似SQL的方式将列组合起来，升序或降序排列。
SPH_SORT_EXPR 模式，按某个算术表达式排序。
SPH_SORT_RELEVANCE忽略任何附加的参数，永远按相关度评分排序。所有其余的模式都要求额外的排序子句，子句的语法跟具体的模式有关。SPH_SORT_ATTR_ASC, SPH_SORT_ATTR_DESC以及SPH_SORT_TIME_SEGMENTS这三个模式仅要求一个属性名。SPH_SORT_RELEVANCE模式等价于在扩展模式中按"@weight DESC, @id ASC"排序，SPH_SORT_ATTR_ASC 模式等价于"attribute ASC, @weight DESC, @id ASC"，而SPH_SORT_ATTR_DESC 等价于"attribute DESC, @weight DESC, @id ASC"。
SPH_SORT_TIME_SEGMENTS 模式
在SPH_SORT_TIME_SEGMENTS模式中，属性值被分割成“时间段”，然后先按时间段排序，再按相关度排序。
时间段是根据搜索发生时的当前时间戳计算的，因此结果随时间而变化。
时间段的分法固化在搜索程序中了，但如果需要，也可以比较容易地改变（需要修改源码）。
这种模式是为了方便对Blog日志和新闻提要等的搜索而增加的。使用这个模式时，处于更近时间段的记录会排在前面，但是在同一时间段中的记录又根据相关度排序－这不同于单纯按时间戳排序而不考虑相关度。
SPH_SORT_EXTENDED 模式
在 SPH_SORT_EXTENDED 模式中，您可以指定一个类似SQL的排序表达式，但涉及的属性（包括内部属性）不能超过5个，例如：
@relevance DESC, price ASC, @id DESC
只要做了相关设置，不管是内部属性（引擎动态计算出来的那些属性）还是用户定义的属性就都可以使用。内部属性的名字必须用特殊符号@开头，用户属性按原样使用就行了。在上面的例子里，@relevance和@id是内部属性，而price是用户定义属性。
已知的内置属性：
@id (匹配文档的 ID)
@weight (匹配权值)
@rank (等同 weight)
@relevance (等同 weight)
@random (随机顺序返回结果)
@rank 和 @relevance 只是 @weight 的别名。
SPH_SORT_EXPR 模式
表达式排序模式使您可以对匹配项按任何算术表达式排序，表达式中的项可以是属性值，内部属性（@id和@weight），算术运算符和一些内建的函数。例如：
$cl-&gt;SetSortMode ( SPH_SORT_EXPR,
"@weight + ( user_karma + ln(pageviews) )*0.1" );
支持的运算符和函数如下。它们是模仿MySQL设计的。函数接受参数，参数的数目根据具体函数的不同而不同。
运算符: +, -, *, /, &lt;, &gt; &lt;=, &gt;=, =, &lt;&gt;.
布尔操作符: AND, OR, NOT.
无参函数: NOW().
一元函数（一个参数）: ABS(), CEIL(), FLOOR(), SIN(), COS(), LN(), LOG2(), LOG10(), EXP(), SQRT(), BIGINT().
二元函数（两个参数）: MIN(), MAX(), POW(), IDIV().
其他函数: IF(), INTERVAL(), IN(), GEODIST().
