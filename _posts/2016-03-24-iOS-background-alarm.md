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
     在设置定时闹钟后，退回后台；打开第三方播放器播（例如酷狗音乐）播放歌曲，当定时闹钟时间到后，弹出本地通知，但是设置的播放闹铃歌曲   
  并没有播放出来。经调试发现，此时在后台中执行setActive失败：   
  
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
    在定时闹钟时间到后，设置AVAudioSession参数为允许其他播放器混音选项AVAudioSessionCategoryOptionMixWithOthers；这样就能解决后台播放   
  失败的问题。   

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
