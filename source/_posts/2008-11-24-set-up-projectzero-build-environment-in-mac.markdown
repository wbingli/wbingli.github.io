---
author: liwenbing
comments: true
date: 2008-11-24 15:56:02+00:00
layout: post
slug: set-up-projectzero-build-environment-in-mac
title: 在Mac机上搭建Projectzero的Build环境
wordpress_id: 94
categories:
- english
- javascript
- projectzero
tags:
- mac projectzero build
---

一直想在本地建立zero的Build环境，无奈总是太耗时，也没有机器，现在有了mac在旁边，当然要拿它来做build的环境了。最近几天下班后都要整一会这个，现在终于是过了，在欢快地BUILD,TEST....

所有的应该参考：[http://www.projectzero.org/wiki/bin/view/Development/Build](http://www.projectzero.org/wiki/bin/view/Development/Build)。下面一步一步来讲吧。

1. Check out code.

这个比较简单了。对于zero现在不同的版本最好是建立对于的结构，这样以后做不同的build也利于管理。Mac已经内置了svn，所以直接敲就可以了。打开酷酷的Terminal，敲吧。。

    
    
    mkdir zero
    cd  zero
    mkdir sebring
    mkdir silverstone
    cd sebring
    mkdir source
    cd source
    svn checkout https://www.projectzero.org/svn/zero/trunk --username liwenb --password 



然后等着吧，喝点咖啡，论坛逛逛。。。。

终于checkout了zero所有的code了，嘿嘿。那么要不试试呗，

    
    
    cd trunk/BUILD/zero.build/
    ant -f zbuild.xml BUILD
    


很好，开始build了。。。嗯，nice。。。咋啦咋啦，错了？什么标签不支持（我现在是不记得了).看来是因为ant的版本的问题。查查看：

ant -version

哦，原来是1.7，而zero指明需要1.7.1，那么没有办法，安装呗。

2.安装ant 1.7.1

这个倒是简单啦，去[http://ant.apache.org/bindownload.cgi](http://ant.apache.org/bindownload.cgi).解压，那么到底放到哪里呢？于是查看了一下ant的实际地址。原来mac装在了/usr/share/ant/. 反正我也用不着它的旧的，干脆替换吧。

cp -R apache-ant-1.7.1 /usr/share/ant/

什么什么？没有权限！？查看后是这些文件是root的用户文件。之前也遇到过这个问题，想删除一个root用户的文件，可惜总是没有权限。问了mark老大，也不知道root的用户哪里去了。Google了一把，才知道原来对于Mac机，root用户是需要自己来启动的。好，去启动

3.启动Mac的root用户

当然看到apple的help就可以完成这个了，[http://support.apple.com/kb/HT1528](http://support.apple.com/kb/HT1528)。

ok，su root, 将ant替换原来的。 好，再试试，ant -f zbuild.xml BUILD。什么错？zso...?OK，zero的build中是需要build一个native的code，对于Mac需要安装Xcode。

4.安装Xcode 到Mac上

默认来说，mac是没有安装这些的。只需要找到第二张安装盘就可以了，很容易就安装上了。如果没有安装盘，那么就可以follow这个来安装了。[http://www.macworld.com/article/46286/2005/08/installxcode.html](http://www.macworld.com/article/46286/2005/08/installxcode.html).

好，再来ant -f zbuild.xml BUILD。。。很好，10分钟一直在跑，嗯？eclipse的plugin的build有问题。没办法回去看前提要求吧，原来是需要配置jdk和eclipse的。

5.配置eclipse

这个简单了。到[http://aeneis.raleigh.ibm.com/prereqs/eclipse/](http://aeneis.raleigh.ibm.com/prereqs/eclipse/)copy过来，注意download macosx的就好了，根据要求unzipped in your {prereqs directory}/eclipse/3.3.2就可以了。

6.配置JDK

根据Michael的说明，# create a link from zero.build/prereqs/jdks/java5 to  /System/Library/Frameworks/JavaVM.framework/Versions/1.5/Home

好ln java5 .....

7.完成，:)

再ant -f zbuild.xml BUILD，等上15分钟，就可以看到success的信息了。如果你要跑TEST,那可能就要等好几个小时了。

[![](http://liwenbing.cn/wp-content/uploads/2008/11/terminal-300x192.png)](http://liwenbing.cn/wp-content/uploads/2008/11/terminal.png)

P.S. 本来是上周末的文章，放到draft就到了现在，kaka..
