---
author: liwenbing
comments: true
date: 2009-06-05 08:36:38+00:00
layout: post
slug: why-ff3-cannot-load-local-javascript-file
title: 为什么Firefox 3不能加载本地的JavaScript文件了？
wordpress_id: 191
categories:
- javascript
tags:
- firefox javascript local file policy cross domain
---

一段时间来一直受这样的困扰，就是我的Firefox无法运行本地的dojo的测试文件。一直以为是我的firefox或者机器出了什么问题，就只好去使用IE或者Chrome去运行这些测试例子，可惜不能用firebug的确让人很不爽。

今天在firebug查看了一些错误情况，报错居然是“Access to restricted URI denied”。这个明显是跨域访问的错误，但是本地文件怎么报这样的错呢？在Firefox的about:config搜索了一下policy,居然找到了原因所在，原来Firefox对于本地文件也进行了同源访问的安全设置,配置参数是：security.fileuri.strict_origin_policy。这个新的设置只是在firefox 3才被加入，并且默认是开启的。不过你也可以将这个关掉，这样就可以如同以前那样运行本地的dojo测试用例，或者其它你想本地加载的JavaScript文件。

[![local-file-p-origin-policy](http://liwenbing.cn/wp-content/uploads/2009/06/local-file-p-origin-policy-300x42.png)](http://liwenbing.cn/2009/06/05/why-ff3-cannot-load-local-javascript-file/local-file-p-origin-policy/)

继续在google了一下，找了这个"feature"的由来，[https://bugzilla.mozilla.org/show_bug.cgi?id=230606](https://bugzilla.mozilla.org/show_bug.cgi?id=230606)，大概是说本地的文件如果没有这样的限制，可以访问本机的其他文件，这样会造成安全隐患。John Resig（Father of  jQuery) 也有一个blog关于这个问题，[http://ejohn.org/blog/tightened-local-file-security/](http://ejohn.org/blog/tightened-local-file-security/)，下面的评论也挺值得看看的。

**More Links: **



	
  * [http://www.dojotoolkit.org/support/faq/why-does-dojo-fail-load-file-urls-firefox-3](http://www.dojotoolkit.org/support/faq/why-does-dojo-fail-load-file-urls-firefox-3)

	
  * [http://kb.mozillazine.org/Security.fileuri.origin_policy](http://kb.mozillazine.org/Security.fileuri.origin_policy)

	
  * [http://kb.mozillazine.org/Security.fileuri.strict_origin_policy](http://kb.mozillazine.org/Security.fileuri.strict_origin_policy)


