---
author: Bruce Li
comments: true
date: 2008-04-29 07:44:51+00:00
layout: post
slug: dependency-management-is-a-big-issue-in-projectzero
title: Projectzero的版本管理是一个大问题
wordpress_id: 29
categories:
- projectzero
---

每次看projectzero论坛的帖子，经常有这样的帖子：为什么我的东西跑不起来了呀？为什么XXX找不了到？XXX有问题？接着就有人回答，你需要重新下载包，或者重新下载plug-in，或者重新使用最新的版本，或者重新resolve....

现在projectzero使用的版本管理是[Apache Ivy](http://ant.apache.org/ivy/index.html)，一种动态的dependecy管理。可惜用的过程中却不是那么的理想。尤其是用户在使用几个milestone的版本的时候，问题就出现了，这总会带给新手非常大的挫败感。我并知道到底还有什么更好的版本管理的办法，似乎对于这种在community中快速变化的软件需要这样动态的依赖管理。但是能不能更好的解决不断出现的种种问题，等待ivy的不断完善？

Zero的更新也是一个大的问题，Eclipse的zero plugin需要更新，包依赖也需要更新，对于中国用户完全是不可以忍受的速度。以后sMash如果要买的话，最好能推出一个sMash All in one的软件包。不要在resovle，这个词总让我觉得太多余了。
