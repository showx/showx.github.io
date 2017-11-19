---
layout: post
title: javascript prototype
date: 2015-08-20 08:37
author: admin
comments: true
categories: []
---
<b>定义和用法</b>
prototype 属性使您有能力向对象添加属性和方法。
<b>语法</b>
object.prototype.name=value
<b>实例</b>
在本例中，我们将展示如何使用 prototype 属性来向对象添加属性：
&lt;script type="text/javascript"&gt;function employee(name,job,born){this.name=name;this.job=job;this.born=born;}var bill=new employee("Bill Gates","Engineer",1985);employee.prototype.salary=null;bill.salary=20000;document.write(bill.salary);&lt;/script&gt;
输出：
20000


<b>Prototype</b>继承
<b>Prototype</b>
每个对象都有一个[[Prototype]]的内部属性，它的值为null 或者另外一个对象。函数对象都有一
个显示的prototype 属性，它并不是内部[[Prototype]]属性。不同的JS 引擎实现者可以将内部
[[Prototype]]属性命名为任何名字，并且设置它的可见性，只在JS 引擎内部使用。虽然无法在JS
代码中访问到内部[[Prototype]](FireFox 中可以，名字为__proto__因为Mozilla 将它公开了)，但
可以使用对象的isPrototypeOf()方法进行测试，注意这个方法会在整个Prototype 链上进行判断。
使用obj.propName 访问一个对象的属性时，按照下面的步骤进行处理(假设obj 的内部
[[Prototype]]属性名为__proto__):
1. 如果obj 存在propName 属性，返回属性的值，否则
2. 如果obj.__proto__为null，返回undefined，否则
3. 返回obj.__proto__.propName
调用对象的方法跟访问属性搜索过程一样，因为方法的函数对象就是对象的一个属性值。
提示: 上面步骤中隐含了一个递归过程，步骤3 中obj.__proto__是另外一个对象，同样将采用1,
2, 3 这样的步骤来搜索propName 属性。
例如下图所示，object1 将具备属性prop1, prop2, prop3 以及方法fn1, fn2, fn3。图中虚线箭头
表示prototype 链。



这就是基于Prototype 的继承和共享。其中object1 的方法fn2 来自object2，概念上即object2
重写了object3 的方法fn2。
JavaScript 对象应当都通过prototype 链关联起来，最顶层是Object，即对象都派生自Object
类型。
类似C++等面向对象语言用类(被抽象了的类型)来承载方法，用对象(实例化对象)承载属性，
Prototype 语言只用实例化的对象来承载方法和属性。本质区别是前者基于内存结构的描述来实
现继承，后者基于具体的内存块实现。
