---
author: liwenbing
comments: true
date: 2008-05-07 13:02:02+00:00
layout: post
slug: ie-ff-comapatibility-in-dojo
title: 在dojo中处理IE和Firefox的常见的兼容性问题
wordpress_id: 33
categories:
- javascript
---

让javascript在不同的浏览器之间兼容就是一件pains-taking的事情，虽然dojo已经封装了不同浏览器之间的 大部分差异，在实际使用dojo进行开发的时候，仍然偶尔会遇到此类问题。
近一段时间又在做这个工作，也将这些tips记录如下。这些内容也是在组里人在处理这些问题不断总结的出来。



	
  1. 
Object的定义不严格（可以说最容易出错的地方）：
**实例：**

    
    var obj = {a:"ha",b:"he",}; //Error in IE


**原因:**
这个会在IE中出错，但是在firefox中work。原因就是在IE中Object后面不允许多一个逗号，其实ECMAScript上面的确定义这个是不合法的，只能说IE在这种地方还真是遵循标准。在新的JavaScript 5中，这个将会是合法。
**解决办法：**
去掉逗号，如上例{a:"ha",b:"he"}。在Eclipse中可以用如下正则表达式来需找这种不合法的使用：**,\s*}**

	
  2. 
javascript 的Array的定义不一样
** 实例：**

    
    [1,2,3,].length === 3 //  FF
    [1,2,3,].length === 4 // IE


**错误原因及修正：**
这个麻烦的是IE并不会报语法错误，所以更要小心。注意在array的定义中请去掉逗号。在JavaScript 5中，IE将更正这个bug。

	
  3. 
 非显式局部变量声明
**实例：**

    
    self = this;
    self.a = {};


**原因以及修改办法：**
IE中不允许隐式声明局部变量，任何局部变量必须通过类似var a = xx的方式进行显式声明

	
  4. 
直接设dom节点的class属性
**实例：**

    
    dom.setAttribute("class","xx")


**错误原因:**
并不是所有的属性都可以使用setAttribute来设置的。在IE中设置class实际上要使用className。这个link中给出了所有的不兼容的属性，要避免使用setAttribute设置这些属性。http://webbugtrack.blogspot.com/2007/08/bug-242-setattribute-doesnt-always-work.html
**解决办法:**
使用dojo，当然尽量使用dojo的方法出处理这些问题。或者按照上面的link给出的办法去处理。

	
  5. 
IE ,Firefox 的CSS 的hover机制不同
**描述： **
在Firefox中，css的hover可以针对所有的HTML节点 ,like DIV,SPAN...,但是在IE中，hover的对象是有限制的，现在是可以对hyperlike可以进行hover.

**错误原因及修正：**
尽量使用<a></a>来wrap所需要hover的元素。
或者使用js中onMourseOver触发事件更改style

	
  6. 
IE 的table在创建时TR必须要append到tbody上
**描述：**
在Firefox中，TR可以直接append到table上面，但是在IE中，直接的append会导致TR显示不出来。
**错误原因及修正 ：**
这个是一个IE DOM API一个不兼容的问题，具体细节可以看下面两个链接。解决办法当然是每次在东西创建table时，也需要创建tbody，然后见TR append到tbody中。

具体可以看：http://www.quirksmode.org/dom/w3c_html.html#t03
原因可以看：http://www.ericvasilik.com/2006/07/code-karma.html

	
  7. 
IE 6.0 以下版本Select Element问题
**问题描述： **
在IE 60 以下版本中的Select Element无论其z-index 设为何值，其均在任何Div之上，对于有拖动div的页面，应该考虑进行改进。
详细的Test Case 请参见： [1] 中的4. Bug: SELECT elements ignore Z-index ("windowed element" problem)
**解决方法 ： **
1.Dojo中使用 dojoType 为dijit.form.ComboBox的 Select Element会将其parse成为input Box，从而解决此问题。
2.Google的GWT中将每个Div下添加一个iframe 元素，使其能够掩盖Select Element。


