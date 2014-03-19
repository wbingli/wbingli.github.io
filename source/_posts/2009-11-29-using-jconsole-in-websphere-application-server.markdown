---
author: Bruce Li
comments: true
date: 2009-11-29 09:40:19+00:00
layout: post
slug: using-jconsole-in-websphere-application-server
title: '用jconsole来管理WebSphere AppServer的MBean'
wordpress_id: 289
tags:
- webpshere application server was jmx jconsole mbean RMI
---

最近需要在WebSphere中做点魔术，所以要使用WAS(WebSphere Application Server)中的管理能力。JMX是WAS的管理的核心，苦于[WAS Mbean文档](http://publib.boulder.ibm.com/infocenter/wasinfo/v6r1/index.jsp?topic=/com.ibm.websphere.javadoc.doc/public_html/mbeandocs/index.html)并不是那么详细，这个时候就可以使用jconsole了。jconsole是一个JDK自带的JMX兼容的JVM管理工具，可以用它来可视化的查看和操作Mbean。

在启动jconsole时需要一些WAS的jar，所以我这样来做：

- 创建一个文件夹，例如：C:\programs\jconsole
- 将一些需要的jar 拷贝进入到这个文件夹的libs中：

  * com.ibm.ws.admin.client_6.1.0.jar   （在<WAS_HOME>/runtimes)	
  * ibmorbapi.jar  (在<WAS_HOME>/java/jre/lib)
  * ibmorb.jar (在<WAS_HOME>/java/jre/lib)
  * ibmcfw.jar (在<WAS_HOME>/java/jre/lib)

- 编写一个bat文件
    
    set JAVA_HOME="C:\Program Files\Java\jdk1.6.0_16"
    set WAS6.1_JARS=C:\programs\jconsole\libs
    
    set BOOTJARS=%WAS6.1_JARS%\ibmorbapi.jar
    set BOOTJARS=%BOOTJARS%;%WAS6.1_JARS%\ibmorb.jar
    set BOOTJARS=%BOOTJARS%;%WAS6.1_JARS%\ibmcfw.jar
    
    set CLASSPATH=%WAS6.1_JARS%\com.ibm.ws.admin.client_6.1.0.jar
    set CLASSPATH=%CLASSPATH%;%JAVA_HOME%\lib\tools.jar
    set CLASSPATH=%CLASSPATH%;%JAVA_HOME%\lib\jconsole.jar
    
    %JAVA_HOME%\bin\jconsole  -J-Xbootclasspath/p:%BOOTJARS% -J-Djava.class.path=%CLASSPATH%
    
- 使用如下URL来远程连接WAS中的JMX server
    
    service:jmx:iiop://localhost:2809/jndi/JMXConnector
    
其中这个端口号是RMI Connector的端口号，可以在启动的logs发现。
    
    ADMC0026I:The RMI Connector is available at port  2809
    
或者到admin console中查看
Application servers > server1 >   Administration Services > JMX   connectors

好，这样就可以了。
 
