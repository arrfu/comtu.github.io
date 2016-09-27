---
layout : post
title : "状态栏实时显示，隐藏及改变样式"
category : iOS
duoshuo: true
date : 2016-09-27
tags : [iOS ,status bar,statusbar hidden]
---

#1、隐藏状态栏方式：   
->1.
<pre class="brush: oc;  ">
-(BOOL)prefersStatusBarHidden{
    return YES;
}
</pre>


->2.   
<pre class="brush: oc;  ">
[[UIApplication sharedApplication] setStatusBarHidden:YES withAnimation:UIStatusBarAnimationSlide];   
</pre>


#2、状态栏setStatusBarHidden 设置无效解决办法：   
在plist添加字段 View controller-based status bar appearance 为NO     

如图：   
![图](/res/img/blog/2016/09/iOS-2016-09-27-pic2.png )


#3、设置状态栏StatusBar颜色样式：
->1.
<pre class="brush: oc;  ">
- (UIStatusBarStyle)preferredStatusBarStyle
{
    return UIStatusBarStyleLightContent;
}
</pre>

->2.
<pre class="brush: oc;  ">
[[UIApplication sharedApplication] setStatusBarStyle:UIStatusBarStyleDefault];
</pre>

#4、设置闪屏时状态栏StatusBar颜色样式：

如图：   
![图](/res/img/blog/2016/09/iOS-2016-09-27-pic1.png )




