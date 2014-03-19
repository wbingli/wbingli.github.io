---
author: liwenbing
comments: true
date: 2008-12-11 17:10:58+00:00
layout: post
slug: diy-your-website-social
title: 你的website也social了吗？
wordpress_id: 109
categories:
- tech
tags:
- gadget
- google friend connect
- mashup
- opensocial
- opensocialdirectory
---

如果你访问我的website，你可能注意到我的页面中多了两个小东西（我们一般叫gadget），通过它们，你可以加入我的website、看到所有的人、加他们为好友、或者可以留言。嘿嘿，看，我的website也成为social了哦。（一定要给面子加入到我的website，:),）。不过感觉比较遗憾的一点是，朋友之间不能相互地发message，是我漏点了什么吗？

这就是[Google Friend Connect](http://www.google.com/friendconnect/)所提供的能力，你只需要copy/paste它提供的HTML到你的页面中就完事了。是不是很简单呢？


Google的确在兑现它的承诺，让所有的website都能容易加入social的能力，并且还是使用最基本的东西(HTML/Javscript)。年初的时候在讲什么OpenSocial，觉得挺玄乎的，并没有搞懂这个东西到底怎么折腾到我的website，也懒得去研究它。现在Friend Connect可以说是提供了一个平民化的social方式 - Copy/Paste HTML总会了吧， 2分钟搞定。

OpenSocial就是提供了一堆的API来帮助你构建你需要的Social能力。当然这个对于需要social能力的商业网站是可行，能力很强，干啥都可以。而Friend Connect就更进一步了，就是将OpenSocial的能力平民化。通过使用OpenSocial的API写相关的OpenSocial gadget，你就可以通过Copy/Paste的方式加入各种各样的gadget。Cool？YES.  当然Friend Connect另外提供了一些基本的login，管理的能力。

看看几个简单的OpenSocial的API (当然就是JavaScript的)：



	
  * **API:** `opensocial.DataRequest.newFetchPersonRequest('OWNER')`
**Site Name:** 李文兵的主页

	
  * **API:** `opensocial.DataRequest.newFetchPeopleRequest('OWNER_FRIENDS')`
**Site Members:。。。。**

	
  * **API:** `opensocial.DataRequest.newFetchPeopleRequest('VIEWER_FRIENDS')`
**Viewer friends:。。。**


是不是很cool？你是不是也想抄起code写点东东,呵呵。 当然你也可以找些你喜欢的OpenSocial gadget来用就可以了。[http://opensocialdirectory.org/](http://opensocialdirectory.org/).

未来的mashup会怎么样呢？还有多少infrastructure的东西都被google给占了? 期待&反思。
