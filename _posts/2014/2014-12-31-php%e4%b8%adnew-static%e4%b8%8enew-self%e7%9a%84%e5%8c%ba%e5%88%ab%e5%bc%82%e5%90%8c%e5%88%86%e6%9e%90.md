---
layout: post
title: PHP中new static()与new self()的区别异同分析
date: 2014-12-31 16:18
author: admin
comments: true
categories: []
---
本文实例讲述了PHP中new static()与new self()的区别异同，相信对于大家学习PHP程序设计能够带来一定的帮助。
问题的起因是本地搭建一个站。发现用PHP 5.2 搭建不起来，站PHP代码里面有很多5.3以上的部分，要求更改在5.2下能运行。
改着改着发现了一个地方
?
1
return new static($val);
这尼玛是神马，只见过
?
1
return new self($val);
于是上网查了下，他们两个的区别。
self - 就是这个类，是代码段里面的这个类。
static - PHP 5.3加进来的只得是当前这个类，有点像$this的意思，从堆内存中提取出来，访问的是当前实例化的那个类，那么 static 代表的就是那个类。
还是看看老外的专业解释吧：
self refers to the same class whose method the new operation takes place in.
static in PHP 5.3's late static bindings refers to whatever class in the hierarchy which you call the method on.
In the following example, B inherits both methods from A. self is bound to A because it's defined in A's implementation of the first method, whereas static is bound to the called class (also see  get_called_class() ).
?
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
class A {
  public static function get_self() {
    return new self();
  }
 
  public static function get_static() {
    return new static();
  }
}
 
class B extends A {}
 
echo get_class(B::get_self()); // A
echo get_class(B::get_static()); // B
echo get_class(A::get_static()); // A
这个例子基本上一看就懂了吧。
原理了解了，但是问题还没有解决，如何解决掉 return new static($val); 这个问题呢？
其实也简单就是用 get_class($this); 代码如下：
?
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
class A {
  public function create1() {
    $class = get_class($this);
　　　　return new $class();
  }
  public function create2() {
    return new static();
  }
}
 
class B extends A {
 
}
 
$b = new B();
var_dump(get_class($b->create1()), get_class($b->create2()));
 
/*
The result 
string(1) "B"
string(1) "B"
*/

