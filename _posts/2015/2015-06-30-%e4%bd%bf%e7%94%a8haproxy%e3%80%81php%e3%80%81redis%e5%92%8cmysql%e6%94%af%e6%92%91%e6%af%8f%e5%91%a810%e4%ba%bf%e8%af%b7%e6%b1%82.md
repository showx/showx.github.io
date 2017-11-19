---
layout: post
title: 使用HAProxy、PHP、Redis和MySQL支撑每周10亿请求
date: 2015-06-30 16:04
author: admin
comments: true
categories: []
---
在公司的发展中，保证服务器的可扩展性对于扩大企业的市场需要具有重要作用，因此，这对架构师提出了一定的要求。Octivi联合创始人兼软件架构师<a href="http://labs.octivi.com/author/aorfin/">Antoni Orfin</a>将向你介绍一个非常简单的架构，使用HAProxy、PHP、Redis和MySQL就能支撑每周10亿请求。同时，你还能了解项目未来的横向扩展途径及常见的模式。

<img src="https://dn-linuxcn.qbox.me/data/attachment/album/201411/17/104839nva6wzct6l6b0lju.jpg" alt="" width="700" height="623" />
<h3>状态</h3>
<ul>
	<li>服务器
<ul>
	<li>3个应用程序节点</li>
	<li>2个MySQL+1个备份</li>
	<li>2个Redis</li>
</ul>
</li>
	<li>应用程序
<ul>
	<li>应用程序每周处理10亿请求</li>
	<li>峰值700请求/秒的单Symfony2实例（平均工作日约550请求/秒）</li>
	<li>平均响应时间30毫秒</li>
	<li>Varnish，每秒请求超过1.2万次（压力测试过程中获得）</li>
</ul>
</li>
	<li>数据存储
<ul>
	<li>Redis储存了1.6亿记录，数据体积大约100GB，同时它是我们的主要数据存储</li>
	<li>MySQL储存了3亿记录，数据体积大约300GB，通常情况下它作为三级缓存层</li>
</ul>
</li>
</ul>
<h3>平台</h3>
<img src="https://dn-linuxcn.qbox.me/data/attachment/album/201411/17/104840ab8fbxfyyrbf1v6z.jpg" alt="" border="0" />
<ul>
	<li>监视：
<ul>
	<li>Icinga</li>
	<li>Collectd</li>
</ul>
</li>
</ul>
<ul>
	<li>应用程序
<ul>
	<li>HAProxy + Keepalived</li>
	<li>Varnish</li>
	<li>PHP（PHP-FPM）+ Symfony2 Framework</li>
</ul>
</li>
	<li>数据存储
<ul>
	<li>MySQL（主从配置），使用HAProxy做负载均衡</li>
	<li>Redis （主从配置）</li>
</ul>
</li>
</ul>
<h2><strong>背景</strong></h2>
大约1年前，一个朋友找到我并提出了一个苛刻的要求：它们是一个飞速发展的电子商务初创公司，而当时已经准备向国际发展。介于那个时候他们仍然是一个创业公司，初始解决方案必须符合所谓的成本效益，因此也就无法在服务器上投入更多的资金。遗留系统使用了标准的LAMP堆栈，因此他们拥有一个强力的PHP开发团队。如果必须引入新技术的话，那么这些技术必须足够简单，不会存在太多架构上的复杂性；那么，他们当下的技术团队就可以对应用进行长期的维护。

为了满足他们扩展到下一个市场的需求，架构师必须使用可扩展理念进行设计。首先，我们审视了他们的基础设施：

<img src="https://dn-linuxcn.qbox.me/data/attachment/album/201411/17/104840oia5o3hmjua5k3ra.jpg" alt="" border="0" />

<a href="https://dn-linuxcn.qbox.me/data/attachment/album/201411/17/104841ptcxtnpoixt20ouq.jpg" target="_blank"><img src="https://dn-linuxcn.qbox.me/data/attachment/album/201411/17/104841ptcxtnpoixt20ouq.jpg" alt="" border="0" /></a>

老系统使用了单模块化设计思路，底层是一些基于PHP的Web应用程序。这个初创公司有许多所谓的前端网站，它们大多都使用了独立的数据库，并共享了一些支撑业务逻辑的通用代码。毫不客气的说，长期维护这种应用程序绝对是一个噩梦：因为随着业务的发展，有些代码必须被重写，这样的话，修改某个网站将不可避免导致业务逻辑上的不一致，这样一来，他们不得不在所有Web应用程序上做相同的修改。

通常情况下，这该归结于项目管理问题，管理员必须对横跨多个代码库的那些代码负责。基于这个观点，整改第一步就是提取核心的业务关键功能，并将之拆分为独立的服务（这也是本文的一个重点部分），也就是所谓的面向服务架构，在整个系统内遵循“separation of concern”原则。每个服务只负责一个业务逻辑，同时也要明确更高等级的业务功能。举个形象的例子也就是，这个系统可能是个搜索引擎、一个销售系统等。

前端网站通过REST API与服务交互，响应则基于JSON格式。为了简单起见，我们没有选择SOAP，一个开发者比较无爱的协议，因为谁都不愿意解析一堆的XML。

提取一些不会经常处理的服务，比如身份验证和会话管理。这是非常必要的一个环节，因为它们的处理等级比较高。前端网站负责这个部分，只有它们可以识别用户。这样一来我们可以保持服务的足够简单，在处理扩展和代码相关问题时都具有巨大的优势，可谓各司其职，完美无缺。
<h3>带来的好处：</h3>
<ul>
	<li>独立子系统（服务）可以便捷的在不同团队中开发，开发者互不干涉，效率理所当然提升。</li>
	<li>身份验证和会话不会通过它们来管理，因此它们造成的扩展问题不翼而飞。</li>
	<li>业务逻辑被区分，不同的前端网站不会再存在功能冗余。</li>
	<li>显著地提高了服务的可用性。</li>
</ul>
<h3>共生的缺点：</h3>
为系统管理员带来更大的工作量。鉴于服务都使用了独立的基础设施，这将给管理员带来更多需要关注的地方。

很难保持向后兼容。在一年的维护之后，API方法中发生了数不尽的变化。因此问题发生了，它们必将破坏向后兼容，因为每个网站的代码都可能发生变化，还可能存在许多技术人员同时修改一个网站的情况……然而，一年后，所有方法匹配的仍然是项目开始时建立的文档。
<h2>应用程序层</h2>
<img src="https://dn-linuxcn.qbox.me/data/attachment/album/201411/17/104841klwgvbmr7bx122ri.jpg" alt="" border="0" />

着眼请求工作流，第一层是应用程序。HAProxy负载均衡器、Varnish和Symfony2应用程序都在这一层。来自前端网站的请求首先会传递给HAProxy，随后负载均衡器将把他分给不同的节点。
<h3>应用程序节点配置</h3>
<ul>
	<li>Xeon E5-1620@3.60GHz，64GB RAM，SATA</li>
	<li>Varnish</li>
	<li>Apache2</li>
	<li>PHP 5.4.X（PHP-FPM），使用APC字节码缓存</li>
</ul>
我们购买了3个这样的服务器，N+1冗余配置的active-active模式，备份服务器同样处理请求。因为性能不是首要因素，我们为每个节点配置独立的Varnish以降低缓存hit，同时也避免了单点故障（SPOF）。在这个项目中，我们更重视可用性。因为一个前端网站服务器中使用了Apache 2，我们保留了这个堆栈。这样一来，管理员不会困扰于太多新加入的技术。
<h3>Symfony2应用程序</h3>
应用程序本身基于Symfony2建立，这是一个PHP全堆栈框架，提供了大量加速开发的组件。作为基于复杂框架的典型REST服务可能受到很多人质疑，这里为你细说：
<ul>
	<li><strong>对 PHP/Symfony 开发者友好。</strong>客户端IT团队由PHP开发者组成，添加新技术将意味必须招聘新的开发者，因为业务系统必须做长时间的维护。</li>
	<li><strong>清晰的项目结构。</strong> PHP/Symfony虽然从来都不是必需品，但却是许多项目的默认选择。引入新的开发者将非常方便，因为对他们来说代码非常友好。</li>
	<li><strong>许多现成的组件。</strong>遵循DRY思想……没有人愿意花力气去做重复的工作，我们也不例外。我们使用了大量的Symfony2 Console Component，这个框架非常有利于做CLI命令，以及应用程序性能分析（debug工具栏）、记录器等。</li>
</ul>
在选用Symfony2之前，我们做了大量的性能测试以保证应用程序可以支撑计划流量。我们制定了概念验证，并使用JMeter执行，我们得到了让人满意的结果——每秒700请求时响应时间可以控制在50毫秒。这些测试给了我们足够的信心，让我们坚信，即使Symfony2这样复杂的框架也可以得到理想的性能。
<h3>应用程序分析与监控</h3>
我们使用Symfony2工具来监视应用程序，在收集指定方法执行时间上表现的非常不错，特别是那些与第三方网络服务交互的操作。这样一来，我们可以发现架构中潜在的弱点，找出应用程序中最耗时的部分。

冗长的日志同样是不可缺少的一部分，我们使用PHP Monolog库把这些日志处理成优雅的log-lines，便于开发者和管理员理解。这里需要注意的是尽可能多地添加细节，越详细越好，我们使用了不同的日志等级：
<ul>
	<li><strong>Debug，</strong>可能会发生的事情。比如，请求信息在调用前会传送给一个外部Web服务；事情发生后从API调用响应。</li>
	<li><strong>Error，</strong>当错误发生时请求流并未被终止，比如第三方API的错误响应。</li>
	<li><strong>Critical，</strong>应用程序崩溃的瞬间。</li>
</ul>
因此，你可以清晰地了解Error和Critical信息。而在开发/测试环境中，Debug信息同样被记录。同时，日志被存储在不同的文件中，也就是Monolog库下的“channels”。系统中有一个主日志文件，记录了所有应用程序级错误，以及各个channel的短日志，从单独的文件中记录了来自各个channel的详细日志。
<h3>扩展性</h3>
扩展平台的应用程序层并不困难，HAProxy性能并不会在短时间耗尽，唯一需要考虑的就是如何冗余以避免单点故障。因此，当下需要做的只是添加下一个应用程序节点。
<h2>数据层</h2>
我们使用Redis和MySQL存储所有的数据，MySQL更多作为三级缓存层，而Redis则是系统的主要数据存储。
<h3>Redis</h3>
在系统设计时，我们基于以下几点来选择满足计划需求的数据库：
<ul>
	<li>在存储大量数据时不会影响性能，大约2.5亿记录</li>
	<li>通常情况下多是基于特定资源的简单GET请求，没有查找及复杂的SELECT操作</li>
	<li>在单请求时尽可能多的获得资源以降低延时</li>
</ul>
在经过一些调查后，我们决定使用Redis
<ul>
	<li>大部分我们执行的操作都具有 O（1）或O（N）复杂性， N是需要检索键的数量，这意味着keyspace大小并不会影响性能。</li>
	<li>通常情况下会使用MGET命令行同时检索100个以上的键，这样可以尽可能的避免网络延时，而不是在循环中做多重GET操作。</li>
</ul>
我们当下拥有两个Redis服务器，使用主从复制模式。这两个节点的配置相同，都是Xeon E5-2650v2@2.60GHz，128GB，SSD。内存限制被设置为100GB，通常情况下使用率都是100%。

<img src="https://dn-linuxcn.qbox.me/data/attachment/album/201411/17/104842sglpxlaaiijfi3xz.jpg" alt="" border="0" />

在应用程序并没有耗尽单个Redis服务器的所有资源时，从节点主要作作备份使用，用以保证高有效性。如果主节点宕机，我们可以快速的将应用程序切换到从节点。在维护和服务器迁移时，复制同样被执行——转换一个服务器非常简单。

你可能会猜想当Redis资源被一直耗尽时的情景，所有的键都是持久化类型，大约占90% keyspace，剩余资源被全部被用于TTL过期缓存。当下，keyspace已经被分为两个部分：一个是TTL集（缓存），另一个则是用于持久化数据。感谢“volatile-lru”最大化内存设置的可行性，最不经常使用缓存键会被移除。如此一来，系统就可以一直保持单Redis实例同时执行两个操作——主存储和通用缓存。

使用这个模式必须一直监视“期满”键的数量：
<ol class="linenums">
	<li class="L0"><span class="pln">db</span><span class="pun">.</span><span class="pln">redis1</span><span class="pun">:</span><span class="lit">6379</span><span class="pun">&gt;</span><span class="pln"> info keyspace</span></li>
	<li class="L1"><span class="com"># Keyspace</span></li>
	<li class="L2"><span class="pln">db0</span><span class="pun">:</span><span class="pln">keys</span><span class="pun">=</span><span class="lit">16XXXXXXX</span><span class="pun">,</span><span class="pln">expires</span><span class="pun">=</span><span class="lit">11XXXXXX</span><span class="pun">,</span><span class="pln">avg_ttl</span><span class="pun">=</span><span class="lit">0</span></li>
</ol>
“期满”键数量越接近0情况越危险，这个时候管理员就需要考虑适当的分片或者是增加内存。

我们如何进行监控？这里使用Icinga check，仪表盘会显示数字是否会达到临界点，我们还使用了Redis来可视化“丢失键”的比率。

<img src="https://dn-linuxcn.qbox.me/data/attachment/album/201411/17/104842xt88jwtmm1vfuqq1.jpg" alt="" border="0" />

在一年后，我们已经爱上了Redis，它从未让我们失望，这一年系统从未发生任何宕机情况。
<h3>MySQL</h3>
在Redis之外，我们还使用了传统RDBMS——MySQL。但是区别于他人，我们通常使用它作为三级缓存层。我们使用MySQL存储一些不会经常使用对象以降低Redis的资源使用率，因此它们被放到了硬盘上。这里没有什么可说道的地方，我们只是尽可能地让其保持简单。我们使用了两个MySQL服务器，配置是Xeon E5-1620@3.60GHz，64GB RAM，SSD。两个服务器使用本地、异步的主-主复制。此外，我们使用一个单独的从节点作为备份。
<h3>MySQL的高可用性</h3>
在应用程序中，数据库永远是最难的瓶颈。当前，这里还不需要考虑横向扩展操作，我们多是纵向扩展Redis和MySQL服务器。当下这个策略还存在一定的发展空间，Redis运行在一个126GB内存的服务器上，扩展到256GB也并不困难。当然，这样的服务器也存在劣势，比如快照，又或是是简单的启动——Redis服务器启动需要很长的时间。

在纵向扩展失效后进行的必然是横向扩展，值得高兴的是，项目开始时我们就为数据准备了一个易于分片的结构：

在Redis中，我们为记录使用了4个“heavy”类型。基于数据类型，它们可以分片到4个服务器上。我们避免使用哈希分片，而是选择基于记录类型分片。这种情况下，我们仍然可以运行MGET，它始终在一种类型键上执行。

在MySQL上，结构化的表格非常易于向另一台服务器上迁移——同样基于记录类型（表格）。当然，一旦基于记录类型的分片不再奏效，我们将转移至哈希。
<h2>学到的知识</h2>
<ul>
	<li><strong>不要随便共享你的数据库给别人用。有</strong>一次，一个前端网站希望将会话处理切换到Redis，所以就直接连接了一个，它耗尽了Redis的缓存空间，以至于应用程序再也不能保存下一个缓存键了。所有的缓存应该在 MySQL 的结果集占用大量开销时才进行存储。</li>
	<li><strong>日志越详细越好。</strong>如果log-lines中没有足够的信息，快速Debug问题定位将成为难点。如此一来，你不得不等待一个又一个问题发生，直到找到根结所在。</li>
	<li><strong>架构中使用复杂的框架并不意味着低性能。</strong>许多人惊讶我们使用全堆栈框架来支撑如此流量应用程序，其秘诀在于更聪明的使用工具，否则即使是Node.js也可能变得很慢。选择一个提供良好开发环境的技术，没有人期望使用一堆不友好的工具，这将降低开发团队士气。</li>
</ul>
