---
layout : post
title : "iOS 服务器返回空值NULL及崩溃的解决方式总结"
category : iOS
duoshuo: true
date : 2016-11-24
tags : [iOS ,json,crash]
---

******

**一、产生问题现象及原因：**  

  客户端在访问服务器接口时，理论上服务器都会返回对应的有效数据。但是在现实开发中，往往会出现不尽人意的情况。例如：   
  
  * 服务器端返回null数据，若直接对该空值进行赋值或访问操作，会导致程序崩溃。
  
  * 由于接口变动或新需求等原因，常常返回字段缺失或者新增字段等情况，这时候若直接访问缺失字段会导致程序崩溃。
  
  * 由于服务器开发人员失误或者接口需求变动，将之前约定返回某个字段的类型改变为其他类型；例如json中某个字段为string型，但是服务器端却返回的是int型，
    这种情况也很容易导致程序崩溃。
    
    ...
    
    
**二、解决办法：**  

  由上面的几个例子可以看出，无论是哪方面的原因，作为一个合格的开发人员，都必须要做好各种情况的处理，提高程序的健壮性。而不是别人随随便便传个空值给你，
你就崩了，这样弱不禁风的应用，想必不是你真正想要的。   

  那怎么处理才能解决这个问题呢？我找到的几种解决方式总结如下：`(PS:如果你有更好的解决方案，请务必告知一声，非常感谢！)`      
  
  * **1.较直接的方式：**
  
    当我们使用AFNetwork第三方库访问服务器的时候，可以用它自带的清除空值属性 `removesKeysWithNullValues` 为我们自动处理返回数据中携带空值的字段。  
    
{% highlight Objective-C %}
/**
 Whether to remove keys with `NSNull` values from response JSON. Defaults to `NO`.
 */
@property (nonatomic, assign) BOOL removesKeysWithNullValues;
      
{% endhighlight %}

   使用方式，直接设置为Yes即可：
{% highlight Objective-C %}
  self.removesKeysWithNullValues = YES;
{% endhighlight %}
   这样的话,后台返回的JSON数据中空的键值对,将会被自动删除,可以避免我们对这些空值做操作,造成崩溃问题.   
   
  
  * **2.使用第三方框架：**
  
   利用第三方框架处理服务器返回的JSON数据，将空值设置为nil，而nil是安全的，可以向nil对象发送任何message而不会奔溃。      
   
   github上比较多人使用的是`NullSafe` ，使用方式简单，直接将该分类拖入工程中即可。        
   
   [链接地址：https://github.com/nicklockwood/NullSafe](https://github.com/nicklockwood/NullSafe)       
   
   
  
  * **3.给自己的程序添加容错处理：**
  
    很多人为了方便省事，常常直接对传进来的参数做各种操作处理，而不考虑该参数是否合法，这样对自己的程序往往是致命性的。    
    
    好的编程习惯应当给自己编写的代码添加容错处理，判断参数的合法性，增强程序的健壮性尤为重要。     



**未完待续。。。**    

