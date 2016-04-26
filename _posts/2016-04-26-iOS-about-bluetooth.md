---
layout : post
title : "iOS 蓝牙4.0 CoreBluetooth 框架的使用"
category : iOS 优秀技术文章摘录
duoshuo: true
date : 2016-04-26
tags : [iOS ,CoreBluetooth,CBPeripheral]
---



***
**文章一：**
**本文出自:coderyi  [文章链接地址:http://www.coderyi.com/archives/137)**   
***
   
     
内容简介:      
   
  iOS开发蓝牙4.0的框架是CoreBluetooth，本文主要介绍CoreBluetooth的使用，关于本文中的代码片段大多来自github上的一个demo，地址是myz1104/Bluetooth。   
在CoreBluetooth中有两个主要的部分,Central和Peripheral，有一点类似ClientServer。CBPeripheralManager作为周边设备是服务器。CBCentralManager作为中心设备是客户端。所有可用的iOS设备可以作为周边（Peripheral）也可以作为中央（Central），但不可以同时既是周边也是中央。   

一般手机是客户端， 设备（比如手环）是服务器，因为是手机去连接手环这个服务器。   
周边（Peripheral）是生成或者保存了数据的设备，中央（Central）是使用这些数据的设备。你可以认为周边是一个广播数据的设备，他广播到外部世界说他这儿有数据，并且也说明了能提供的服务。   
另一边，中央开始扫描附近有没有服务，如果中央发现了想要的服务，然后中央就会请求连接周边，一旦连接建立成功，两个设备之间就开始交换传输数据了。      
除了中央和周边，我们还要考虑他俩交换的数据结构。这些数据在服务中被结构化，每个服务由不同的特征（Characteristics）组成，特征是包含一个单一逻辑值的属性类型。      


**[详细文章链接地址:http://www.coderyi.com/archives/137)**   



***
**文章二：**
**本文出自:liuyanwei  [文章链接地址:http://liuyanwei.jumppo.com/2015/07/17/ios-BLE-1.html)**   
***

内容简介: 

1.iOS蓝牙开发（一）蓝牙相关基础知识    
蓝牙常见名称和缩写       
MFI ======= make for ipad ,iphone, itouch 专们为苹果设备制作的设备   
BLE ==== buletouch low energy，蓝牙4.0设备因为低耗电，所以也叫做BLE   
peripheral,central == 外设和中心,发起连接的时central，被连接的设备为perilheral   
service and characteristic === 服务和特征    每个设备会提供服务和特征，类似于服务端的api，但是机构不同。每个外设会有很多服务，每个服务中包含很多字段，这些字段的权限一般分为    读read，写write，通知notiy几种，就是我们连接设备后具体需要操作的内容。   
Description 每个characteristic可以对应一个或多个Description用户描述characteristic的信息或属性   
MFI === 开发使用ExternalAccessory 框架   
4.0 BLE === 开发使用CoreBluetooth 框架   

2.ios蓝牙开发（二）ios连接外设的代码实现   
>1. 建立中心角色
>2. 扫描外设（discover）
>3. 连接外设(connect)
>4. 扫描外设中的服务和特征(discover)
    - >4.1 获取外设的services
    - >4.2 获取外设的Characteristics,获取Characteristics的值，获取Characteristics的Descriptor和Descriptor的值
>5. 与外设做数据交互(explore and interact)
>6. 订阅Characteristic的通知
>7. 断开连接(disconnect)

3.ios蓝牙开发（三）app作为外设被连接的实现      
peripheral模式的流程   

>1. 打开peripheralManager，设置peripheralManager的委托
>2. 创建characteristics，characteristics的description 创建service，把characteristics添加到service中，再把service添加到peripheralManager中
>3. 开启广播advertising
>4. 对central的操作进行响应
    >4.1 读characteristics请求
    >4.2 写characteristics请求
    >4.4 订阅和取消订阅characteristics
    
4.ios蓝牙开发（四）BabyBluetooth蓝牙库介绍   
>1.基于原生CoreBluetooth框架封装的轻量级的开源库，可以帮你更简单地使用CoreBluetooth API。   
>2.CoreBluetooth所有方法都是通过委托完成，代码冗余且顺序凌乱。BabyBluetooth使用block方法，可以重新按照功能和顺序组织代码，并提供许多方法减少蓝牙开发过程中的代码量。   
>3.链式方法体，代码更简洁、优雅。   
>4.通过channel切换区分委托调用，并方便切换   

**[详细文章链接地址:http://liuyanwei.jumppo.com/2015/07/17/ios-BLE-1.html)**   
