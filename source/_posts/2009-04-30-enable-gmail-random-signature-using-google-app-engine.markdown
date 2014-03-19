---
author: Bruce Li
comments: true
date: 2009-04-30 21:21:52+00:00
layout: post
slug: enable-gmail-random-signature-using-google-app-engine
title: Enable Gmail Random Signature using Google App Engine
wordpress_id: 128
categories:
- fun
---

在GMail的Lab中有这样的一个小东西，它可以随机地加入签名档到你的邮件中。这个一直是我想要的，于是在五一无聊的时候试试。

首先当然是要到GMail的Lab中enable这个功能了。

[![enable1](http://liwenbing.cn/wp-content/uploads/2009/05/enable1.png)](http://liwenbing.cn/2009/05/01/enable-gmail-random-signature-using-google-app-engine/enable1/)

这个时候到Setting->General中的Signature中会多出这么一个东西，

[![setting](http://liwenbing.cn/wp-content/uploads/2009/05/setting.png)](http://liwenbing.cn/wp-content/uploads/2009/05/setting.png)

默认是http://www.brainyquote.com/link/quotefu.rss这个地址，这个时候当你新建一个邮件，就会发现有签名档随机插入到邮件中。但是这个并不是我想要的签名档，我需要它是从我自己的签名档列表中去找。这个功能的签名档的信息是由一个RSS来给出的，那么何不使用Google App Engine来做一个自己的签名档管理的小系统，这样我可以加入自己新的签名档，然后还可以提供签名档的RSS给它呢？

说动手就动手吧，很快就完成了。[http://liwb-quote.appspot.com/](http://liwb-quote.appspot.com/)。界面是抄twitter的，因为我的功能的确就是和它一样。我不得不说，在App Engine上面开发这样的应用的效率是惊人的。我很久没有动python了，App Engine的东西也是边看它的tutorial来完成的。但是我还是没有碰到太多的障碍就完成这些事情。看截图吧。

[![quote](http://liwenbing.cn/wp-content/uploads/2009/05/quote.png)](http://liwb-quote.appspot.com/)

然后再是提供RSS就可以了：http://liwb-quote.appspot.com/rss。 再将这个地址放到gmail中设置就可以了。嘿嘿，当你再新建一个邮件的时候，签名档就是你在appspot上面的自己的了。是不是很high呢？

**Conclusion**

App Egine来开发这样的应用的效率是极高的。无非是数据库的一些操作，开发、调试和上线的体验只有用过才知道是如何的high。

另外，Gmail中的[Random Signature](http://groups.google.com/group/gmail-labs-help-random-signature)还是非常不成熟，它并不是实时地去拿我当前的记录。也不知道它多久抓一下，这点让人沮丧。

**Download**

这个是我的App Engine的工程，如果你管理自己的签名档或者类似一句话的东东，这个都可以用。

[**下载App Engine应用**](http://liwenbing.cn/download/liwb-quote.zip)

一些注意事项：



	
  1. 下载后，修改app.yaml 文件中的application: {{ 你的应用名字  }}。

	
  2. 在main.py中需要把users.get_current_user().email()=='wbinglee@gmail.com'改成你自己的email，因为我让只有本人才有添加的能力，其他人只有浏览的权限。

	
  3. 然后appcfg.gy update 就好了。




