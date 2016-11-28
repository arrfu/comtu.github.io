---
layout : post
title : "iOS 处理崩溃的第三方框架 NullSave 和 AvoidCrash 比较"
category : iOS
duoshuo: true
date : 2016-11-28
tags : [iOS , NullSave, AvoidCrash]
---

******

**AvoidCrash 功能介绍及使用场景：**

 * 这个框架利用runtime技术对一些常用并且容易导致崩溃的方法进行处理，可以有效的防止崩溃。

 * 并且打印出具体是哪一行代码会导致崩溃，让你快速定位导致崩溃的代码。

 * 你可以获取到原本导致崩溃的主要信息<由于这个框架的存在，并不会崩溃>，进行相应的处理。比如：
  你可以将这些崩溃信息发送到自己服务器。

 * 你若集成了第三方崩溃日志收集的SDK,比如你用了腾讯的Bugly,你可以上报自定义异常。

[github 地址：https://github.com/chenfanfang/AvoidCrash](https://github.com/chenfanfang/AvoidCrash)

**NullSave 功能介绍及使用场景：**

  * 这个框架通过将Null控制替换为nil，而nil是安全的，可以向nil对象发送任何message而不会奔溃。可以有效的防止崩溃。
  
  * NullSafe 是一个 Category，使用起来非常方便，只要加入到了工程中就可以了。
  
  * NullSafe在ARC和非ARC中都能使用。

[github 地址：https://github.com/nicklockwood/NullSafe](https://github.com/nicklockwood/NullSafe)
