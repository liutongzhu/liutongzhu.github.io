---
author: 飞雪无情
comments: true
date: 2014-03-22 13:29:02+00:00
layout: post
slug: android-avd-not-launcher
title: android新建大分辨率模拟器不能启动的问题
wordpress_id: 230
categories:
- Android
tags:
- Android
---
新建的Android模拟器的分辨率超过一定大小的时候,就无法启动,比如超过1920*1080,就启动不了啦,更别说现在的2560*1440了.这时候只需要在新建AVD的时候,选择Use Host GPU选项然后即可正常启动..已经新建好的AVD也可以点击编辑,然后勾选Use Host GPU即可
