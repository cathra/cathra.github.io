---
layout: post
title: "ios系统实现点击通知栏弹出窗体"
date: 2016-03-17 10:38:28
author: "cathra"
catalog: true
tags:
    - ios
---

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


