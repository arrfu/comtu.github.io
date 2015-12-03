---
layout : post
title : "iOS AppStore 评分Demo"
category : iOS
duoshuo: true
date : 2015-12-03
tags : [iOS ,appStore, score ]
---

**今天整理了下评分模块，总结了2种进入App Store页面评分的方式，记录下来，分享给大家**  
分别是：  
1.直接跳转至AppStore商店应用。  
2.应用内部嵌入AppStore商店应用。    
[Demo例子地址：](http://www.tuicool.com/articles/ZZzQru)     

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


**方法二：通过调用SKStoreProductViewController类，在应用中嵌入AppStore商店**  
从iOS6以后苹果提供了在应用内部打开App Store中某一个应用下载页面的方式，提供了一个SKStoreProductViewController的类对该功能进行支持。   
使用条件：   
1.导入#import <StoreKit/StoreKit.h>   
2.遵守<SKStoreProductViewControllerDelegate>这个协议设置代理。   
3.调用以下代码即可：   

<pre class="brush: oc;  ">

/**
 * 实例化一个SKStoreProductViewController类
 */
- (void)openAppWithIdentifier:(NSString *)appId {
    SKStoreProductViewController *storeProductVC = [[SKStoreProductViewController alloc] init];
    storeProductVC.delegate = self;
    
    NSDictionary *dict = [NSDictionary dictionaryWithObject:appId forKey:SKStoreProductParameterITunesItemIdentifier];
    [storeProductVC loadProductWithParameters:dict completionBlock:^(BOOL result, NSError *error) {
        if (result) {
            [self presentViewController:storeProductVC animated:YES completion:nil];
        }
    }];
}


#pragma mark - SKStoreProductViewControllerDelegate 嵌入应用商店
/**
 * 按取消按钮Cancel返回所调用代理方法,此处返回到ViewController控制器
 */
- (void)productViewControllerDidFinish:(SKStoreProductViewController *)storeProductVC {
    [storeProductVC dismissViewControllerAnimated:YES completion:^{
        ViewController *moreVc = [[ViewController alloc]init];
        [self.navigationController pushViewController:moreVc animated:YES];
    }];
    
}

// 使用   
[self openAppWithIdentifier:@"1037988113"];

</pre>

参考资料来源：   
[http://www.tuicool.com/articles/ZZzQru](http://www.tuicool.com/articles/ZZzQru)   
