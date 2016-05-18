---
layout : post
title : "iOS UIAlertView弹出视图动画效果实现"
category : iOS 优秀技术文章摘录
duoshuo: true
date : 2016-05-18
tags : [iOS ,UIAlertView,Animation]
---


**本文出自: [文章链接地址:http://www.tuicool.com/articles/E3iiQb]**   


  在App设计中为了加强用户体验，我们会常常加入一些友好的动画效果。比如类似UIAlertView弹出的动画效果，由于系统中并没有直接提供类似的动画API，
如果我们想要做出一样的效果，那就得深入的研究一下系统中的UIAlertView了。

  仔细观察UIAlertView的动画你就会发现：这个动画是由几部分组成，它带一个视图大小抖动的效果。先是由小变大，再由大变小，最后变成本来的大小。
但是这个大小的具体参数值和动画的速度恐怕是肉眼所不能看出来的。 本篇文章会使用一些objc runtime和CAAnimation的一些知识，通过本文你可以了解
到如何研究一些objc中内部调用机制和动画基础。

要想知道这些动画的组成，我们就要从比较低层次的API：CALayer的一些调用开始。iOS动画最终都是加到Layer中的，加入Layer就要调用Layer对象这个方法：

<pre>
- (void)addAnimation:(CAAnimation *)anim forKey:(NSString *)key;
</pre>

  所以只要我们知道了anim参数，并把anim动画对象的属性揪出来，就可以知道到底是什么动画了，但是这个方法是系统Framework中的，通常我们是无法能知道anim到底是什么。
这时我们就需要用一些objc的一些底层API：Objc Runtime来解决了。   

**Objc Runtime**

Objc Runtime是由一组处理Objctive-C动态语言运行时的API函数组成，这些函数都是一些比较底层的C函数。它有很多实用功能比如查看对象的成员，类/对象方法签名等等。这次我们要用的就是其中把对象方法调用替换的API。

<pre class="brush: oc;  ">
void method_exchangeImplementations(Method m1, Method m2) 
</pre>

这个函数的功能就是把类/对象的方法m1和m2进行调换。如果执行了这个函数，那么在App运行过程中所有调用方法m1的指令，最终都会执行成了方法m2。

**方法调换**

有了Objc Rumtime的API，就可以很方便的将调用系统库中方法的代码，执行成我们自己的代码了。所以我们想要知道Layer中加入了什么方法，只要把addAnimation:forKey:这个方法调换成我们自己的方法就行了。下面的这段代码就实现了这个功能。

<pre>

@implementation CALayer(Hacked)   

+ (void)load{   
    method_exchangeImplementations(class_getInstanceMethod([CALayer class], @selector(addAnimation:forKey:)), 
class_getInstanceMethod([CALayer class], @selector(hackedAddAnimation:forKey:)));   
}   

- (void)hackedAddAnimation:(CABasicAnimation *)anim forKey:(NSString *)key{
    [self hackedAddAnimation:anim forKey:key];
    if ([anim isKindOfClass:[CABasicAnimation class]]) {
        if ([anim.keyPath isEqualToString:@"transform"]) {
            if (anim.fromValue) {
                CATransform3D fromValue = [anim.fromValue CATransform3DValue];
                NSLog(@"From:%@",NSStringFromCGAffineTransform(CATransform3DGetAffineTransform(fromValue)));
            }
            if (anim.toValue) {
                CATransform3D toValue = [anim.toValue CATransform3DValue];
                NSLog(@"To:%@",NSStringFromCGAffineTransform(CATransform3DGetAffineTransform(toValue)));
            }
            if (anim.byValue) {
                CATransform3D byValue = [anim.byValue CATransform3DValue];
                NSLog(@"By:%@",NSStringFromCGAffineTransform(CATransform3DGetAffineTransform(byValue)));
            }
            NSLog(@"Duration:%.2f",anim.duration);
            NSLog(@"TimingFunction:%@",anim.timingFunction);
        }
    }
}

@end

</pre>

下面来说明一下上面的代码，这段代码是CALayer做了一个Catalog处理。其中initialize是一个类的方法，是进程开始时初始化类时调用，一般只有类有加载这个方法就会第一个调用了。hackedAddAnimation:forKey:是要被调换的代码。在类的初始化方法initialize中（代码中的第5行）实现了CALayer的addAnimation:forKey:和hackedAddAnimation:forKey:方法的调换。在hackedAddAnimation:forKey:中首先直接调用了[self hackedAddAnimation:forKey:]，也许你会问：这不死循环递归了么？其实不是，应为method_exchangeImplementations实现的是调换而不是替换，所以代码中调用addAnimation:forKey:运行时就成了调用hackedAddAnimation:forKey:。而代码中调用hackedAddAnimation:forKey:运行时成了调用addAnimation:forKey:。所以这里虽然写的是hackedAddAnimation:forKey:，实际上会调用系统Framework中的addAnimation:forKey:。这样做的目的是保证虽然我们把系统的方法改变了，我们还是调用系统的一次，以保持系统功能运行是正常的。在hackedAddAnimation:forKey:剩下的代码就只是把anim动画对象的各个属性的值打印出来了。

好了，把上面的这段代码粘贴到你的代码文件中。然后简单的写个UIAlertView弹出动画代码。

<pre>

UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Title" message:@"Message" delegate:nil cancelButtonTitle:@"Close" otherButtonTitles:nil];
[alert show];

</pre>

编译并运行上面这段代码，你就会在控制台中看到下面这些日志：

<pre>

2013-04-10 19:13:11.795 Test[10952:c07] From:[0.01, 0, 0, 0.01, 0, 0]
2013-04-10 19:13:11.796 Test[10952:c07] Duration:0.20
2013-04-10 19:13:11.796 Test[10952:c07] TimingFunction:easeInEaseOut
2013-04-10 19:13:11.999 Test[10952:c07] From:[1.1, 0, 0, 1.1, 0, 0]
2013-04-10 19:13:12.000 Test[10952:c07] Duration:0.10
2013-04-10 19:13:12.000 Test[10952:c07] TimingFunction:easeInEaseOut
2013-04-10 19:13:12.101 Test[10952:c07] From:[0.9, 0, 0, 0.9, 0, 0]
2013-04-10 19:13:12.101 Test[10952:c07] Duration:0.10
2013-04-10 19:13:12.101 Test[10952:c07] TimingFunction:easeInEaseOut

</pre>

查看CGAffineTransformMakeScale函数的头文件你会看到：

<pre>

/* Return a transform which scales by `(sx, sy)':
     t' = [ sx 0 0 sy 0 0 ] */

CG_EXTERN CGAffineTransform CGAffineTransformMakeScale(CGFloat sx, CGFloat sy)
  CG_AVAILABLE_STARTING(__MAC_10_0, __IPHONE_2_0);
  
</pre>

所以根据日志我们会发现这其实是3个关键帧动画，首先scale(缩放比例)从0.01放大到1.1，历时0.2秒；然后从1.1到0.9，历时0.1秒；那么最后就是从0.9到1.0（正常缩放比例），历时0.1秒。哈哈，那我们就简单的写个关键帧动画对象就可以表示UIAlertView的弹出动画效果了。

<pre>

CAKeyframeAnimation *popAnimation = [CAKeyframeAnimation animationWithKeyPath:@"transform"];
popAnimation.duration = 0.4;
popAnimation.values = @[[NSValue valueWithCATransform3D:CATransform3DMakeScale(0.01f, 0.01f, 1.0f)],
                        [NSValue valueWithCATransform3D:CATransform3DMakeScale(1.1f, 1.1f, 1.0f)],
                        [NSValue valueWithCATransform3D:CATransform3DMakeScale(0.9f, 0.9f, 1.0f)],
                        [NSValue valueWithCATransform3D:CATransform3DIdentity]];
popAnimation.keyTimes = @[@0.0f, @0.5f, @0.75f, @1.0f];
popAnimation.timingFunctions = @[[CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut],
                                 [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut],
                                 [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut]];
[anAlertAnimationView.layer addAnimation:popAnimation forKey:nil];

</pre>

你可以把popAnimation加入到你想进行动画的任何View中的layer中这样就实现了UIAlertView一样的弹出动画效果。

**结论**

UIAlertView动画其实是由三部分动画组成：缩放比例变化0.01->1.1->0.9->1.0。每次变化的时间函数（控制加速度）都是EaseInEaseOut。
在研究系统中调用函数的参数是我们可以用method_exchangeImplementations来hack到系统调用中去，但不要忘记调用系统本身的方法。否则容易导致App异常。当然，如果你是研究测试不怕crash，那随便。


**本文出自: [文章链接地址:http://www.tuicool.com/articles/E3iiQb]** 
