---
author: liwenbing
comments: true
date: 2008-06-25 15:13:24+00:00
layout: post
slug: ajax-upload-file-using-dojo-in-smash
title: 在sMash环境中，使用dojo来Ajax上传文件
wordpress_id: 58
categories:
- projectzero
tags:
- sMash dojo upload ajax
---

在dojo的test page中给出了如何上传文件，[http://archive.dojotoolkit.org/nightly/dojotoolkit/dojo/tests/io/iframeUploadTest.html](http://archive.dojotoolkit.org/nightly/dojotoolkit/dojo/tests/io/iframeUploadTest.html).可惜server端的code是python写的，现在把dojo的upload文件在WebSphere sMash的环境下实现，并且强调几个关键的trick,在code中进行说明。故事很简单：上传一个文件，完毕后返回文件的大小，最后浏览器弹出该信息。



	
  1. **HTML code**

    
    <form action="/resources/upload" id="uploadForm"
    <!-- 注意上传文件时需要的form属性enctype="multipart/form-data"  -->
    	method="POST"  enctype="multipart/form-data">
    <!-- 上传文件在HTML的控件 -->
    	<input type="file" name="attachment">
    	<input type="button" onclick="uploadIt(); return false;" value="send it!">
    </form>




	
  2. **Javascript code**

    
    <script type="text/javascript">
    	dojo.require("dojo.io.iframe");
    	function uploadIt(){
    		dojo.io.iframe.send({     //使用iframe进行提交
    			form: dojo.byId("uploadForm"),
    			handleAs: "application/json",
    			handle: function(response, ioArgs){
    				if(response instanceof Error){
    					console.error("Request FAILED: ", response);
    				}else{
    					alert(response);  //alert结果
    				}
    			}
    		});
    	}
    </script>




	
  3. **zero code**

    
    def onCreate() {
    	//zero中的上传文件放在request的files中
    	def attachment = request.files["attachment"]
    
    	//每个上传文件有上传文件的临时路径以及文件
    	def filepathUpload = attachment.path[0]
    	def fileName = attachment.filename[0]
    
    	int fileSize = new FileInputStream(new File(filepathUpload)).available()
    	def msg = "The size of your file '" + fileName + "' is " + fileSize + " bytes."
    
    	//这个是dojo iframe做定义的，必须要使用一个<textarea>进行包裹结果.
    	def iframeData = "<textarea>" + msg + "</textarea>"
    	println iframeData
    }





这个是sMash的project，可以[Download(Ajax upload file using dojo in sMash)](http://liwenbing.cn/wp-content/uploads/2008/06/zeroupload.zip)下来试试。

**_Useful links:_**

[http://www.cs.tut.fi/~jkorpela/forms/file.html](http://www.cs.tut.fi/~jkorpela/forms/file.html)
[https://www.projectzero.org/javadoc/latest/CORE/API/zero/core/context/GlobalContextURIs.Request.html#files](https://www.projectzero.org/javadoc/latest/CORE/API/zero/core/context/GlobalContextURIs.Request.html#files)
