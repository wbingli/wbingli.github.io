---
layout: post
title: "“我从来不用if-else…”"
date: 2010-05-01 08:00:00 -0700
comments: true
categories:
---

前几天，同事面试完回来哈哈大笑说，面试的人折腾了半天一个简单的程序没有搞定，还很牛逼哄哄地说我写程序从来不用if-else…连Mark同学听到也开心地笑了，呵呵

这几天路上无聊琢磨到底不用if-else怎么写程序，倒是想了几个办法。(使用JavaScript)

方法一：用while代替.

```
function noifelsewhile(condition){
    while(condition){
	alert("I'm Jack");
	break;
    }
    while(!condition){
	alert("I'm Rose");
	break;
   }
}
noifelsewhile(true);
noifelsewhile(false);
```

方法二：用for代替.
和while一个套路

```
function noifelsefor(condition){
   for(;condition;){
	alert("I'm Jack")
	break;
   }

   for(;!condition;){
	alert("I'm Rose");
	break;
   }
}
noifelsefor(true);
noifelsefor(false);
```

办法三:三元表达式
因为三元表达式只能使用表达式，所以需要使用一个function用来支持多行statements

```
function noifelseternary(condition){
    condition?function(){
		    alert("I'm Jack");
		}():
		function(){
		    alert("I'm Rose");
		}();
}
noifelseternary(true);
noifelseternary(false);
```

办法四:逻辑与或-Default
在JavaScript中&&是logical and, 也可以称谓guard。如果第一个参数是false，那么返回第一个值，否则返回第二个值。而并不一定返回true或false；

var value = p && p.name; /* The name value will only be retrieved from p if p has a value, avoiding an error. */
||是logical or，也可以成为default。如果第一个参数是false，那么返回第二个值，反则返回第一个只。同样并不是一定返回true或者false。

value = v || 10; /* Use the value of v, but if v doesn't have a value, use 10 instead. */
更多这个信息可以查看A Survey of the JavaScript Programming Language。
好，现在就运用这个两个操作来模拟if-else

```
function noifelsedefault(condition){
 (condition ||
	function(){
		alert("I'm Rose");
	}())&&
	function(){
		alert("I'm Jack");
	}();
}
noifelsedefault(true);
noifelsedefault(false);
```

办法五:逻辑与或-Guard
这一次把&&放到前面。这种逻辑与或在其他语言也有，比如python中的and，or

```
function noifelseguard(condition){
  (condition &&
	function(){
		alert("I'm Jack");
		return true; //注意一定要有return true，要保证这个函数返回true。
                //其实办法4中需要保证第一个函数返回false，因为没有返回值就是null，所以就可以不用显式加return false了。
	}())||
	function(){
		alert("I'm Rose");
	}();
}
noifelseguard(true);
noifelseguard(false);
```

还有其他的办法吗？大家来变态~，:)
五一快乐~
