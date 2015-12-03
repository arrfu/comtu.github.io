---
layout : post
title : "iOS 类前缀使用原则"
category : iOS
duoshuo: true
date : 2015-12-03
tags : [iOS ,class prefix, oc ]
---

**前缀**
在Objective-C应用中的所有类名都必须是全局唯一的。由于很多不同的框架中会有一些相似的功能，所以在名字上也可能   
会有重复（users， views， requests / responses 等等），所以苹果官方文档规定类名需要有2-3个字母作为前缀。

**类前缀**
苹果官方建议两个字母作为前缀的类名是为官方的库和框架准备的，而对于作为第三方开发者的我们，官方建议使用3个或   
者更多的字母作为前缀去命名我们的类:   
[官网地址:https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Conventions](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Conventions/Conventions.html)。   

**一个资深的Mac或iOS开发者可能会记得下面大部分的缩写标识符：**

Prefix	Frameworks    
AB	AddressBook / AddressBookUI   
AC	Accounts   
AD	iAd   
AL	AssetsLibrary   
AU	AudioUnit   
AV	AVFoundation   
CA	CoreAnimation   
CB	CoreBluetooth   
CF	CoreFoundation / CFNetwork   
CG	CoreGraphics / QuartzCore / ImageIO   
CI	CoreImage   
CL	CoreLocation   
CM	CoreMedia / CoreMotion   
CV	CoreVideo   
EA	ExternalAccessory   
EK	EventKit / EventKitUI   
GC	GameController   
GLK	GLKit   
JS	JavaScriptCore   
MA	MediaAccessibility   
MC	MultipeerConnectivity   
MF	MessageUI   
MIDI	CoreMIDI   
MK	MapKit   
MP	MediaPlayer   
NK	NewsstandKit   
NS	Foundation, AppKit, CoreData   
PK	PassKit   
QL	QuickLook   
SC	SystemConfiguration   
Se	Security   
SK	StoreKit / SpriteKit   
SL	Social   
SS	Safari Services   
TW	Twitter    
UI	UIKit   
UT	MobileCoreServices    

**第三方类前缀**
直到最近，由于CocoaPods的出现和大量新的iOS开发者的涌现，开源代码的遍布，第三方代码在很大程度上对苹果和其余的Objective-C开发社区来说已经不是问题了。最近苹果官方的命名指南也发生了变化，它将三个字母作为前缀的建议只是做为一个习惯做法。

正因为这样，那些已经存在的第三方库依然使用2个字母作为前缀，你可以参考一些那些在GitHub上得到很多start的Objective-C的仓库。

Prefix	Frameworks   
AF	AFNetworking (“Alamofire”)   
RK	RestKit   
PU	GPUImage   
SD	SDWebImage   
MB	MBProgressHUD   
FB	Facebook SDK   
FM	FMDB (“Flying Meat”)   
JK	JSONKit   
UI	FlatUI   
NI	Nimbus   
AC	Reactive Cocoa   

我们已经看到这个第三方库的前缀已经和我的AFNetworking一样了，所以最好还是要在你的代码中遵守要三个字母以上的作为类前缀的规定。


参考资料来源：   
[http://mobile.51cto.com/hot-437049.htm](http://mobile.51cto.com/hot-437049.htm)   
