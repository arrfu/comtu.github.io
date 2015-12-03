---
layout : post
title : "iOS AppStore 评分Demo"
category : iOS
duoshuo: true
date : 2015-12-03
tags : [iOS ,appStore, score ]
---

**总结了2种进入App Store页面评分的方式  
分别是：  
1.直接跳转至AppStore商店应用。
2.应用内部嵌入AppStore商店应用。  
[Demo例子地址：](http://www.tuicool.com/articles/ZZzQru) **  

**方法一：直接通过URL地址跳转至AppStore**    

<pre class="brush: oc;  ">

+(void)turnIntoAppStoreScoresUrl:(NSString *)url appID:(NSString *)ID{
    
    NSString *appUrl = [NSString stringWithFormat:@"%@%@",url,ID];
    
    [[UIApplication sharedApplication] openURL:[NSURL URLWithString:appUrl]];
}

NSString *appID = @"1037988113";
NSString *appUrl = @"itms-apps://itunes.apple.com/WebObjects/MZStore.woa/wa/viewContentsUserReviews?type=Purple+Software&id=";
   
[SLTurnIntoAppStoreScores turnIntoAppStoreScoresUrl:appUrl appID:appID];

</pre>




参考资料来源：   
[http://www.tuicool.com/articles/ZZzQru](http://www.tuicool.com/articles/ZZzQru)   
