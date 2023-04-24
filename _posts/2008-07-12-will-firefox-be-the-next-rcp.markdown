---
author: Bruce Li
comments: true
date: 2008-07-12 14:05:19+00:00
layout: post
slug: will-firefox-be-the-next-rcp
title: Firefox会成为新的RCP吗？
wordpress_id: 64
categories:
- tech
---

对于firefox的插件大家都觉得属于添加浏览器的功能，这些插件只是firefox中的小甜点，并不能自成一个完整的软件。 我也是一直这么认为，但是今天看到一个用firefox做的画图的工具，[http://www.evolus.vn/Pencil/Home.html](http://www.evolus.vn/Pencil/Home.html)，一个和原有插件很不一样的插件。它只是将 **Mozilla Gecko engine**做成了一个画图的工具。并且最特别的地方是大小只有400K，非常的惊人。

觉得firefox非常有希望成为一个RCP的平台，相对Eclipse这样的RCP平台(或者Abode的AIR)来说是有非常多的优势的：



	
  * 安装非常的方便:显然firefox的插件安装方式非常方便，点个link就安装了。

	
  * 更新同样简单： 同样由firefox自己对插件更新的支持，自动检查更新最新版本。

	
  * 软件大小非常小：看一个完整的画图工具只有400K，就知道能它能多小，安装可以多快。使用firefox所提供的对UI等的支持，将js的文件压缩后真的可以非常的小。

	
  * 多平台支持：firefox就支持多平台，你写出来的软件也就自然能跑在所有firefox能支持的平台了。

	
  * 开发要求低：基本上只需要掌握javascript，而世界上最多程序员的将会是javascript程序员。firefox的UI开发比起HTML，CSS来说是简单的。每个人估计是受到过写HTML，尤其是CSS中要处理多平台的煎熬。从这个角度来说，写firefox的插件比开发一个Rich的Web应用要快很多。

	
  * 不需要安装体积庞大的SDK：你只需要firefox就可以了。很多人抱怨abode的SDK或者java的JDK都太大了，而很多非技术人不明白为什么需要安装这些乱七八糟并且几十M的东西，这也是abode的AIR很难快速流行的问题。


现在也是有很多不太如意的地方：

	
  * 每次启动你得从firefox中启动，不能直接在OS的建立快捷方式。我觉得这个问题应该不大，看看[Prism](http://labs.mozilla.com/projects/prism/)这样的firefox的lab project就可以了。

	
  * 如何来操作数据库？操作数据库是一个基本的，可惜firefox并没有提供这样的支持。wait，firefox 3提供了类似google gears的能力，估计将数据持久化到本地是问题不大。但是连接到传统的数据库mysql等的呢？


当然firefox从来没有提过它要成为一个RCP的platform，它是否想成为abode AIR的competitor？是否它可以成为新的RCP平台，甚至它是否可以成为[RIA](http://en.wikipedia.org/wiki/Rich_Internet_application)平台，让我们试目以待吧。
