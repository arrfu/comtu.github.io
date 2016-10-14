---
layout : post
title : "iOS10 相册图片二维码识别"
category : iOS
duoshuo: true
date : 2016-10-14
tags : [iOS10 ,PhotoLibrary,QR Code]
---

#1、问题描述：   
在iOS8，iOS9下，打开相册里的图片识别二维码功能正常，但在iOS10下，该功能失效。    

打开相册代码：

<pre class="brush: oc;  ">
- (void)openImagePicker {
    UIImagePickerControllerSourceType sourceType = UIImagePickerControllerSourceTypePhotoLibrary;
    if (![UIImagePickerController isSourceTypeAvailable: UIImagePickerControllerSourceTypeCamera]) {
        sourceType = UIImagePickerControllerSourceTypePhotoLibrary;
    }

    UIImagePickerController *pickerVC = [[UIImagePickerController alloc] init];
    pickerVC.delegate = self;
    pickerVC.allowsEditing = NO;
    pickerVC.sourceType = sourceType;
    [self presentViewController:pickerVC animated:YES completion:nil];
    
}
</pre>

识别二维码：   
<pre class="brush: oc;">
+ (NSString *)readQRCodeImage:(UIImage *)imagePicked {
    CIImage *qrcodeImage = [CIImage imageWithCGImage:imagePicked.CGImage];
    CIContext *qrcodeContext = [CIContext contextWithOptions:nil];
    
    //检测图片中的二维码，并设置检测精度为高
    CIDetector *qrcodeDetector = [CIDetector detectorOfType:CIDetectorTypeQRCode context:qrcodeContext options:@{CIDetectorAccuracy:CIDetectorAccuracyHigh}];
    
    //读取图片的qrcode特性
    NSArray *qrcodeFeatures = [qrcodeDetector featuresInImage:qrcodeImage];
    
    //返回的结果，只读取一条
    NSString *qrcodeResultString = nil;
    if (qrcodeFeatures && qrcodeFeatures.count > 0) {
        for (CIQRCodeFeature *qrcodeFeature in qrcodeFeatures) {
            if (qrcodeResultString && qrcodeResultString.length > 0) {
                break;
            }
            qrcodeResultString = qrcodeFeature.messageString;
        }
    }
    NSLog(@"%@",qrcodeResultString);
    
    return qrcodeResultString;
}
</pre>

在iOS10下提示错误：
<pre class="brush: oc">
[qrcodeDetector featuresInImage:qrcodeImage]；   
</pre>

解决办法：设置相册图片为可编辑属性，即可解决此问题   
<pre class="brush:oc">
pickerVC.allowsEditing = Yes;
</pre>

#2.iOS10下相关权限适配：   

在Info.plist中添加字段   
<pre class="brush:oc">
Privacy - Camera Usage Description             打开相机   
Privacy - Photo Library Usage Description      打开相册   
</pre>
