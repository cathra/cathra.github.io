---
layout: post
title: "iOS开发小技巧汇总"
date: 2018-07-24 20:36:19
author: "骄傲的猫"
catalog: true
tags:
    - iOS
    - ObjC
---



# 系统实现点击通知栏弹出窗体


Push Notification是ios系统中，后台主动向APP推送消息的重要手段。有时我们希望推送一个标题过来后，点击系统的推送栏，能够在APP中弹出一个ViewController显示消息相关的具体内容。本文将对不同情况下的消息推送处理进行介绍。


<!-- more -->

ios对于消息推送的处理有3种情况：

1、 程序没有运行，点通知栏，程序开始执行application:didFinishLaunchingWithOptions:方法的launchOptions参数中，包含提送信息。

``` ObjC
    - (BOOL)application:(UIApplication *)application 
    	didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        //获取推送信息
        NSDictionary *userInfo = [launchOptions objectForKey:UIApplicationLaunchOptionsRemoteNotificationKey];
        
        ...

        [self.window setRootViewController: viewController];
        [self.window makeKeyAndVisible];

        //显示消息ViewController

        return YES;
    }
```


2、 程序Inactive状态，点通知栏或解锁屏幕，程序进入active状态，回调application:didReceiveRemoteNotification:fetchCompletionHandler:方法，通过判断application参数的applicationState判断，收到的推送是否是在inactive状态下。

``` ObjC
    -(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo 
        fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler {

        BOOL isInactive = application.applicationState == UIApplicationStateInactive;
        if (isInactive) {
    	    //如果实在inactive状态下收到推送
    	    ...
        } else {
    	    //如果实在active状态下收到推送
    	    ...
        }
        
        ...
    }
```


3、 程序运行状态下，收到通知，和2相同都是回调application:didReceiveRemoteNotification:fetchCompletionHandler:方法，只不过application参数的applicationState值是UIApplicationStateActive



# 实现UISearchBar的Placeholder居左显示


从ios7开始，ios系统UISearchBar组件显示Placeholder图标提示信息和放大镜都是居中的，而且没有相应的方法、属性对placeholder进行操作。本文提供一个UISearchBar的扩展，使placeholder的内容居左



UISearchBar+Extension.h

``` ObjC
    #import <UIKit/UIKit.h>
    
    @interface UISearchBar(Extension)
    
    -(void)setLeftPlaceholder:(NSString *)placeholder;
    
    @end
```


UISearchBar+Extension.m

``` ObjC
    #import "UISearchBar+Extension.h"
    
    @implementation UISearchBar(Extension)
    
    -(void)setLeftPlaceholder:(NSString *)placeholder {
        self.placeholder = placeholder;
            
        SEL centerSelector = NSSelectorFromString([NSString stringWithFormat:@"%@%@", @"setCenter", @"Placeholder:"]);
        if ([self respondsToSelector:centerSelector]) {
            BOOL centeredPlaceholder = NO;
            NSMethodSignature *signature = [[UISearchBar class] instanceMethodSignatureForSelector:centerSelector];
            NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:signature];
            [invocation setTarget:self];
            [invocation setSelector:centerSelector];
            [invocation setArgument:&centeredPlaceholder atIndex:2];
            [invocation invoke];
        }
    }
    
    @end
```

# UISearchBar 光标不显示的问题


> 今天发现做的App中的UISearchBar中的光标不显示，网上找了一下解决办法如下：

``` ObjC
	[searchBar setTintColor:[UIColor blueColor]];
```

或者不设置tintColor就可以了




# 正确缩放UIImage对象

在iOS开发中，常常会用到需要缩放UIImage图片的情况，那么如何才能保证缩放图片的清晰度呢


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



# weakSelf/strongSelf在Block中的使用


> Objective C 的 Block 是一个很实用的语法，特别是与GCD结合使用，可以很方便地实现并发、异步任务。
> 但是，如果使用不当，Block 也会引起一些循环引用问题(retain cycle)—— Block 会 retain ‘self’，而 ‘self‘ 又 retain 了 Block。
> 因为在 ObjC 中，直接调用一个实例变量，会被编译器处理成 ‘self->theVar’，’self’ 是一个 strong 类型的变量，引用计数会加 1，
> 于是，self retains queue， queue retains block，block retains self。


# 解决 retain circle


Apple 官方的建议是，传进 Block 之前，把 ‘self’ 转换成 weak automatic 的变量，这样在 Block 中就不会出现对 self 的强引用。如果在 Block 执行完成之前，self 被释放了，weakSelf 也会变为 nil。

示例代码：

{% highlight bash %}

__weak __typeof__(self) weakSelf = self;
dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
    [weakSelf doSomething];
});

{% endhighlight %}


clang 的文档表示，在 doSomething 内，weakSelf 不会被释放。但，下面的情况除外：

{% highlight bash %}

__weak __typeof__(self) weakSelf = self;
dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
    [weakSelf doSomething];
    [weakSelf doOtherThing];
});

{% endhighlight %}


在 doSomething 中，weakSelf 不会变成 nil，不过在 doSomething 执行完成，调用第二个方法 doOtherThing 的时候，weakSelf 有可能被释放，于是，strongSelf 就派上用场了：


{% highlight bash %}

__weak __typeof__(self) weakSelf = self;
dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
    __strong __typeof(self) strongSelf = weakSelf;
    [strongSelf doSomething];
    [strongSelf doOtherThing];
});

{% endhighlight %}


# 总结


- 在 Block 内如果需要访问 self 的方法、变量，建议使用 weakSelf。
- 如果在 Block 内需要多次 访问 self，则需要使用 strongSelf。


# 在UIImageView上平铺显示图片

## 代码示例

{% highlight bash %}
UIColor *color = [UIColor colorWithPatternImage:[UIImage imageNamed:@"water_mark"]];
UIImageView imageView = [[UIImageView alloc] init];
imageView.backgroundColor = color;
{% endhighlight %}


# 在移动APP中像使用字体一样使用图标



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

