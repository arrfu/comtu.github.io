---
layout : post
title : "iOS 后台无限运行"
category : iOS
duoshuo: true
date : 2015-11-11
tags : [iOS ,EnterBackground, oc ]
---


1.各个程序运行状态时代理的回调：

- (BOOL)application:(UIApplication *)application willFinishLaunchingWithOptions:(NSDictionary *)launchOptions
  告诉代理进程启动但还没进入状态保存
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
  告诉代理启动基本完成程序准备开始运行
- (void)applicationWillResignActive:(UIApplication *)application
  当应用程序将要入非活动状态执行，在此期间，应用程序不接收消息或事件，比如来电话了
- (void)applicationDidBecomeActive:(UIApplication *)application 
  当应用程序入活动状态执行，这个刚好跟上面那个方法相反
- (void)applicationDidEnterBackground:(UIApplication *)application
  当程序被推送到后台的时候调用。所以要设置后台继续运行，则在这个函数里面设置即可
- (void)applicationWillEnterForeground:(UIApplication *)application
	当程序从后台将要重新回到前台时候调用，这个刚好跟上面的那个方法相反。
- (void)applicationWillTerminate:(UIApplication *)application
	当程序将要退出是被调用，通常是用来保存数据和一些退出前的清理工作。这个需要要设置UIApplicationExitsOnSuspend的键值。
- (void)applicationDidFinishLaunching:(UIApplication*)application
	当程序载入后执行

资料来源:

[后台无线运行Demo源代码:https://github.com/mddios/runInBackground](https://github.com/mddios/runInBackground)
[http://www.cnblogs.com/stoic/archive/2012/10/08/2785182.html](http://www.cnblogs.com/stoic/archive/2012/10/08/2785182.html)
