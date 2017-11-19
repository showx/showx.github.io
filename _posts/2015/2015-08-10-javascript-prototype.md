---
layout: post
title: JavaScript prototype
date: 2015-08-10 09:31
author: admin
comments: true
categories: []
---
用过JavaScript的同学们肯定都对prototype如雷贯耳，但是这究竟是个什么东西却让初学者莫衷一是，只知道函数都会有一个prototype属性，可以为其添加函数供实例访问，其它的就不清楚了，最近看了一些 JavaScript高级程序设计，终于揭开了其神秘面纱。

每个函数都有一个prototype属性，这个属性是指向一个对象的引用，这个对象称为原型对象，原型对象包含函数实例共享的方法和属性，也就是说将函数用作构造函数调用（使用new操作符调用）的时候，新创建的对象会从原型对象上继承属性和方法。
<h3>私有变量、函数</h3>
在具体说prototype前说几个相关的东东，可以更好的理解prototype的设计意图。之前写的一篇<a href="http://www.cnblogs.com/dolphinX/p/3269145.html" target="_blank">JavaScript 命名空间</a>博客提到过JavaScript的函数作用域，在函数内定义的变量和函数如果不对外提供接口，那么外部将无法访问到，也就是变为私有变量和私有函数。
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<pre>function Obj(){
                var a=0; //私有变量
                var fn=function(){ //私有函数
                    
                }
            }</pre>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
这样在函数对象Obj外部无法访问变量a和函数fn，它们就变成私有的，只能在Obj内部使用，即使是函数Obj的实例仍然无法访问这些变量和函数
<div class="cnblogs_code">
<pre>var o=new Obj();
            console.log(o.a); //undefined
            console.log(o.fn); //undefined</pre>
</div>
<h3>静态变量、函数</h3>
当定义一个函数后通过 “.”为其添加的属性和函数，通过对象本身仍然可以访问得到，但是其实例却访问不到，这样的变量和函数分别被称为静态变量和静态函数，用过Java、C#的同学很好理解静态的含义。
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<pre>function Obj(){
                
            }
            
            Obj.a=0; //静态变量
            
            Obj.fn=function(){ //静态函数
                    
            }
            
            console.log(Obj.a); //0
            console.log(typeof Obj.fn); //function
            
            var o=new Obj();
            console.log(o.a); //undefined
            console.log(typeof o.fn); //undefined</pre>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
<h3>实例变量、函数</h3>
在面向对象编程中除了一些库函数我们还是希望在对象定义的时候同时定义一些属性和方法，实例化后可以访问，JavaScript也能做到这样
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<pre>function Obj(){
                this.a=[]; //实例变量
                this.fn=function(){ //实例方法
                    
                }
            }
            
            console.log(typeof Obj.a); //undefined
            console.log(typeof Obj.fn); //undefined
            
            var o=new Obj();
            console.log(typeof o.a); //object
            console.log(typeof o.fn); //function</pre>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
这样可以达到上述目的，然而
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<pre>function Obj(){
                this.a=[]; //实例变量
                this.fn=function(){ //实例方法
                    
                }
            }
            
            var o1=new Obj();
            o1.a.push(1);
            o1.fn={};
            console.log(o1.a); //[1]
            console.log(typeof o1.fn); //object
            var o2=new Obj();
            console.log(o2.a); //[]
            console.log(typeof o2.fn); //function</pre>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
上面的代码运行结果完全符合预期，但同时也说明一个问题，在o1中修改了a和fn，而在o2中没有改变，由于数组和函数都是对象，是引用类型，这就说明o1中的属性和方法与o2中的属性与方法虽然同名但却不是一个引用，而是对Obj对象定义的属性和方法的一个复制。

这个对属性来说没有什么问题，但是对于方法来说问题就很大了，因为方法都是在做完全一样的功能，但是却又两份复制，如果一个函数对象有上千和实例方法，那么它的每个实例都要保持一份上千个方法的复制，这显然是不科学的，这可肿么办呢，prototype应运而生。
<h3>prototype</h3>
无论什么时候，只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个prototype属性，默认情况下prototype属性会默认获得一个constructor(构造函数)属性，这个属性是一个指向prototype属性<strong>所在函数</strong>的指针，有些绕了啊，写代码、上图！
<div class="cnblogs_code">
<pre>function Person(){
                
            }</pre>
</div>
<a href="http://images.cnitblog.com/blog/349217/201308/27224134-32838e6dd26f4a33ac0d859e9fec86f0.png"><img title="image" src="http://images.cnitblog.com/blog/349217/201308/27224136-4b97c5495fe64f46abddc78f50cb8097.png" alt="image" width="314" height="190" border="0" /></a>

根据上图可以看出Person对象会自动获得prototyp属性，而prototype也是一个对象，会自动获得一个constructor属性，该属性正是指向Person对象。

当调用构造函数创建一个实例的时候，实例内部将包含一个内部指针（很多浏览器这个指针名字为__proto__）指向构造函数的prototype，这个连接存在于实例和构造函数的prototype之间，而不是实例与构造函数之间。
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<pre>function Person(name){
                this.name=name;
            }
            
            Person.prototype.printName=function(){
                alert(this.name);
            }
            
            var person1=new Person('Byron');
            var person2=new Person('Frank');</pre>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
&nbsp;

<a href="http://images.cnitblog.com/blog/349217/201308/27224136-12d8c3cc713e44c8a6e310eb2e02430a.png"><img title="image" src="http://images.cnitblog.com/blog/349217/201308/27224146-31fb37b7c1d0485eaf25500b30fe739e.png" alt="image" width="459" height="186" border="0" /></a>

Person的实例person1中包含了name属性，同时自动生成一个__proto__属性，该属性指向Person的prototype，可以访问到prototype内定义的printName方法，大概就是这个样子的

<a href="http://images.cnitblog.com/blog/349217/201308/27224146-a78177522b814194a4eaaa840e1b4aea.png"><img title="image" src="http://images.cnitblog.com/blog/349217/201308/27224151-5a1aff0645604df58e40abc919bc836d.png" alt="image" width="478" height="320" border="0" /></a>

写段程序测试一下看看prototype内属性、方法是能够共享
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<pre>function Person(name){
                this.name=name;
            }
            
            Person.prototype.share=[];
            
            Person.prototype.printName=function(){
                alert(this.name);
            }
            
            var person1=new Person('Byron');
            var person2=new Person('Frank');
            
            person1.share.push(1);
            person2.share.push(2);
            console.log(person2.share); //[1,2]</pre>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
果不其然！实际上当代码读取某个对象的某个属性的时候，都会执行一遍搜索，目标是具有给定名字的属性，搜索首先从对象实例开始，如果在实例中找到该属性则返回，如果没有则查找prototype，如果还是没有找到则继续递归prototype的prototype对象，直到找到为止，如果递归到object仍然没有则返回错误。同样道理如果在实例中定义如prototype同名的属性或函数，则会覆盖prototype的属性或函数。
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<pre>function Person(name){
                this.name=name;
            }
            
            Person.prototype.share=[];

            var person=new Person('Byron');
            person.share=0;
            
            console.log(person.share); //0而不是prototype中的[]</pre>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
<h3>构造简单对象</h3>
当然prototype不是专门为解决上面问题而定义的，但是却解决了上面问题。了解了这些知识就可以构建一个科学些的、复用率高的对象，如果希望实例对象的属性或函数则定义到prototype中，如果希望每个实例单独拥有的属性或方法则定义到this中，可以通过构造函数传递实例化参数。
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<pre>function Person(name){
                this.name=name;
            }
            
            Person.prototype.share=[];
            
            Person.prototype.printName=function(){
                alert(this.name);
            }</pre>
<div class="cnblogs_code_toolbar"></div>
</div>
