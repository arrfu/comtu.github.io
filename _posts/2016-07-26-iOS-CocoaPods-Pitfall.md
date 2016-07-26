---
layout : post
title : "CocoaPods 遇到的坑"
category : iOS
duoshuo: true
date : 2016-07-25
tags : [iOS ,cocoapods,pitfall]
---

一、找不到相关库   

出现错误提示：   
<pre class="brush: oc;  ">
ld: library not found for -lPods-CocoaLumberjack`   
</pre>

解决办法：   
设置 `Project` -> `Pods` 下所有第三方库的 `Build Active Architecture Only` 为 `NO`   

[参考地址：http://my.oschina.net/ioslighter/blog/382422](http://my.oschina.net/ioslighter/blog/382422)   

二、pod install 失败   
出现错误提示：   

<pre class="brush: oc;  ">
[!] The `app [Debug]` target overrides the `OTHER_LDFLAGS` build setting defined in `Pods/Target Support Files/Pods/Pods.debug.xcconfig'. This can lead to problems with the CocoaPods installation
    - Use the `$(inherited)` flag, or
    - Remove the build settings from the target.

[!] The `app [Release]` target overrides the `OTHER_LDFLAGS` build setting defined in `Pods/Target Support Files/Pods/Pods.release.xcconfig'. This can lead to problems with the CocoaPods installation
    - Use the `$(inherited)` flag, or
    - Remove the build settings from the target.
</pre>

解决办法:   
在Build Settings -> Other linker flags 位置，添加 $(inherited) 参数   

三、pod install 提示缺少目标依赖target   
解决办法：   

修改前：   

<pre class="brush: oc;  ">
platform :ios, '7.0'
pod 'CocoaLumberjack','~> 2.3.0'
</pre>

修改后：   
在Podfile中添加
<pre class="brush: oc;  ">
target 'TelinkBlueDemo' do
platform :ios, '7.0'
pod 'CocoaLumberjack','~> 2.3.0'
end
</pre>

其中，TelinkBlueDemo为所在项目中，TARGETS配置里的工程名字   
[参考地址：http://www.tuicool.com/articles/e6VrieN](http://www.tuicool.com/articles/e6VrieN)   
