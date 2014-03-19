---
author: liwenbing
comments: true
date: 2009-05-22 04:59:18+00:00
layout: post
slug: javascript-equal-type-conversion
title: JavaScript中==等同运算符的类型转换
wordpress_id: 171
categories:
- javascript
tags:
- javascript equal == === type conversion
---

这周在给一些新员工讲JavaScript的时候，谈了==和===的区别，本质来说，===是严格的等同运算符，要求两者类型相同并且值相同；而==运算符在做比较时，会做一定的类型转换。我们在使用过程中应该使用===而不是==，因为这种类型转换后的比较往往都不是你想要的。当时列出了corckfork最喜欢一些例子：

    
    
    	'' == '0'          // false
    	0 == ''            // true
    	0 == '0'           // true
    	false == 'false'   // false
    	false == '0'       // true
    	false == undefined // false
    	false == null      // false
    	null == undefined  // true
    	' \t\r\n ' == 0    // true
    




### 转换规则


当时有人就问了，那么在做类型转换的时候倒是等式的左边向右边转，还是反过来呢？其实这些都是不对的，我们去看看ECMAScript的规范，会发现它有对于等同运算符做类型转换很明确的比较算法，下面我将其翻译如下:
**对于比较x==y，**

1.如果x和y类型不同，那么到14步；
//
//2-13步，为类型相同的比较
//
14.如果x是null，y是undefined，返回true；
15.如果x是undefined，y是null，返回true；
16.如果x是Number，y是String，将y转化成Number，然后再比较；
17.如果x是String，y是Number，将x转化成Number，然后再比较；
18.如果x是Boolean，那么将x转化成Number，然后再比较；
19.如果y是Boolean，那么将y转化成Number，然后再比较；
20。如果x是String或者Number，y是Object，那么将y转化成基本类型，再进行比较；
21.如果x是Object，y是String或者Number，将x转化成基本类型，再进行比较；
22.其他情况均返回false；

ECMA这帮人写的算法过程比较啰嗦，简单一句话来概括就是，对于基本类型Boolean，Number，String，三者之间做比较时，总是向Number进行类型转换，然后再比较；如果有Object，那么将Object转化成这三者，再进行比较；对于null和undefined，只有x，y分别是它们时才相同，其他都为false。

另外，对于转化到Number的算法，细节可以来看ECMAScript的规范，但是基本上下面这个[几个表](http://www.jibbering.com/faq/faq_notes/type_convert.html#tcNumber)可以覆盖大部分的内容:


type-convert to number (+col) : String Values.





""
(empty
string)
"-1.6"
"0"
"1"
"1.6"
"8"
"16"
"16.8"






+col

0


-1.6


0


1


1.6


8


16


16.8





type-convert to number (+col) : String Values.





"123e-2"
"010"
(Octal)
"0x10"
(Hex)
"0xFF"
(Hex)
"-010"
"-0x10"
"xx"






+col

1.23


10


16


255


-10


NaN


NaN







type-convert to number (+col) : Other Values.





undefined
null
true
false
new Object()
function(){
return;
}






+col

NaN


0


1


0


NaN


NaN



再回头来看看corkford给出的例子，然后使用上面的规则去判断；

    
    
     '' == '0'          // false
    //类型相同，毫无疑问，值不同，所以结果为false
    
    0 == ''            // true
    //String要像Number转化，''是空String，根据上面的表，转成0，所以结果是true
    
    0 == '0'           // true
    //String要像Number转化，根据上面的转化Number表，'0'转成0，所以结果是true
    
    false == 'false'   // false
    //有Boolean，转化成Number，所以第一步转化后为0=='false'；
    //然后'false'向Number转，结果是NaN,最后变成比较0==NaN,那么肯定是false。
    //（NaN和任何相比都是false，就算是自己也是false， NaN==NaN //false)
    
    false == '0'       // true
    //有Boolean，转化成Number，经过第一次转化就成了0=='0';
    //就变成了上面的第3个例子，所以是true
    
    false == undefined // false
    //对于undefined和null，只有两边分别是两者才是true，其他都是false；所以是false
    
    false == null      // false
    //对于undefined和null，只有两边分别是两者才是true，其他都是false；所以是false
    
    null == undefined  // true
    //对于undefined和null，只有两边分别是两者才是true，其他都是false；所以是true
    	
    ' \t\r\n ' == 0    // true
    //对于String，先转成Number，对于空String，都将转成0，所以转化后成为0==0,结果为true
    //（注意，空字符不仅仅是只是空格，还包括\t\r\n等等，更多可以见ECMAScript spec的9.3.1）
    
    




## 总结


虽然我们了解了==这个坏东西的本质，但是在我们的实际JavaScript编程中是要避免使用==，而是去使用===这个严格的比较运算符。
