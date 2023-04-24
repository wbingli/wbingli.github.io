---
author: Bruce Li
comments: true
date: 2008-06-13 16:04:22+00:00
layout: post
slug: aptana-jaxer-the-ajax-server
title: Aptana Jaxer:The Ajax Server?
wordpress_id: 55
categories:
- blog
- book
- fun
- tech
tags:
- apatan
- jaxar
---

一直以来都在用Aptana的Editor来编辑Javascript/CSS/HTML,都挺好。今天尝试了Aptana自己一直在推的所谓‘世界上第一个Ajax Server的[Jaxer](http://aptana.com/jaxer)。

在Jaxar里面写code倒是很有意思，所有你需要做的事情就是写Javascript/CSS/HTML。你根本不需要使用任何其他server-side语言,所有的事情就是写Javascript就可以了。来看一个例子：

    
    <code>
     <script type="text/javascript" runat="server">
    	function getAuthenticatedUser()
    	{
    		var username = Jaxer.session.get("username");
    		if (typeof username == "undefined") return null;
    		var rs = Jaxer.DB.execute("SELECT * FROM users WHERE username = ?", [username]);
    		if (rs.rows.length == 0)
    		{
    			return null;
    		}
    		return rs.rows[0];
    	}
    </script>
    </code>


用‘runat=server’就可以让上面对数据库的操作运行在server端，而client端对该方法的调用不变，这样在写Web应用时就不用在Server side和client side两边跑来跑去了。并且还有对template的支持。

这个和原来老毛和科长做的project zero client programming model是非常相似的，目的是都用来屏蔽client和server之间的boundary。不过Jaxar做的更加彻底，通过扩展Apache的server，加入自己的Server side framework和client side framework，让所有的一切都通过写JS就搞定了。并且对session，database, web ,SMTP 进行支持，对于一般的应用差不多就够了。老毛原来做的也是通过加入client framework以及扩展server的一些能力来让开发者在client和server之间进行无缝交互。可惜还是需要写Javascript和groovy，并且有一大堆的convention，不知道为什么没有发展下去（又是政治问题?).

那么这种开发模式到底好不好呢？我觉得对于比较小的应用，不考虑扩展和与外界交互，还是一个比较快捷的开发方式。毕竟client和server的无缝交互所带来的好处是非常大的，比如说学习的门槛低（只需要知道一个Javascript就搞定了), 数据传输中麻烦的异步调用，编码，解码，格式转换等等都将消失。但是一旦你的web应用大一些的时候，我想这种模式就面临着很大的问题。关键还是不容易扩展，当它把UI和数据逻辑混合的时候，要做分离是比较困难的。当然你可以在它的编程模型上写一层数据操作层，但是这样就变成了典型的RPC了。另外，这样做并不[REST](http://en.wikipedia.org/wiki/Representational_State_Transfer),Jaxer开发出来的应用根本提供不了service(更谈不上RESTful)，这样就无法被它人所用了。如果Jaxer应用以后要做整合，那绝对是一个大麻烦。
