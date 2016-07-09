---
author: 飞雪无情
comments: true
date: 2011-11-15 01:41:00+00:00
layout: post
slug: download-android-4-0-sources
title: 最新最全的Android4.0 API源代码下载和完整Android4.0源代码下载教程
wordpress_id: 46
categories:
- Android
tags:
- Android
- Ubuntu
---

这时刚刚整理好的最新的，包含所有的API的源代码，第一次上传的那个Android4.0 API 源代码有部分API没有包含，请点击下面的链接下载最新的。

附Android4.0 API 源代码下载[http://download.csdn.net/detail/michael__li/3802305](http://download.csdn.net/detail/michael__li/3802305)

昨日凌晨Android放出4.0源代码，已经托管到GoogleSource上，大家可以下载了，这次放出的branch为Android-4.0.0_r1。

下载教程。操作系统环境为Linux,并且确保你已经安装了git jdk python等.

1.创建bin目录，并加入到PATH

    
    $ mkdir ~/bin
    $ PATH=~/bin:$PATH


2.下载repo到刚刚创建的bin目录，并设置权限为可执行。

    
    $ curl https://dl-ssl.google.com/dl/googlesource/git-repo/repo > ~/bin/repo
    $ chmod a+x ~/bin/repo


3.创建Android源代码存放目录并进入到该目录

    
    $ mkdirAndroid-4.0.0_r1
    $ cdAndroid-4.0.0_r1


4.初始化Android4.0.1的Git库

    
    $ repo init -u https://android.googlesource.com/platform/manifest -b android-4.0.1_r1


5.最后执行开始同步下载

    
    $ repo sync


然后就是漫长的等待了。。
