---
layout: post
title: "如何在UIImageView上平铺显示图片"
date: 2016-11-04 09:30:10
author: "cathra"
catalog: true
tags:
    - iOS
---


## 代码示例

{% highlight bash %}
UIColor *color = [UIColor colorWithPatternImage:[UIImage imageNamed:@"water_mark"]];
UIImageView imageView = [[UIImageView alloc] init];
imageView.backgroundColor = color;
{% endhighlight %}



