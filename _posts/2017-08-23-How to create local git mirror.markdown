---
layout: post
title: "如何创建本地Git镜像仓库"
date: 2017-08-23 15:02:19
author: "骄傲的猫"
catalog: true
tags:
    - Git
---


1、下载代码
git clone --mirror source_repo_url

2、修改Remote
git remote set-url origin local_repo_url

3、推送到Git本地服务器
git push origin
