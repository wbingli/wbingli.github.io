---
author: liwenbing
comments: true
date: 2008-06-03 06:22:25+00:00
layout: post
slug: css-selector-basic
title: CSS Selector - 基础
wordpress_id: 41
categories:
- css
---

想总结一下关于CSS的selector部分以及对selector的一些思考。我准备分成四个部分来写，算是系列文章吧。四个部分分别是：



	
  1. 基础：主要是从CSS selector来谈起，也是平时用的最多的；

	
  2. 高级：谈谈一些高级的selector的用法；

	
  3. 关于Cascade：关于CSS继承，优先级的一些topic；

	
  4. 未来的思考：未来的CSS selector会怎么发展？CSS 3做了什么？javascript所带来的影响是什么？




## 1.基础


**一.Tag selector**
标签selecor ,也可以称为类型selector，它将对HTML页面中所有的该类型有效。

    
    
    h1 {
       font-family:Arial, sans-serif;
       color:#CCCCFF;
    }


**二.Class Selector**
类选择器和HTML中的class相结合，应用到所有声明了该class的标签中。类选择器必须要以 "."来开头。
对于

    
    
    .title{
       color:#FF000;
       font-size:24px;
    }


**三.ID selector**
ID选择器会将其syle应用在HTML中该ID的tag上。
对于

    
    
     #sidebar{
      float:left;
      margin-left:30px;
    }


一个有意思的问题是如果在一个HTML中，有两个以上的tag用了同一个ID会怎么样呢？虽然并不是合法的HTML，但是大部分的游览器会忽略这个问题。这个时候ID选择器就像类选择器一样，只要哪个tag的使用了该ID，就会应用该style.
** 四.Descendent selector**

这种选择器的的意思就是根据DOM树的祖先、父子关系来选择具体的元素。

如:

    
    
    h1 strong {
         color:red;
    }


将对<h1> This is homepage of <strong>liwenbing</strong></h1>中的strong的内容进行标红。

同样这种方式是可以组合前面的任何三种选择器：

    
    
    h1 .intro a{
          color:yellow;
    }


将对<h1> This is homepage of <strong >liwenbing   </strong> who is <span class="intro"><a href="#">lovely boy</a></span></h1>中的lovely boy标黄。

注意tag和类之间是否有空格是非常重要的。

有空格表示是应用该类的元素在tag之中，就像上面的例子；没有空格，则表示tag和class都标明的是同一个元素。

    
    
    h1.intro a{   //注意h1和.intro之间没有空格
         color:blue;
    }


对<h1 class="intro">This is homepage of <a href="#" >liwenbing </a></h1>中的liwenbing是有效的。

再举一个和ID selector结合的例子：

    
    
    #sidebar h2{
          font-size:1.2em;
    }


上述style将对sidebar元素下面的所有h2进行应用。

**五.Group selector**

组选择器顾名思义就是对于一组selector应用同一中style，以逗号","隔开；组中的元素都可以是前面四中的任何一种。

    
    
    h1,
    p .intro,
    #sidebar .title{
     color:#6F6F6F;
    }


**六.伪选择**
伪选择器可以进一步地定义用户和页面的行为。最常用的就是对link的定义了；

    
    
    a:hover{
      text-decoration:  none;
    }
    a:visited{
     color:blue;
    }


当然关于link的除了上述两种，还有a:link,a:active;
其他还有很多的伪选择，例如现在在用的focus.下面这个CSS可以让input 有safari中的高亮效果（不过IE并不支持)。

    
    
     input:focus{
      border-color: #feca70;
    }


其他伪选择器还有:before,:after,:firs-child.
