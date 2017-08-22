---
layout: post
title: "iOS开发小技巧（七） -- 在移动APP中像使用字体一样使用图标"
date: 2017-08-16 15:38:19
author: "cathor"
catalog: true
tags:
    - iOS
    - iOS开发小技巧
---



> 一直以来在移动APP的开发过程中，经常会碰到使用图标的地方，比如：Tabbar、导航栏、工具栏上的按钮；或者TableView上每行的图标。如何有效的管理这些图标，并且能够方便的使用它们？

这里我先介绍一个工具：[http://www.iconfont.cn/](http://www.iconfont.cn/) 这是阿里巴巴旗下的一个图标管理平台，它可以有效的管理你不同项目中的图标，并且将图表导出成一个ttf文件，每个图标对应一个不同的Unicode代码。最后，我们只需要将Unicode按这个ttf文件的字体转换成UIImage就可以了。（这里的原理，其实和显示汉字和emoji字体是一样的，比如：“我”这个汉字，它的Unicode编码是“\u6211”，但在屏幕上看到的其实是某个字体对应这个Unicode代码的点阵图，这个点阵图即可以是“我”也可以其他样子的图片）



## iOS实现代码

将字体文件复制到项目中，在plist中添加自定义字体

{% highlight xml %}

	<key>UIAppFonts</key>
	<array>
		<string>iconfont.ttf</string>
	</array>
{% endhighlight %}


下面是swift实现代码，将unicode代码转换成UIImage对象

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

