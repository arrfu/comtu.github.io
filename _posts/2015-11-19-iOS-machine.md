---
layout : post
title : "iOS 获取设备机器型号"
category : iOS
duoshuo: true
date : 2015-11-19
tags : [iOS ,machine, oc ]
---

**获取iPhone手机设备型号:**

<pre class="brush: oc;  ">
#import <sys/utsname.h>

struct utsname systemInfo;
uname(&systemInfo);
NSString *deviceString = [NSString stringWithCString:systemInfo.machine encoding:NSUTF8StringEncoding];
NSLog(@"machine = %@",deviceString);

</pre>

输出为：machine = iPhone5,2   
此方法可以获取手机硬件设备的机器型号，如iPhone5，iPhone5S,iPhone6，iPhone6S等。   

参考资料来源：   
[http://blog.sina.com.cn/s/blog_6f0c918801015lhy.html](http://blog.sina.com.cn/s/blog_6f0c918801015lhy.html)
