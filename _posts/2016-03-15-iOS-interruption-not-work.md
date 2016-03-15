---
layout : post
title : "iOS 设置AVAudioSessionInterruptionNotification通知无效"
category : iOS
duoshuo: true
date : 2016-03-15
tags : [iOS ,AVAudioSessionInterruptionNotification,Interruption]
---

**设置AVAudioSessionInterruptionNotification通知无效**  
**如下代码**


<pre class="brush: oc;  ">

[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(onAudioSessionEvent:) name:AVAudioSessionInterruptionNotification object:nil];

-(void)onAudioSessionEvent:(NSNotification*)noty{
    NSLog(@"%@",noty.userInfo);
    //    AVAudioSessionInterruptionTypeBegan
    //     AVAudioSessionInterruptionTypeEnded
    //      AVAudioSessionInterruptionTypeKey = 1;
    //     AVAudioSessionInterruptionTypeKey = 0;
    NSDictionary *dict = noty.userInfo;
    if ([dict[AVAudioSessionInterruptionTypeKey]integerValue]==AVAudioSessionInterruptionTypeBegan) {
        // pause
        
    }else if([dict[AVAudioSessionInterruptionTypeKey]integerValue]==AVAudioSessionInterruptionTypeEnded){
        // play
    }
}

</pre>

**解决办法**
将AVAudioSession中的配置选项设置为AVAudioSessionCategoryOptionDefaultToSpeaker即可。

**例如**

<pre class="brush: oc;  ">
- (void)setUpAudioSession {
    // 新建AudioSession会话
    AVAudioSession *audioSession = [AVAudioSession sharedInstance];
    
    // 设置后台播放
    NSError *error = nil;
    [audioSession setCategory:AVAudioSessionCategoryPlayback withOptions:AVAudioSessionCategoryOptionDefaultToSpeaker error:&error];
    if (error) {
        NSLog(@"Error setCategory AVAudioSession: %@", error);
    }
    
    NSError *activeSetError = nil;
    // 启动AudioSession，如果一个前台app正在播放音频则可能启动失败
    [audioSession setActive:YES error:&activeSetError];
    if (activeSetError) {
        NSLog(@"Error activating AVAudioSession: %@", activeSetError);
    }
}

</pre>
