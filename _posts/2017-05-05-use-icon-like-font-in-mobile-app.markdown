---
layout: post
title: "在移动APP中像使用字体一样使用图标"
date: 2017-04-04 15:38:19
author: "cathor"
catalog: true
tags:
    - ios
    - android
---


## 概述

一直以来在移动APP的开发过程中，经常会碰到使用图标的地方，比如：Tabbar、导航栏、工具栏上的按钮；或者TableView上每行的图标。而且，各个地方大小、颜色也不尽相同。

## 实现原来



## iOS实现

将字体文件复制到项目中，在plist中自定自定义字体

{% highlight xml %}

	<key>UIAppFonts</key>
	<array>
		<string>iconfont.ttf</string>
		<string>STHeiti Light.ttc</string>
	</array>
{% endhighlight %}



{% highlight swift %}

    public class func image(unicode: NSString, size: CGFloat, color: UIColor?) -> UIImage? {
        let imageSize = CGSize(width: size, height: size)
        let font = UIFont(name: "iconfont", size: size)!;
        
        var opt: [String : Any] = [NSFontAttributeName: font]
        if (color != nil) {
            opt[NSForegroundColorAttributeName] = color!
        }
        
        UIGraphicsBeginImageContextWithOptions(imageSize, false, 0.0)
        
        unicode.draw(in: CGRect(x: 0, y: 0, width: size, height: size), withAttributes: opt)
        
        let result = UIGraphicsGetImageFromCurrentImageContext()
        
        UIGraphicsEndImageContext()
        
        return result
    }

{% endhighlight %}

## Android实现