---
layout: post
title: "UISearchBar 光标不显示的问题"
date: 2017-05-05 15:38:19
author: "cathor"
catalog: true
tags:
    - ios
---


## 正文

今天发现做的App中的UISearchBar中的光标不显示，网上找了一下解决办法如下：

``` ObjC
	[searchBar setTintColor:[UIColor blueColor]];
```

或者不设置tintColor就可以了

