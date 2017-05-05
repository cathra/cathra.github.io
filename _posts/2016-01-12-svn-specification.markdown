---
layout: post
title: "SVN代码版本管理规范"
date: 2016-01-12 14:04:02
author: "cathra"
catalog: true
tags: 
    - 规范
---


目录介绍
===
***

* trunk:

>主开发目录，开发时版本存放的目录，即在开发阶段的代码都提交到该目录上。

* branches:

>分支开发目录，是用来做并行开发的，这里的并行是指和trunk进行比较。 比如，3.0开发完成，这个时候要做一个tag，tag_release_3_0，然后基于这个tag做release，比如安装程序等。trunk进入3.1的开发，但是3.0发现了bug，那么就需要基于tag_release_3_0做一个branch，branch_bugfix_3_0，基于这个branch进行bugfix，等到bugfix结束，做一个tag，tag_release_3_0_1，然后，根据需要决定 branch_bugfix_3_0是否并入trunk。 


* tag:

>存档目录（不允许修改）,是用来做一个milestone的，不管是不是release，都是一个可用的版本。这里，应该是只读的。更多的是一个显示用的，给人一个可读（readable）的标记。


<!-- more -->


使用方法
===
***

一般的，我们的所有的开发都是基于trunk进行开发，当一个版本release开发告一段落（开发、测试、文档、制作安装程序、打包等）结束后，代码处于冻结状态（人为规定，可以通过hook来进行管理）。此时应该基于当前冻结的代码库，打tag。当下一个版本阶段的开发任务开始，继续在trunk进行开发。

此时，如果发现了上一个已发行版本（Released Version）有一些bug，或者一些很急迫的功能要求，而正在开发的版本（Developing Version）无法满足时间要求，这时候就需要在上一个版本上进行修改了。应该基于发行版对应的tag，做相应的分支（branch）进行开发。

例如，刚刚发布1.0，正在开发2.0，此时要在1.0的基础上进行bug修正。
按照时间的顺序

1.0开发完毕，代码冻结 
基于已经冻结的trunk，为release1.0打tag。此时的目录结构为：

    svn://proj/
               +trunk/ (freeze)
               +branches/
               +tags/
                      +tag_release_1.0　(copy from trunk) 

2.0开始开发，trunk此时为2.0的开发版 。发现1.0有bug，需要修改，基于1.0的tag做branch，此时的目录结构为：

    svn://proj/
                +trunk/ ( dev 2.0 )
                +branches/
                         +dev_1.0_bugfix (copy from tag/release_1.0)
                +tags/
                        +release_1.0　(copy from trunk) 

在1.0 bugfix branch进行1.0 bugfix开发，在trunk进行2.0开发 
在1.0 bugfix 完成之后，基于dev_1.0_bugfix的branch做release等 
根据需要选择性的把dev_1.0_bugfix这个分支merge回trunk（什么时候进行这步操作，要根据具体情况） 
这是一种很标准的开发模式，很多的公司都是采用这种模式进行开发的。trunk永远是开发的主要目录。

