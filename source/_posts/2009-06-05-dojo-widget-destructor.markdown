---
author: liwenbing
comments: true
date: 2009-06-05 11:09:13+00:00
layout: post
slug: dojo-widget-destructor
title: Dojo widget的析构过程
wordpress_id: 204
categories:
- javascript
tags:
- javascript dojo dijit widget destructor
---

了解dojo widget（或者说dijit）的析构过程，不仅让你更加了解整个dijit的生命周期，同样也能帮助我们在自己定制化的dijit中如何正确地释放资源。（这里讨论的dojo应该是在0.9或者以上版本的)

下面是dijit的析构过程：

    
                            destroyRecursive
                        /                      \
                    destroy                   destroyDescendants
            /        |        \
    uninitialize  disconnect() destroyRendering


一些常见的**错误**是如下：



	
  * 使用destroy()去销毁一个dijit。我们应该使用destroyRecursive()去销毁一个dijit，从上面的过程可以看出，destroyRecursive()会销毁其孩子widgets。

	
  * 使用destory()去销毁定制dijit中的资源。更可怕的是有的代码可能是直接覆盖destroy，而根本不调用_Widget中的destory。uninitialize()才是dijit暴露出来给定制化widget进行析构的stub function。




### 结论


**使用destroyRecursive()去销毁dijit，使用uninitialize()在定制化的dijit来释放自己的资源**。destroyDescendants，destroyRendering基本上用不到，也不要去覆盖它们。
