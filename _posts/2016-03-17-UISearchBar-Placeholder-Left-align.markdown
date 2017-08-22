---
layout: post
title: "iOS开发小技巧（二） -- 实现UISearchBar的Placeholder居左显示"
date: 2016-03-17 11:47:15
author: "cathra"
catalog: true
tags:
   - iOS
   - iOS开发小技巧
---

从ios7开始，ios系统UISearchBar组件显示Placeholder图标提示信息和放大镜都是居中的，而且没有相应的方法、属性对placeholder进行操作。本文提供一个UISearchBar的扩展，使placeholder的内容居左



<!-- more -->


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
