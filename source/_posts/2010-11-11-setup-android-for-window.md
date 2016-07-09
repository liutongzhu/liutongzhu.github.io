---
author: 飞雪无情
comments: true
date: 2010-11-11 06:41:25+00:00
layout: post
slug: setup-android-for-window
title: Window下搭建Android开发环境
wordpress_id: 22
categories:
- Android
tags:
- Android
---

>自2007年Google发布Android后，Android是越来越火，甚至有超越IPhone的势头。Android的推出，对于我们java开发人员无疑是一件值得庆幸的事情，因为Android的应用是由java开发的，因此，我们java开发人员入门Android比其他语言开发者有很大的优势。本系列教程将有浅到深系统的学习Android，这是我学习的过程，相信也会对你们有所帮助。该系列教程尽量保证每日一更。

### 一：准备工作

1. JDK1.6.0_13 下载地址[http://java.sun.com/products/archive/index.html](http://java.sun.com/products/archive/index.html)
2. Eclipse3.5.2 下载地址[http://www.eclipse.org/downloads/packages/release/galileo/sr2](http://www.eclipse.org/downloads/packages/release/galileo/sr2)
3. Android SDK2.2 下载地址[http://developer.android.com/index.html](http://developer.android.com/index.html)
4. ADT0.9.9 下载地址[http://developer.android.com/sdk/installing/installing-adt.html](http://developer.android.com/sdk/installing/installing-adt.html)
5. 有一定的java基础
6. 
### 二：环境配置

* JDk配置略，不会可以参考网上，很多帖子
* SDK Setup启动和设置。下载好Android SDK后，解压放到一个你常用的目录下，然后运行SDK Setup.exe将启动Android SDK and AVD Manager。这里打开后会常遇到的一个问题是“Failed to fetch URL...”，这是因为默认是使用的HTTPS的方式，我们改成HTTP方式就行了。。选择左边窗口的Settings，选中Forece Https://......即可。

![android manager https下载方法](/uploads/2013/10/7c61ddbf-2da7-3b62-ba2a-2bcc1d9d1587-300x156.png)

* SDK版本下载。点击Available Packages 选择要安装的API版本以及SDK文档，然后点击“install selected”即可安装。等待一段时间就可以下载好SDK以及相应的文档了。。

![android sdk 选择](/uploads/2013/10/7c61ddbf-2da7-3b62-ba2a-2bcc1d9d15871-300x218.png)

*  Android SDK配置。右击“我的电脑”，选择“属性”-"高级"-“环境变量”，新建系统变量,名称为“ANDROID_HOME”，值为你的Android SDK目录，我这里是“H:\Frame\android\android-sdk-windows”，然后把“%ANDROID_HOME%\tools”加入到PATH环境变量中，这时候你打开CMD输入android就可以看到Android SDK and AVD Manager了。


![配置环境变量](/uploads/2013/10/7c61ddbf-2da7-3b62-ba2a-2bcc1d9d15872-300x170.png)

* 安装和配置ADT。ADT是一个Eclipse插件们用于开发Android应用程序。打开eclipse,安装ADT，在线安装地址为https://dl-ssl.google.com/android/eclipse/ ，也可以下载ADT压缩包安装。安装成功后重启Eclipse，然后选择Preferences->android->点击Browse..选择你的Android SDK所在目录，然后ok即可。

![配置adt](/uploads/2013/10/5afc0bd8-fe88-3770-a492-aded3c56f0fa-300x61.png)

### 三：结束语


到这里我们的开发环境就配置好了，因为我们有java开发基础，其实很简单，总结起来就三个步骤：

1. 下载JDK，Eclipse，SDK，ADT
2. 配置JDK，SDK
3. 配置ADT。。
