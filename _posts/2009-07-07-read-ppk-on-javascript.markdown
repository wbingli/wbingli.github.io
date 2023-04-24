---
author: Bruce Li
comments: true
date: 2009-07-07 23:53:46+00:00
layout: post
slug: read-ppk-on-javascript
title: 读《PPK on JavaScript》
wordpress_id: 269
categories:
- book
- javascript
- tech
tags:
- ppk javascript book accessability usability
---

 [PPK](http://www.quirksmode.org/)也是在JavaScript世界中的风云人物了，这位老兄对于浏览器端的技术以及各种浏览器的兼容性有极其丰富的经验。在这本书中，他谈了很多关于可访问性(Accessibility)和可用性(Usability)的一些问题，非常有趣。比如他说“不同的开发者以不同的方式诠释了JavaScript的目的。简单而形象地说就是：深受CSS革命影响的传统Web开发者们，创建的是瘦的、可访问性很强、乱糟糟的JavaScript代码；而来至服务器端开发的‘资深程序员们’用完美的面向对象代码、创建的是胖的、可访问性很差的Ajax客户端”。显然，ppk同学应该属于写乱糟糟但可访问性很好的人，而现在我做的大量事情却是后者。他更多相信现在的Ajax只是一种泡沫，当这个泡沫破灭并且大量‘资深服务器程序员’消失时，JS的开发者会更加注重可访问性。  可能体会不到ppk经历浏览器各种痛苦的经历，但是总体来说浏览器都在坚定地遵循Web标准，JavaScript的支持也会成为浏览器必备要求。anyway，事情总在发展，好戏在后头。分享一下我觉得这本书中几个有趣的地方。


### 可访问性和可用性

可访问性([Accessibility](http://en.wikipedia.org/wiki/Accessibility))是指你的网页对于任何人、在任何环境下都是可持续访问的。特别是指某些用户，比如弱视、浏览器不支持JavaScript或者另外一些情况，比如用户使用Mobile使用你的网页等等。而可用性(Usability)是指使用或者浏览你的网页的容易程度(这里我们只谈web页面),通常它指我们能更有效率地使用、更容易地学习以及更加满意地使用它。举个例子来说，你让你的web页面支持IE6，或者支持mobile都是在提高它的可访问性；而是用CSS来改善布局让用户更容易阅读、使用JavaScript做一些对用户有帮助的互动都是在提高它的可用性。

Web页面都是由下面三个层组成的，通过它们我们可以了解到它们之间的关系以及它们和可访问性和可用性之间的关系。
	
  * HTML结构层

	
  * CSS表现层

	
  * JavaScript行为层


![web3layers](http://liwenbing.cn/wp-content/uploads/2009/07/web3layers-300x245.PNG)

Web页面的三个层，HTML结构层是必需的基础，CSS表现层和JavaScript行为层建于它之上。所以在客户端代码中不得不关注的话题就是这三个层的关注点分离。具体探讨一下这三个的分离：

**表现与结构的分离(CSS与HTML)**

这个分离很好理解，基本思想就是确保HTML来定义结构，而所有的表现都定义在另外单独的CSS文件中。HTML不应该出现`<font>`标签和用于表现的表格。如果想定义字体和布局，都应该在CSS中处理。

大部分情况下面我们知道达到某个效果是修改表现或者结构是清楚的。但是有些情况下当更改HTML和修改CSS都可以时，你需要慎重地思考到底哪种是合理的。比如一个节点，你希望它不显示，那么你可以在HTML上面删除该节点或者使用CSS来“display:none”。当这种情况是，需要自己分析所需要的效果属于哪种情况，然后修改合理的层。

**行为与结构的分离(JavaScript与HTML)**

这个也比较好理解，就是不要把任何的JavaScript代码写到你的HTML页面中。应该把所有JavaScript代码放到一个独立的js文件中，然后将它链入到所有需要它的HTML页面中。

关于这个有一个有趣的话题就是：**无侵入脚本编程(unobtrusive scripting).**简单来说它就是通过HTML和JavaScript的分离以达到页面的可访问性和可用性的最大化。既JavaScript失效了，页面还是可阅读和理解的；而通过引入脚本和JavaScript的hook，就可以让脚本运行，增强可用性。

**行为与表现的分离(JavaScript与CSS)**

这个的分离是非常复杂的，而且并没有总结出什么特别系统的规则。CSS和JavaScript是有重合的灰色地带的，有时候完全不能确切地把某个效果归为表现还是行为。比如是使用CSS中的hover还是JavaScript的mouseover/mouseout。基本来说你自己得根据具体情况做合理的选择吧。


### 事件捕捉模型


在HTML的事件模型中有一个有趣的话题。这个简单的问题就是：如果一个节点和它的父亲节点都有对同一个事件的处理，那么到底哪个事件会被先执行呢？这就是关于事件的冒泡和捕获。事件冒泡是说事件从它的目标元素开始，沿着文档树依次向上冒泡，并触发相应的事件处理函数。而事件的捕捉是刚好相反的，它从文档的第一级开始，然后沿着文档树向下游，知道事件目标为止。

在W3C模型中，捕获和冒泡都会发生。当一个事件触发时，它先被文档捕获，到了事件目标后，再冒泡到文档顶层。而传统模型和微软模型只支持事件冒泡，而不支持事件捕获。所以最好是限制使用事件冒泡。其实在我们的实际编程中，很少关心这个话题，是因为大部分情况我们都只使用了事件冒泡。

更加具体的可以查看ppk的文章：[http://www.quirksmode.org/js/events_order.html](http://www.quirksmode.org/js/events_order.html)


### 小记


应该来说PPK所谈的JavaScript是一个更加全面的浏览器编程的世界，让人可以全面来了解这个世界包含的东西。其实大部分的内容在PPK的网站上都有，值得读读。[http://www.quirksmode.org/js/contents.html](http://www.quirksmode.org/js/contents.html)
