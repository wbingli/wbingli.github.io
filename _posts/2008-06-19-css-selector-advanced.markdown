---
author: Bruce Li
comments: true
date: 2008-06-19 23:57:17+00:00
layout: post
slug: css-selector-advanced
title: CSS Selector - 高级
wordpress_id: 46
categories:
- css
---

在前面一篇文章中([CSS Selector - 基础](http://liwenbing.cn/2008/06/03/css-selector-basic/))介绍一些CSS selector的基本用法。里面来谈一些平时比较少用的CSS selector用法。

**一.Child selector**
这个选择器和前面的descendent比较类似，descendent适用与所有的下代，但是Child描述的是父子关系。它需要用尖括号'>'来表示。  例子如下：

    
    
    body>h2{
     font-size:2.5em;
    }


在未来的发展中，有些有趣的讨论就是：[是否我们需要右括号](http://www.shauninman.com/archive/2008/05/05/css_qualified_selectors),就像：
a < img { border: none; }就表明如果  父亲是, 那么border就不存在。当然这只是一个提议，更加站在目标tag来进行表述。可惜并不在CSS3的规范中。

**二.Adjacent siblings selecor**
前面说了父子关系的选择器，这个是关于兄弟关系的选择器。

    
    
    h2+p{
     font-size:1.2em;
    }
    


如果一个<a>前面是<h2>那么将应用该selector。

**三.属性 selector**
属性选择器是一个更加强大的选择器。前面的selector都是更加tag的位置关系等来进行选择。属性选择器是根据标签内部的属性来进行选择。

    
    
    //选择<a>中href属性为'liwenbing.cn'的
    a[href='liwenbing.cn'] {
     font-size:red;
    }
    //选中<input>中类型type为text的。
    input[type="text"]{
     border-color:1px solid #666;
    }
    


在CSS3中，[对属性选择器是有加强的](http://www.w3.org/TR/2005/WD-css3-selectors-20051215/#attribute-substrings)。并且现在流行的浏览器(IE7,FF)基本上已经支持这样的selector了。
有三种：



	
  * [att^=val]：选择属性att的值是以val来开头的；

	
  * [att&=val]：选择属性att的值是以val来结束的；

	
  * [att*=val]：选择属性att的值是包含val的；



    
    //这个将选中 <p title='home' ></p>
    p[title^="ho"] {background: green;}
    
    //这个将选中 <p title='hot' ></p>
    p[title$="t"] {background: green;}
    
    //这个将选中 <p title='contact' ></p>
    p[title*="on"] {background: green;}
    
