---
layout : post
title : "iOS App 调试证书和发布证书的创建和使用流程"
category : iOS 优秀技术文章摘录
duoshuo: true
date : 2016-04-21
tags : [iOS ,xCode,Certificates,Distribution]
---

**本文出自:伯乐在线 - 小良  ** 
**[文章链接地址:http://ios.jobbole.com/84643/](http://ios.jobbole.com/84643/)**

内容简介: 相关证书的创建，使用流程及注意事项。   

  1.Certificates: 证书，常用的证书类型有4种：真机调试证书、推送调试证书，发布证书、推送生产证书。   
  
  2.Identifiers: App ID，跟项目工程的 Bundle Identifier   
  
  3.需要支持推送、Game Center 等功能的 App ID 不能包含通配符* （下图就是在新建App ID时，选择App ID的后缀）   
  
  4.Devices: iOS设备在真机调试、AdHoc发布时都需要包含设备的UDID才可以安装。   
  
  5.Provisioning Profiles: 配置文件(描述文件)，不同类型的开发者账号都包含 Development、AdHoc 这两种   
  
  6.Profile，不同的是个人、公司开发者账号有发布到 AppStore 的 Profile，而企业开发者账号则是 InHouse 企业内发布的 Profile。   


**[文章链接地址:http://ios.jobbole.com/84643/](http://ios.jobbole.com/84643/)**
