---
layout: post
title: "在iOS系统中，如何正确缩放UIImage对象"
date: 2016-03-18 10:13:36
author: "cathra"
catalog: true
tags: 
    - ios
---

在iOS开发中，常常会用到需要缩放UIImage图片的情况，那么如何才能保证缩放图片的清晰度呢

<!-- more -->

UIImage+Extension.m

``` ObjC

- (UIImage *)scaleImageToScale:(CGFloat)scaleSize {
    UIGraphicsBeginImageContextWithOptions(CGSizeMake(self.size.width * scaleSize, self.size.height * scaleSize), 
    									   NO, 
    									   [[UIScreen mainScreen] scale]);
    
    [self drawInRect:CGRectMake(0, 0, self.size.width * scaleSize, self.size.height * scaleSize)];
    
    UIImage *scaledImage = UIGraphicsGetImageFromCurrentImageContext();
    UIGraphicsEndImageContext();
    
    return scaledImage;
}

```


相关链接：
===

[Image Resizing Techniques](http://nshipster.com/image-resizing/ "Image Resizing Techniques")
[Resize a UIImage the right way](http://vocaro.com/trevor/blog/2009/10/12/resize-a-uiimage-the-right-way/ "Resize a UIImage the right way")

