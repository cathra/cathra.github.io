---
layout: post
title: "使用SSH密钥连接Github"
date: 2016-03-17 11:27:24
author: "比特怪兽"
catalog: true
tags:
    - github
---

对于使用github的开发人员来说，经常的push代码操作是不可避免的，而每次pus都需要输入用户名和密码，异常麻烦，也不利于对github进行自动化处理。本文主要介绍如何通过SSL密钥的方式操作github，避免频繁的输入用户名密码。

<!-- more -->

一、创建新密钥
===

输入命令：

``` bash
$ssh-keygen -t rsa -C "your_email@example.com"
#这将按照你提供的邮箱地址，创建一对密钥
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/you/.ssh/id_rsa): [Press enter]
```

直接回车，则将密钥将按默认文件进行存储。此时也可以输入特定的文件名，比如github_rsa

接着，根据提示，你需要输入密码和确认密码

``` bash
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
```

输入完成之后，屏幕会显示如下信息：

``` bash
Your identification has been saved in /c/Users/you/.ssh/github_rsa.
Your public key has been saved in /c/Users/you/.ssh/github_rsa.pub.
The key fingerprint is:
01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db your_email@example.com
```

二、在Github账户中添加你的公钥
===


运行如下命令，将公钥的内容复制到系统粘贴板(clipboard)中。

``` bash
clip < ~/.ssh/github_rsa.pub
```

接着：

1. 登陆GitHub,进入你的Account Settings.
2. 在左边菜单，点击”SSH Keys”.
3. 点击”Add SSH key”按钮.
4. 粘贴你的密钥到key输入框中.
5. 点击”Add Key”按钮。
6. 到此，大功告成了！

三、测试
===

为了确认我们可以通过SSH连接GitHub，我们输入下面命令。输入后，会要求我们提供验证密码，输入之前创建的密码就ok了。

``` bash
$ssh -T git@github.com
```

如果在创建新密钥是指定了密钥文件名，则输入

``` bash
$ssh -T -i github_rsa git@github.com
```

如果看到下面信息，就说明一切完美！

``` bash
Hi username! You’ve successfully authenticated, but GitHub does not provide shell access.
```

四、配置.ssh/config指定github使用的密钥
===

如果在第一步创建密钥时，指定了自定义的密钥文件名，可以通过配置.ssh/config文件指定github使用的密钥

``` bash
Host me.github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/github_rsa
```




