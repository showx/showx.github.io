---
layout: post
title: Modernizr.js:为HTML5和CSS3而生！
date: 2015-08-03 10:30
author: admin
comments: true
categories: []
---
modernizr这个JS，在国外的主题里面很多地方都看到，就只记得是为html补充的，有点类似与responsive.js一样。今天搜索到这篇文章，深入的讲解了modernizr.js是为检测浏览器的css3和HTML5的属性而生，从而通过CSS来解决兼容性问题。
个人觉得这种做法明显增加了CSS代码量，而且可能会造成当用户的页面没有启用js的情况下，里面的CSS3效果无法使用，另外就是今后维护比较困难，如果改变了某个样式，那样子CSS的代码类也要改变。个人不推荐使用这种方法来写兼容代码。以下为译文


10年前，只有最尖端的网站设计师会为网页的布局和修饰使用CSS。那时的浏览器对CSS进行布局的支持即不完善又漏洞百出，所以这些人在坚持web标准化的同时，也不得不采用hacks来使得他们的页面在所有浏览器中都能正常显示。其中一个被使用的越来越多的hack技术是浏览器嗅探（browser sniffing），使用javascript里的navigator.userAgent属性来判断用户使用的是什么品牌哪个版本的浏览器。浏览器嗅探技术可以快捷的将代码进行分支，以便针对不同的浏览器应用不同的指令。

今天，以CSS为基础进行的布局已经非常普遍，浏览器们对它的支持也非常的坚实。但是现在CSS3和HTML5来了，历史转了个圈又回到了原地——各个浏览器对这些新技术的支持又开始变得参差不齐了。我们早都习惯了书写整洁的符合标准的代码，也不会再使用CSS hacks或者浏览器嗅探这些不靠谱又低级的技术。我们也相信越来越多的用户会认同网站不必在所有浏览器里都看起来一样的理念。那面对当下这个熟悉的情形（浏览器支持的不同），我们该怎么做呢？简单：使用特征检测（feature detection），这意味着我们不必通过问浏览器“你是谁？”来做出不靠谱的推测。取而代之，我们问浏览器“你能做这个或那个吗”。这么来检测浏览器的能力是很简便的，但一个个的花时间去手工测试依然令人厌烦。此时Modernizr可以帮助我们。

Modernizr：专为HTML5和CSS3开发的功能检测类库

Modernizr是一个开源的JS库，它使得那些基于访客浏览器的不同（指对新标准支持性的差异）而开发不同级别体验的设计师的工作变得更为简单。它使得设计师可以在支持HTML5和CSS3的浏览器中充分利用HTML5和CSS3的特性进行开发，同时又不会牺牲其他不支持这些新技术的浏览器的控制。

当你在网页中嵌入Modernizr的脚本时，它会检测当前浏览器是否支持CSS3的特性，比如 @font-face、border-radius、 border-image、box-shadow、rgba() 等，同时也会检测是否支持HTML5的特性——比如audio、video、本地储存、和新的 <input>标签的类型和属性等。在获取到这些信息的基础上，你可以在那些支持这些功能的浏览器上使用它们，来决定是否创建一个基于JS的fallback，或者对那些不支持的浏览器进行简单的优雅降级。另外，Modernizr还可以令IE支持对HTML5的元素应用CSS样式，这样开发者就可以立即使用这些更富有语义化的标签了。

Modernizr是基于渐进增强理论来开发的，所以它支持并鼓励开发者一层一层的创建他们的网站。一切从一个应用了Javascript的空闲地基开始，一个接一个的添加增强的应用层。因为使用了Modernizr，所以你容易知道浏览器都支持什么。下面我们来看一个通过应用Modernizr来创建尖端网站的实例。

从应用Modernizr开始

首先从www.modernizr.com下载Modernizr的最新的稳定版（目前国内封了Modernizr的官网，我们可以从github上下载。或者更简单点，可以从堂主这里下载最新的1.7版的脚本文件），在官网上你还可以看到它支持检测的所有特性的清单（本文末位会给出这些清单，以便翻不了墙的童鞋可以知道都支持哪些）。下载完最新版本以后（作者写这篇文章的时候用的是1.5版），把它加入页面的<head>区域：

<!DOCTYPE html>  
<html>  
<head>  	
<meta charset="utf-8">  	
<title>My Beautiful Sample Page</title>  	
<script src="modernizr-1.5.min.js"></script>  
</head>  
…
接下来，向<html>元素添加“no-js”的类。

<html>
当Modernizr运行的时候，它会把这个“no-js”的类变为“js”来使你知道它已经运行。Modernizr并不仅仅只做这一件事情，它还会为所有它检测过的特性添加class类，如果浏览器不支持某个特性，它就为该特性对应的类名加上“no-”的前缀。所以，你的<html>元素可能会变得看起来像下面这个样子：

<html>
Modernizr同时还会创建一个JS对象，被命名为“Modernizr”，其内容是为每一个检测完的特性给出的布尔值结果组成的列表。如果浏览器支持新的canvas元素，那么“Modernizr.canvas”的值就是true；如果浏览器不支持这个新元素，那它对应的值就是false。这个JS对象针对某些功能还会包含更为详细的信息，如“Modernizr.video.h264”会告诉你浏览器是否支持这个特殊的编解码器。“Modernizr.inputtypes.search”会告诉你当前浏览器是否支持新的search input类型，等等。

我们的未加工的简单小页面看起来有点像一个准测试系统了，但它具备更好的语义性和可访问性。让我们为它添加一点基本的样式：一点文字格式、颜色和布局以使它更好看。目前位置，没什么新东西，只是为一个语义化结构的HTML页面添加表现样式，看看添加了样式后的页面。

下面，我们要增强这个页面使得它更有意思。我想为头部的h1应用一个奇特的文字效果，把关于检测特性的列表分为两栏，然后将带有一张照片的关于Modernizr的部分的一切都弄到右边去。我还要把围绕页面内容的边框变美点。现在，更给力的CSS3使你可以对一条规则添加更多的属性，如果浏览器不支持这些属性，它会忽略它们。配合使用CSS的层叠（继承），你可以不必依赖Modernizr而使用像“border-radius”这样的新属性。不过，使用Modernizr可以为你提供一些既有手段提供不了的功能：根据浏览器对新东西支持的差异动态修改的<html>的类名。我会通过对我们的页面添加2条新的规则来说明这点：

.borderradius #content {
  	border: 1px solid #141414;
  	-webkit-border-radius: 12px;
  	-moz-border-radius: 12px;
  	border-radius: 12px;  
}  
.boxshadow #content {
  	border: none;
  	-webkit-box-shadow: rgba(0,0,0, .5) 3px 3px 6px;
  	-moz-box-shadow: rgba(0,0,0, .5) 3px 3px 6px;
  	box-shadow: rgba(0,0,0, .5) 3px 3px 6px;  
}
第一条规则为“#content ”元素添加了一个12像素的圆角。但在既有的页面里我们已经为“#content ”元素设置了一个属性值为“2px outset #666”的边框，这在box是直角的时候是蛮好看的，但在圆角情况下就不是了。感谢Modernizr，我可以在浏览器支持“border-radius”的情况下给box设置一个1px的实边。

第二条规则更进步了一点，我们为“#content ”元素添加了一个阴影，并且针对支持“box-shadow”属性的浏览器一并移除了border属性。为什么呢？因为大部分浏览器并不为“边框附带边角”的组合外加阴影这样的效果提供一个好的表现（这是一个应该被注意的浏览器的瑕疵，但我们现在却必须忍受它）。同不使用阴影只使用边框相比，我宁可只使用阴影包围元素。采用这种方式，我可以拥有全世界最好的，准确点的，最好的一种CSS表现：在支持“box-shadow”属性的浏览器里将会呈现一个美妙的阴影；只支持“border-radius”属性的浏览器将会呈现一个好看的圆角薄边框；对于那些这2者都不支持的破烂浏览器，我们会看到一个2像素的直角边框。

下面我们要为header应用一个自定义的特殊字体，下面的是我们目前针对h1的声明，没改动版的：

h1 {
  	color: #e33a89;
  	font: 27px/27px Baskerville, Palatino, "Palatino Linotype",
  	"Book Antiqua", Georgia, serif;
  	margin: 0;
  	text-shadow: #aaa 5px 5px 5px;  
}
这些声明在我们的基础网页里工作良好，27像素的文字大小也很适合我们为h1设置的那些字体的展示。但对于我要用的名叫“Beautiful”的字体来说，27像素就太小了。下面我们要添加其他的规则来使用这个自定义字体：

@font-face {
  	src: url(Beautiful-ES.ttf);
  	font-family: "Beautiful";  
}  

.fontface h1 {
  	font: 42px/50px Beautiful;
  	text-shadow: none;  
}
首先，我们添加“@font-face”声明，并在其中为我们的自定义字体指定路径、文件名和字体名。之后我们用一条CSS语句为我们的h1选择字体样式。你会看到，我这里选择了一个很大的字号，那是因为“Beautiful”字体本身就比其他字体的文字要小很多，所以我们必须要指示浏览器在展示我们自定义字体的时候，给予h1一个很大的字号。

此外，我们漂亮的手写体文字在文字阴影方面存在一些渲染问题，所以我们要在浏览器决定使用自定义文字时取消文字的阴影。另外，关于检测特征部分的列表还需要被分为两栏。为了达到目的，我要使用牛叉的CSS columns 属性，这一属性会使浏览器把列表智能分栏而不会打乱它的顺序，而我们的列表，虽然没有数字序号，却也是按照字母顺序排列的。当然，不是所有的浏览器都支持这一属性，对于那些不支持的浏览器，我们使用浮动来达到2栏的目的——使用了浮动后列表在视觉上不会再按照字母顺序排列，但那也好过什么都没有。

.csscolumns ol.features {
  	-moz-column-count: 2;
  	-webkit-columns: 2;
  	-o-columns: 2;
  	columns: 2;
  }  

.no-csscolumns ol.features {
  	float: left;
  	margin: 0 0 20px;
  }  

.no-csscolumns ol.features li {
  	float: left;
  	width: 180px;  
}
我又一次使用了Modernizr来针对不同的情况设置不同的属性。如果浏览器支持CSS columns，它就会把列表完美的分为2栏，如果不支持，通过Modernizr为<html>添加的“no-csscolumns”类我们也可以用浮动的方式使得列表变为两栏，虽然不那么完美，但也比直接来一个长串的单栏列表强。

这里您可能注意到了我为属性添加了不同的前缀（-moz-、-webkit-、-o-），这是因为不同的浏览器厂商对该功能的实现有不同的定义，所以要实现该功能需要针对不同的浏览器加上它们对应的前缀。

经过这些改变，我们新的页面看起来更好了。

我们将为我们的页面添加进更多的渐进式增强效果来结束这篇教程。基于WebKit的浏览器支持一些更牛叉的特效，如CSS变换、动画和三维转换等。并且我想在最后一个阶段的页面中应用一些这类特效。再一次的，这些属性会被添加进我们既有的CSS中并在不支持它们的浏览器中给忽视。所以，针对这种一方面是渐进增强一方面是不被支持的情况，使用Modernizr是恰当的。

首先设置的：

@-webkit-keyframes spin {
  	  0% { -webkit-transform: rotateY(0); }
  	100% { -webkit-transform: rotateY(360deg); }  
}  

.csstransforms3d.cssanimations section {
  	-webkit-perspective: 1000;  
}
@keyframes规则是新的CSS animations规范中的一部分，目前只有WebKit支持。它容许你针对任何属性声明一个完整的动画路径，并在你喜欢的任何阶段改变它们。想知道关于animations的更多知识，请阅读 W3C Working Draft specification。

下面我们添加使得我们一个元素在三维空间里旋转的代码：

.csstransforms3d.cssanimations section h2 {
  	background-image: url(modernizr-logo.png);
  	overflow: hidden;
  	-webkit-animation: spin 2s linear infinite;  
}
因为logo要转动，且又希望它转的时候和背景相处的融洽些，于是这里用了一个png格式的文件。我还采用了一个“overflow:hidden”属性来隐藏设置了缩进-9999像素的header里的文字。使元素以3D的形式旋转虽然有趣却并不太美观。最终，我们选择使用animation规则，设置它的旋转周期为2秒钟，沿着自身的中轴线旋转，永不停止。

最终的页面看起来很给力，甚至还针对WebKit浏览器设置了好玩的动画。我希望到现在你能明白使用Modernizr可以使你对网站控制的手腕变得多么牛叉，以及它可以令真正的渐进增强变得多么简单。这还不仅仅是Modernizr的全部好处，它还可以帮你建立基于JS的fallbacks以及可以帮你应用html5那些牛掰的新特性。

附，Modernizr检测清单：

1. @font-face
2. Canvas
3. Canvas Text
4. WebGL
5. HTML5 Audio
6. HTML5 Audio formats
7. HTML5 Video
8. HTML5 Video formats
9. rgba()
10. hsla()
11. border-image
12. border-radius
13. box-shadow
14. text-shadow
15. Multiple backgrounds
16. background-size
17. opacity
18. CSS Animations
19. CSS Columns
20. CSS Gradients
21. CSS Reflections
22. CSS 2D Transforms
23. CSS 3D Transforms
24. Flexible Box Model
25. CSS Transitions
26. Geolocation API
27. Input Types
28. Input Attributes
29. localStorage
30. sessionStorage
31. Web Workers
32. applicationCache
33. SVG
34. Inline SVG
35. SVG Clip paths
36. SMIL
37. Web SQL Database
38. IndexedDB
39. Web Sockets
40. hashchange Event
41. History Management
42. Drag and Drop
43. Cross-window Messaging
44. Touch Events

