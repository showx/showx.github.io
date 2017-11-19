---
layout: post
title: MongoDB (2) 高级查询条件操作符
date: 2015-04-08 10:07
author: admin
comments: true
categories: []
---
MongoDB 支持多种复杂的查询方式，能实现大多数 T-SQL 功能，远不是 Key-Value 之类的 NoSQL DB 所能比拟的。
Conditional Operators :<strong> $slice </strong>//切片
Conditional Operators : <strong>$lt</strong> &lt;, <strong>$lte</strong> &lt;=, <strong>$gt</strong> &gt;, <strong>$gte</strong> &gt;=
Conditional Operator : <strong>$ne</strong> //不等于
Conditional Operator : <strong>$in</strong> //属于
Conditional Operator : <strong>$nin</strong> //不属于
Conditional Operator : <strong>$mod</strong> //取模运算
Conditional Operator: <strong>  $all</strong>  //全部属于
Conditional Operator : <strong>$size</strong> //数量
Conditional Operator: <strong>$exists</strong> //字段存在
Conditional Operator: <strong>$type</strong> //字段类型
Conditional Operator: <strong>$or</strong> // 或
<strong>Regular Expressions</strong> //正则表达式
<strong>Value in an Array</strong> // 数组中的值
Conditional Operator: <strong>$elemMatch</strong> //要素符合
<strong>Value in an Embedded Object</strong> //内嵌对象中的值
Meta operator: <strong>$not</strong> //不是
Javascript Expressions and<strong> $where</strong> //
<strong>sort() </strong>//排序
<strong>limit()</strong> //限制取数据条数
<strong>skip() </strong>//跳过一定数值开始取
<strong>snapshot() </strong>//
<strong>count() </strong>// 数量
<strong>group()</strong> //分组

<strong>准备数据</strong>
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<div>In [9]: db
Out[9]: Database(Connection('localhost', 27017), u'test')
In [10]: table = db.table_abeen
In [11]: table
Out[11]: Collection(Database(Connection('localhost', 27017), u'test'), u'table_abeen')

In [12]: table.insert({"name":"abeen", "age":27})
Sun Aug 8 23:14:20 connection accepted from 127.0.0.1:46143 #27
Out[12]: ObjectId('4c5f9cbc421aa90fb9000000')

In [14]: table.insert({"name":"shanshan", "age":22})
Out[14]: ObjectId('4c5f9ccb421aa90fb9000001')</div>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
<strong>Conditional Operator: $ne (not equal)</strong>
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<div>//查找name不等于abeen的信息
In [24]: list(table.find({"name":{"$ne":"abeen"}}))
Out[24]:
[{u'_id': ObjectId('4c5f9ccb421aa90fb9000001'),
u'age': 22,
u'name': u'shanshan'},
{u'_id': ObjectId('4c5f9d2d421aa90fb9000002'),
u'age': 22,
u'name': u'shanshan2'},
{u'_id': ObjectId('4c5f9d34421aa90fb9000003'),
u'age': 23,
u'name': u'shanshan3'}]</div>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
<strong>Conditional Operator: $gt $lt(gt= greater than, lt=less than)</strong>
<div class="cnblogs_code">
<div>//查找name不等于abeen,并且age大于22的
In [29]: list(table.find({"name": {"$ne": "abeen"}, "age":{"$gt": 22}}))
Out[29]:
[{u'_id': ObjectId('4c5f9d34421aa90fb9000003'),
u'age': 23,
u'name': u'shanshan3'}]</div>
</div>
<strong>获取子集 $ne  $slice</strong>
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<div>//select "age" from table where name = "abeen"
In [42]: list(table.find({"name": "abeen"}, {"age" : 1}))
Out[42]: [{u'_id': ObjectId('4c5f9cbc421aa90fb9000000'), u'age': 27}]

//get all posts about mongodb without "age"
In [43]: list(table.find({"name": "abeen"}, {"age" : 0}))
Out[43]: [{u'_id': ObjectId('4c5f9cbc421aa90fb9000000'), u'name': u'abeen'}]

//name不等于abeen的"age"信息,取前5条
In [48]: list(table.find({"name": {"$ne":"abeen"}}, {"age":{"$slice":5}}))
//取name信息,从第10条开始取20条
In [54]: list(table.find({}, {"name": {"$slice": [10,20]}}))
//取name信息,从后20条开始取10条
In [55]: list(table.find({}, {"name": {"$slice": [-20,10]}}))</div>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
<strong>取数值范围</strong>
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<div>//age大于23的
In [56]: list(table.find({"age":{"$gt":23}}))
Out[56]: [{u'_id': ObjectId('4c5f9cbc421aa90fb9000000'), u'age': 27, u'name': u'abeen'}]

//age小于23的
In [57]: list(table.find({"age":{"$lt":23}}))
Out[57]:
[{u'_id': ObjectId('4c5f9ccb421aa90fb9000001'),
u'age': 22,
u'name': u'shanshan'},
{u'_id': ObjectId('4c5f9d2d421aa90fb9000002'),
u'age': 22,
u'name': u'shanshan2'}]

//age大于等于23的
In [58]: list(table.find({"age":{"$gte":23}}))
Out[58]:
[{u'_id': ObjectId('4c5f9cbc421aa90fb9000000'), u'age': 27, u'name': u'abeen'},
{u'_id': ObjectId('4c5f9d34421aa90fb9000003'),
u'age': 23,
u'name': u'shanshan3'},
{u'_id': ObjectId('4c5fa2ab421aa90fb9000004'),
u'address': u'da zhong si',
u'age': 23,
u'name': u'shanshan3'}]

//age小于等于23的
In [59]: list(table.find({"age":{"$lte":23}}))
Out[59]:
[{u'_id': ObjectId('4c5f9ccb421aa90fb9000001'),
u'age': 22,
u'name': u'shanshan'},
{u'_id': ObjectId('4c5f9d2d421aa90fb9000002'),
u'age': 22,
u'name': u'shanshan2'},
{u'_id': ObjectId('4c5f9d34421aa90fb9000003'),
u'age': 23,
u'name': u'shanshan3'},
{u'_id': ObjectId('4c5fa2ab421aa90fb9000004'),
u'address': u'da zhong si',
u'age': 23,
u'name': u'shanshan3'}]</div>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
<strong>Conditional Operator: $gt</strong>
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<div>//22 &lt; age &lt; 25的
In [63]: list(table.find({"age": {"$gt":22, "$lt":25}}))
Out[63]:
[{u'_id': ObjectId('4c5f9d34421aa90fb9000003'),
u'age': 23,
u'name': u'shanshan3'},
{u'_id': ObjectId('4c5fa2ab421aa90fb9000004'),
u'address': u'da zhong si',
u'age': 23,
u'name': u'shanshan3'}]</div>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
<strong>Conditional Operator : $in</strong>
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<div>//name在列表["abeen","ab","b"]里面的
In [67]: list(table.find({"name":{"$in":["abeen","ab","b"]}}))
Out[67]: [{u'_id': ObjectId('4c5f9cbc421aa90fb9000000'), u'age': 27, u'name': u'abeen'}]

//name在列表["abeen","ab","b"]里面的,限制取1条数据
In [69]: list(table.find({"name":{"$in":["abeen","ab","b","shanshan"]}}).limit(1))
Out[69]: [{u'_id': ObjectId('4c5f9cbc421aa90fb9000000'), u'age': 27, u'name': u'abeen'}]</div>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
<strong>Conditional Operator : $nin (not in)</strong>
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<div>//name不在列表["abeen","ab","b"]里面的
In [70]: list(table.find({"name":{"$nin":["abeen","ab","b"]}}))
Out[70]:
[{u'_id': ObjectId('4c5f9ccb421aa90fb9000001'),
u'age': 22,
u'name': u'shanshan'},
{u'_id': ObjectId('4c5f9d2d421aa90fb9000002'),
u'age': 22,
u'name': u'shanshan2'},
{u'_id': ObjectId('4c5f9d34421aa90fb9000003'),
u'age': 23,
u'name': u'shanshan3'},
{u'_id': ObjectId('4c5fa2ab421aa90fb9000004'),
u'address': u'da zhong si',
u'age': 23,
u'name': u'shanshan3'}]</div>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
<strong>Conditional Operator: $mod</strong>
<div class="cnblogs_code">
<div>// 查找age除10模等于1的
In [71]: list(table.find({"age":{"$mod":[10,1]}}))</div>
</div>
<strong>Conditional Operator: $all</strong>
<div class="cnblogs_code">
<div>//取name包含所有["abeen","a","b"]的信息
In [77]: list(table.find({"name":{"$all":["abeen","a","b"]}}))
Out[77]:
[{u'_id': ObjectId('4c5facc6421aa90fb9000005'),
u'name': [u'abeen', u'a', u'b', u'e', u'e', u'n']}]</div>
</div>
<strong>Conditional Operator: $size</strong>
<div class="cnblogs_code">
<div>//取name元素数和$size数相同的信息
In [81]: list(table.find({"name":{"$size": 6}}))
Out[81]:
[{u'_id': ObjectId('4c5facc6421aa90fb9000005'),
u'name': [u'abeen', u'a', u'b', u'e', u'e', u'n']}]</div>
</div>
<strong>Conditional Operator: $exists</strong>
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<div>//取name存在的信息
In [83]: list(table.find({"name":{"$exists": True}}))
Out[83]:
[{u'_id': ObjectId('4c5f9cbc421aa90fb9000000'), u'age': 27, u'name': u'abeen'},
{u'_id': ObjectId('4c5f9ccb421aa90fb9000001'),
u'age': 22,
u'name': u'shanshan'},
{u'_id': ObjectId('4c5f9d2d421aa90fb9000002'),
u'age': 22,
u'name': u'shanshan2'},
{u'_id': ObjectId('4c5f9d34421aa90fb9000003'),
u'age': 23,
u'name': u'shanshan3'},
{u'_id': ObjectId('4c5fa2ab421aa90fb9000004'),
u'address': u'da zhong si',
u'age': 23,
u'name': u'shanshan3'},
{u'_id': ObjectId('4c5facc6421aa90fb9000005'),
u'name': [u'abeen', u'a', u'b', u'e', u'e', u'n']}]
//取name不存在信息
In [84]: list(table.find({"name":{"$exists": False}}))
Out[84]: []</div>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
<strong>Conditional Operator: $type</strong>
<div class="cnblogs_code">
<div>//name类型为字符串的
In [88]: list(table.find({"name":{"$type": 2}}))</div>
</div>
type对应该类型表如下:

<img src="http://pic002.cnblogs.com/img/abeen/201008/2010080918101542.jpg" alt="" />

<strong>Conditional Operator: $or</strong>
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<div>//查找name等于abeen或等于shanshan的信息
In [95]: list(table.find({"$or" :[{"name": "abeen"}, {"name":"shanshan"}]}))
Out[95]: []

//查找age等于22,或name等于abeen或等于shanshan的信息
In [96]: list(table.find({"age":22, "$or" :[{"name": "abeen"}, {"name":"shanshan"}]}))
Out[96]: []</div>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
<strong>Regular Expressions</strong>
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<div>//利用正则查询
In [114]: list(table.find({"name": {"$regex": r".*ee.*"}}))
Out[114]:
[{u'_id': ObjectId('4c5f9cbc421aa90fb9000000'), u'age': 27, u'name': u'abeen'},
{u'_id': ObjectId('4c5facc6421aa90fb9000005'),
u'name': [u'abeen', u'a', u'b', u'e', u'e', u'n']}]
正则表达式标记:
i: 忽略大小写。
m: 默认为单行处理，此标记表示多行。
x: 扩展。</div>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
<strong>Conditional Operator: $elemMatch</strong>
<div class="cnblogs_code">
<div>In [135]: list(table.find( { "age" : {"$elemMatch": {"name": {"$regex": r".*ee.*"},
"age":{"$gt":22}}}}))</div>
</div>
<strong>Value in an Embedded Object</strong>
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<div>//查找内部对象信息,
//查找内部对象info的name等于abeen的信息
In [217]: list(table.find({"info.name": "abeen"}))
Out[217]:
[{u'_id': ObjectId('4c5fcd7e421aa90fb9000007'),
u'info': {u'address': u'beijing', u'age': 28, u'name': u'abeen'},
u'name': u'abeen_object'}]</div>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
<strong>Meta operator: $not</strong>
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<div>//查询age不在大于23的范围内的信息
In [160]: list(table.find({"age": {"$not":{"$gt": 23}}}))
Out[160]:
[{u'_id': ObjectId('4c5f9ccb421aa90fb9000001'),
u'age': 22,
u'name': u'shanshan'},
{u'_id': ObjectId('4c5f9d2d421aa90fb9000002'),
u'age': 22,
u'name': u'shanshan2'},
{u'_id': ObjectId('4c5f9d34421aa90fb9000003'),
u'age': 23,
u'name': u'shanshan3'},
{u'_id': ObjectId('4c5fa2ab421aa90fb9000004'),
u'address': u'da zhong si',
u'age': 23,
u'name': u'shanshan3'},
{u'_id': ObjectId('4c5facc6421aa90fb9000005'),
u'name': [u'abeen', u'a', u'b', u'e', u'e', u'n']}]</div>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
<strong>Javascript Expressions and $where</strong>
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<div>//age大于23的
In [164]: list(table.find({"age": {"$gt":23}}))
Out[164]:
[{u'_id': ObjectId('4c5f9cbc421aa90fb9000000'), u'age': 27, u'name': u'abeen'},
{u'_id': ObjectId('4c5fae95421aa90fb9000006'), u'age': 25, u'name': u''}]
//age大于23的
In [165]: list(table.find({"$where": "this.age &gt; 23"}))
Out[165]:
[{u'_id': ObjectId('4c5f9cbc421aa90fb9000000'), u'age': 27, u'name': u'abeen'},
{u'_id': ObjectId('4c5fae95421aa90fb9000006'), u'age': 25, u'name': u''}]


</div>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
<strong>//skip()  limit()</strong>
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<div>In [204]: result = table.find().skip(2).limit(3)

In [205]: for r in result : print r
{u'age': 22, u'_id': ObjectId('4c5f9d2d421aa90fb9000002'), u'name': u'shanshan2'}
{u'age': 23, u'_id': ObjectId('4c5f9d34421aa90fb9000003'), u'name': u'shanshan3'}
{u'age': 23, u'_id': ObjectId('4c5fa2ab421aa90fb9000004'), u'name': u'shanshan3',
u'address': u'da zhong si'}</div>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
&nbsp;
