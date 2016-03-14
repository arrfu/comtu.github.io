---
layout : post
title : "iOS [UIApplication sharedApplication].keyWindow 添加视图无效
category : iOS
duoshuo: true
date : 2016-03-14
tags : [iOS ,keyWindow]
---

**[UIApplication sharedApplication].keyWindow 添加视图无效**  
**解决办法:**

iOS 7使用：
<pre class="brush: oc;  ">

[[[[UIApplication sharedApplication] delegate] window] addSubview:view];

</pre>

iOS 8以上使用：
<pre class="brush: oc;  ">

[[UIApplication sharedApplication].keyWindow addSubview:view];

</pre>

原因：
      在iOS 7中，调试输出可已看到[UIApplication sharedApplication].keyWindow的值为nil ，这是因为 appdelegate 中的keyWindow没有创建成功
      ，故添加无效。在iOS 8中，苹果解决了此问题。

