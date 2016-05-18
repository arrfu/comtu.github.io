---
layout : post
title : "iOS UIAlertView弹出视图动画效果实现"
category : iOS 优秀技术文章摘录
duoshuo: true
date : 2016-05-18
tags : [iOS ,UIAlertView,Animation]
---


**本文出自: [文章链接地址:http://www.tuicool.com/articles/E3iiQb)]**   


  在App设计中为了加强用户体验，我们会常常加入一些友好的动画效果。比如类似UIAlertView弹出的动画效果，由于系统中并没有直接提供类似的动画API，
如果我们想要做出一样的效果，那就得深入的研究一下系统中的UIAlertView了。

  仔细观察UIAlertView的动画你就会发现：这个动画是由几部分组成，它带一个视图大小抖动的效果。先是由小变大，再由大变小，最后变成本来的大小。
但是这个大小的具体参数值和动画的速度恐怕是肉眼所不能看出来的。 本篇文章会使用一些objc runtime和CAAnimation的一些知识，通过本文你可以了解
到如何研究一些objc中内部调用机制和动画基础。

要想知道这些动画的组成，我们就要从比较低层次的API：CALayer的一些调用开始。iOS动画最终都是加到Layer中的，加入Layer就要调用Layer对象这个方法：

<pre>
- (void)addAnimation:(CAAnimation *)anim forKey:(NSString *)key;
</pre>

  所以只要我们知道了anim参数，并把anim动画对象的属性揪出来，就可以知道到底是什么动画了，但是这个方法是系统Framework中的，通常我们是无法能知道
anim到底是什么。这时我们就需要用一些objc的一些底层API：Objc Runtime来解决了。   


**本文出自: [文章链接地址:http://www.tuicool.com/articles/E3iiQb)]** 
