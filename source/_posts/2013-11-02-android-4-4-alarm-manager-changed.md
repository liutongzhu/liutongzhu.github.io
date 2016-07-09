---
author: 飞雪无情
comments: true
date: 2013-11-02 15:46:47+00:00
layout: post
slug: android-4-4-alarm-manager-changed
title: 使用AlarmManager设置的定时服务在Android4.4上可能不准确了
wordpress_id: 191
categories:
- Android
tags:
- Android
- API
---

Android4.4在前天发布了，随着Android4.4的源代码的放出，相信会有更多的手机会慢慢的升级到Android4.4,作为苦逼的Android开发人员，我们需要保证我们的应用在Android4.4的兼容性，也就是可用性。现在这里就有一个比较大的更新会影响到我们的应用功能，他就是AlarmManager。

Android宣称为了电源的使用效率，在Android4.4上位所有应用的Alarm服务使用批量启动的方式来处理相似的时间以激活设备。

以前呢，是set一个Alarm，系统在到点的时候给你单独一个去激活设备启动你设置的服务;现在呢，不是这样了，而是按批的啦，一批激活那个时间点周围设置的差不多的Tigger Time的Alarm，比如你以前设置的是18：00准备自动备份数据，现在手机正好在18：05进行了一次批量的Alarm启动操作，而你设置的Alarm正好在这个时间点周围，那么你的Alarm也被启动了，比你预想的晚了5分钟。

很多人想，这太不合理了啊，那我们根本就没有使用到Android4.4的新东西，只是用户的手机是Android4.4的系统，不能这么武断的就这么处理吧。所以Android还是给了我们兼容的方案的，只要你的App的targetSdkVersion<19,那么你以前设置的Alarm时间还是精准的，和以前一样使用。如果你想使用Android4.4及其以后版本的新功能，必须把targetSdkVersion设置成>=19,而又想要求Alarm的设置精准，那么怎么办呢，Android在4.4给我们提供了新的方法setExact来设置精准的时间。
