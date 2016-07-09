---
author: 飞雪无情
comments: true
date: 2013-10-06 07:11:56+00:00
layout: post
slug: resolve-android-sdk-manager-can-not-update
title: 解决Android SDK Manager不能更新的问题
wordpress_id: 171
categories:
- Android
tags:
- Android
- Linux
- Ubuntu
---

今天打算更新到Android4.3,看看里面的新的API，DOC什么的，但是打开Android SDK Manager后发现不能更新，fetching更新配置文件的时候，都不能获取最新的更新信息。以前还可以以呢，看来这个十一又屏蔽了google拉。

针对这个情况，我第一反应就是使用代理，我本机做的有goagent,想直接用。点击Android SDK Manager的Tools，选中Options，配置里面的代理ip和端口，然后重新启动打开，这时候可以正常fetch了，但是在下载的提示_SSL hostname in certificate didn't match: <dl-ssl.google.com> != <www.google.com> OR <www.google.com>_，看说明提示是ssl证书的hostname不匹配，不能用，这个情况不知道是怎么回事，应该是本机里的证书匹配不上，又没有dl-ssl.google.com的证书，所以不能使用https下载。

不能使用https下载，我们还可以使用http下载。Android doc里也说了，如果使用https下载不了，可以选择使用http下载，只需要在Options里勾选Fore Https...那一个选项即可。。然后重新下载，发现还是下载不了。提示downloading interupt，下载被中断了，这个估计是google对goagent代理有些什么限制或者goagent配置的哪里不对。

最后不得已采取更改hosts的方式。打开终端，使用命令：

    
    sudo vim /etc/hosts


然后输入 密码打开，把以前hosts配置粘贴上去保存即可。

    
    203.208.46.146 www.google.com
    #以前是两个android更新下载的地址
    203.208.46.146 dl.google.com
    203.208.46.146 dl-ssl.google.com


以前配置后保存就可以正常更新下载android。其实更改hosts的方式可以解决很多被墙不能访问的网站，比如facebook等，需要的朋友可以在网站找下相关网站的hosts，然后参考以上配置记录。window系统的host在C:\WINDOWS\system32\drivers\etc目录下。
