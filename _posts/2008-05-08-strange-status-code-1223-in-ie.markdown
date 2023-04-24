---
author: Bruce Li
comments: true
date: 2008-05-08 15:08:04+00:00
layout: post
slug: strange-status-code-1223-in-ie
title: IE中奇怪的status code 1223
wordpress_id: 36
categories:
- javascript
---

又是IE，为什么老是你？今天处理一个IE和firefox不兼容的问题，最后的原因是因为IE的一个奇怪的HTTP status code 1223.原来，IE会将[HTTP的204(No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.2.5)转换成它内部的status code 1223，就会产生这个问题。本来期望dojo能屏蔽这个问题，但是看来它并没有按照期望的方式进行处理。status code还是1223返回，并且作为error来抛给Error callback给处理。

解决的办法可以是在error的时候去判断处理：（感觉不是很好）

    
    
    error:function(err, ioArgs){
        if(dojo.isIE&&ioArgs.xhr.status;==1223){
           //do something
        }else{
            //handle error
        }
    }
    


Some links:
[http://vegdave.wordpress.com/2007/11/05/1223-status-code-in-ie/](http://vegdave.wordpress.com/2007/11/05/1223-status-code-in-ie/)
[http://trac.dojotoolkit.org/ticket/2418](http://trac.dojotoolkit.org/ticket/2418)
[https://groups.google.com/group/jquery-en/browse_thread/thread/8136195c67c9819b](https://groups.google.com/group/jquery-en/browse_thread/thread/8136195c67c9819b)
