---
layout: post
title: "正确缩放UIImage对象"
date: 2016-10-25 10:13:36
author: "cathra"
catalog: true
tags:
    - iOS
---

## 实现代码

UIImage+Extension.m

{% highlight bash %}

- (UIImage *)scaleImageToScale:(CGFloat)scaleSize {
    UIGraphicsBeginImageContextWithOptions(CGSizeMake(self.size.width * scaleSize, self.size.height * scaleSize), 
    									   NO, 
    									   [[UIScreen mainScreen] scale]);
    
    [self drawInRect:CGRectMake(0, 0, self.size.width * scaleSize, self.size.height * scaleSize)];
    
    UIImage *scaledImage = UIGraphicsGetImageFromCurrentImageContext();
   	UIGraphicsEndImageContext();
    
    return scaledImage;
}

{% endhighlight %}


## 相关链接：


[Image Resizing Techniques](http://nshipster.com/image-resizing/ "Image Resizing Techniques")
[Resize a UIImage the right way](http://vocaro.com/trevor/blog/2009/10/12/resize-a-uiimage-the-right-way/ "Resize a UIImage the right way")

