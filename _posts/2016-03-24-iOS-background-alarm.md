---
layout : post
title : "iOS 设置后台定时闹铃"
category : iOS
duoshuo: true
date : 2016-03-24
tags : [iOS ,AVAudioSession,alarm]
---

**一.什么是AVAudioSession ？**  

1). Audio Session 各个参数类别的作用及定义：
[Audio Session 各个参数类别的作用及定义地址:](https://developer.apple.com/library/prerelease/tvos/documentation/Audio/Conceptual/AudioSessionProgrammingGuide/ConfiguringanAudioSession/ConfiguringanAudioSession.html)

2).配置音频后台播放及官方资料：
[配置音频后台播放及官方资料地址:](https://developer.apple.com/library/ios/qa/qa1668/_index.html)

**二.AVAudioSession在开发中的应用**

1）应用里的音频播放时是否要和其他应用的音频实现混音？或者让其他音频静音？   
2）当iOS的闹钟响时应用内的音频是否要暂停？   
3）当用户插拔耳机时应用应该如何反应？拔出耳机时是否要停止音乐？   
4）关闭屏幕后音频是否暂停？   


**三.后台定时闹钟的实现**  

**1.实现方式：**

1).设置后台无限运行   
方式：当程序退回后台时，系统会允许应用5分钟的存活时间。在这段时间里，启动一个定时器，每间隔一两分钟就执行播放一段非常短的无声音频，来继续获取系统重新的5分钟的存活时间，达到在后台无限运行的效果。   

[后台无线运行Demo例子: https://github.com/mddios/runInBackground](https://github.com/mddios/runInBackground)<br />

2).设置定时闹钟   
方式：使用本地通知 UILocalNotification + 播放器AVAudioPlayer   

3).闹钟响起时   
方式：弹出本地通知及播放指定歌曲   

**2.遇到的问题：**

问题1：   
     在设置定时闹钟后，退回后台；打开第三方播放器播（例如酷狗音乐）播放歌曲，当定时闹钟时间到后，弹出本地通知，但是设置的播放闹铃歌曲并没有播放出来。经调试发现，此时在后台中执行setActive失败：   
  
<pre class="brush: oc;  ">
    BOOL activated = [[AVAudioSession sharedInstance] setActive:YES error:&error];
    if(activated)
    {
      NSLog(@"OK");
    }else {
      NSLog(@"error: %@",error);
    }
</pre>
    
    
解决办法：   
    在定时闹钟时间到后，设置AVAudioSession参数为允许其他播放器混音选项AVAudioSessionCategoryOptionMixWithOthers；这样就能解决后台播放失败的问题。   

<pre class="brush: oc;  ">
  // 设置接受远程控制事件
  [[UIApplication sharedApplication] beginReceivingRemoteControlEvents]; 
  
  AVAudioSession *session = [AVAudioSession sharedInstance];

  //设置允许混音
  [session setCategory:AVAudioSessionCategoryPlayback withOptions:AVAudioSessionCategoryOptionMixWithOthers error:nil];
                
  [session setActive:YES error:nil];
                
  //播放歌曲
  _play = [[AVAudioPlayer alloc]initWithContentsOfURL:url error:nil];
  [_play play];
</pre>

问题2：   
     设置了混音AVAudioSessionCategoryOptionMixWithOthers后，虽然能决解了后台定时闹钟响铃失败的问题，但是若此时打开自己APP的播放器播放歌曲，会发现自己的APP无法暂停其他APP播放器（如酷狗播放器）的音乐；别的APP播放器也暂停不了自己APP的音乐。   
     调试发现，之前设置的接收中断通知服务失效：
     
<pre class="brush: oc;  ">

//监听电话进来 或 打开其他APP音乐播放歌曲时接收的中断信息
[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(onAudioSessionEvent:) name:AVAudioSessionInterruptionNotification object:nil];
    
// 中断处理
-(void)onAudioSessionEvent:(NSNotification*)noty{
    NSLog(@"%@",noty.userInfo);

    NSDictionary *dict = noty.userInfo;
    if ([dict[AVAudioSessionInterruptionTypeKey]integerValue]==AVAudioSessionInterruptionTypeBegan) {
        //正当音乐被其他应用或者电话中断时
    }else if([dict[AVAudioSessionInterruptionTypeKey]integerValue]==AVAudioSessionInterruptionTypeEnded){
       //当中断结束时
    }
}
</pre>

解决办法：   
     在应用进入前台时，设置AVAudioSession的参数为AVAudioSessionCategorySoloAmbient选项，即关闭其他APP应用播放器音乐；但是此选项不支持后台音乐播放，故需要再次设置AVAudioSession的参数为AVAudioSessionCategoryPlayback，即允许后台播放歌曲。
     
<pre class="brush: oc;  ">
// 应用进入前台
- (void)applicationDidBecomeActive:(UIApplication *)application
{
    //允许应用程序接收远程控制
    [[UIApplication sharedApplication] beginReceivingRemoteControlEvents];

    AVAudioSession *session = [AVAudioSession sharedInstance];
    
    // 设置为暂停其他播放器音乐
    [session setCategory:AVAudioSessionCategorySoloAmbient error:nil];

    // 设置播放器为允许后台播放
    [session setCategory:AVAudioSessionCategoryPlayback withOptions:AVAudioSessionCategoryOptionDefaultToSpeaker error:nil];
    [session setActive:YES error:nil];
}


</pre>
