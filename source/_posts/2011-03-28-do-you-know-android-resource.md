---
author: 飞雪无情
comments: true
date: 2011-03-28 03:22:31+00:00
layout: post
slug: do-you-know-android-resource
title: android.resource://这个Uri你知道吗
wordpress_id: 91
categories:
- Android
tags:
- Android
---



如何访问平时APK文件Res目录下的内容呢? 如果直接访问Apk下的assets目录可以使用AssetManager类处理，而需要访问
res/raw这样的文件夹怎么办呢? 这里Android123可以告诉大家APK在安装时已经解压缩，部分资源存放在/data/data
/package_name/这里， 比如我们想访问res/raw/android123.cwj文件，可以使用android.resource:
//package_name/" + R.raw.android123 这个Uri，其中package_name是你工程的包名。
完整的处理代码为 Uri uri = Uri.parse("android.resource://com.android123.Sample/raw/android123.cwj"); 即可使用工程res/raw目录下的文件了。

转自：http://www.android123.com.cn/androidkaifa/827.html
