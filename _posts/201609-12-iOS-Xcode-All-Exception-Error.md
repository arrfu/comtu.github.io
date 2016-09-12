---
layout : post
title : "打开全局断点后，每次都会在main中断一下，但是不崩溃的问题"
category : iOS
duoshuo: true
date : 2016-09-12
tags : [iOS ,Xcode,Exception]
---

#1、打开全局断点运行时，总是会在在main中断一下的问题：      

出现问题的原因：      
      删除了在工程中的字体文件，但是plist中没有删除对应字段。   

解决办法：       
  删除plist中不存在的字体文件字段。   

如图：   
![图](/res/img/blog/2016/09/iOS-2016-09-12-exception-error.png )
