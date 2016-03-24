---
layout : post
title : "iOS 设置后台定时闹铃"
category : iOS
duoshuo: true
date : 2016-03-24
tags : [iOS ,AVAudioSession,alarm]
---

**一.什么是AVAudioSession ？**  

**二.AVAudioSession在开发中的应用**

1）应用里的音频播放时是否要和其他应用的音频实现混音？或者让其他音频静音？   
2）当iOS的闹钟响时应用内的音频是否要暂停？   
3）当用户插拔耳机时应用应该如何反应？拔出耳机时是否要停止音乐？   
4）关闭屏幕后音频是否暂停？   


**三.后台定时闹钟的实现**  

**1.实现方式：**

1).设置后台无限运行   

2).设置定时闹钟   
方式：使用本地通知 UILocalNotification + 播放器AVAudioPlayer   

3).闹钟响起时   

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

<pre class="brush: oc;  ">

</pre>
