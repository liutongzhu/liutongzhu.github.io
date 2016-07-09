---
author: 飞雪无情
comments: true
date: 2013-12-14 15:35:36+00:00
layout: post
slug: open-source-project-in-android-app
title: Android产品开发中常用的一些开源项目
wordpress_id: 207
categories:
- Android
tags:
- Android
- 开源
---

你在工作中可能会遇到同时给你说不要重复发明轮子，其实这个说的就是第一：不要做重复的事情，第二：站在巨人的肩膀上。现在有了互联网以及开源的精神，我们的很多在产品中需要实现的功能组件都可以在互联网上找到，我们可以直接拿来用，这样我们就不用重复做东西了，这得益于开源奉献精神。像Google、Facebook这类的大公司都在使用很多的开源产品，比如我们常见的Apache系列、我们使用的Eclipse IDE等等。

在我自己的产品开发中，我也使用到了一些常用的开源项目，下面就介绍一下我使用的一些Android开源项目。


### 1.android-wheel


还记得iPhone上经典的日期选择滑轮组件吗?这个就是android版本的实现，记得当时这种选择方式很风靡，可以用于时间、分类等一些具有选择类型功能的开发实现，非常方便。

开源项目地址：[https://code.google.com/p/android-wheel/](https://code.google.com/p/android-wheel/)


### [![android-wheel-cities](/uploads/2013/12/android-wheel-cities.png)](/uploads/2013/12/android-wheel-cities.png)2.achartengine


这个是android上用于报表类的组件，可以实现条形图、饼图、柱状图、折线图等等。可以在产品中用于一些报表统计类的功能，通过这些图形报表给用户一个直观的统计，比如用户的收支走势图、分类支出对比图等等。

开源项目地址：[https://code.google.com/p/achartengine/](https://code.google.com/p/achartengine/)


### [![android-wheel-cities](/uploads/2013/12/android-wheel-cities.png)](/uploads/2013/12/android-wheel-cities.png) [![combined_chart](/uploads/2013/12/combined_chart-300x500.png)](/uploads/2013/12/combined_chart.png)3.Satellite Menu


还记得Path的弧形弹出式菜单吗？当时太风靡了，大家都在抄啊，一时间市场上所有的App的功能和菜单键全被这个设计代替了。不多说，给出地址。

开源项目地址：[https://github.com/siyamed/android-satellite-menu](https://github.com/siyamed/android-satellite-menu)


### [![687474703a2f2f692e696d6775722e636f6d2f3049676b6b74642e706e67](/uploads/2013/12/687474703a2f2f692e696d6775722e636f6d2f3049676b6b74642e706e67-489x500.png)](/uploads/2013/12/687474703a2f2f692e696d6775722e636f6d2f3049676b6b74642e706e67.png)4.drag-sort-listview


Android产品中ListView相信大家都用的不少，用于展示用户的数据是很不错的，不过有的时候用户需要调整一下列表项的顺序，比如用户想把好听的因为放在上面，便于选择，这时候这个组件就派上用场了，可以让用户拖动着某一项进行上下排序。

开源项目地址：[https://github.com/bauerca/drag-sort-listview](https://github.com/bauerca/drag-sort-listview)


### 5.SlidingMenu


不知道从什么时候起侧滑又成了App设计的一个风向标，一时间全是侧滑啊，就是感觉不用侧滑这个产品就不好意思发布似的。这是一个非常经典的侧滑开源项目，都多种方式、可以灵活配置和定制。

开源项目地址：[https://github.com/jfeinstein10/SlidingMenu](https://github.com/jfeinstein10/SlidingMenu)


### [![2012110712582439](/uploads/2013/12/2012110712582439.png)](/uploads/2013/12/2012110712582439.png)6.NineOldAndroids


Android3.0增加了一套属性动画的框架，让开发者能更容易的完成更复杂的动画，但是为了兼容低版本的手机，我们又不能把应用升级到Android3.0,不过幸好有NineOldAndroids，我们可以在低系统版本的手机上也可以使用属性动画

开源项目地址：[https://github.com/JakeWharton/NineOldAndroids](https://github.com/JakeWharton/NineOldAndroids)


### 7.Android-PullToRefresh


下拉列表刷新，上拉加载，下拉。。。，上下拉可以做很多事情，上下拉又成了时髦的设计。这是一个非常NB的组件，可以把任何View变成具有可上下拉功能的View，并且不会对原来的View造成污染。

开源项目地址：[https://github.com/chrisbanes/Android-PullToRefresh](https://github.com/chrisbanes/Android-PullToRefresh)


### [![header_graphic](/uploads/2013/12/header_graphic-500x254.png)](/uploads/2013/12/header_graphic.png)8.ActionBarSherlock


从 Android3.0开始增加了ActionBar，它能帮助开发者定制菜单，导航等，非常方便，但是低版本的用不了，所以ActionBarSherlock就出世了，它能让我们在低版本的手机上也可以使用ActionBar

开源项目地址：[https://github.com/JakeWharton/ActionBarSherlock](https://github.com/JakeWharton/ActionBarSherlock)

[![687474703a2f2f616374696f6e626172736865726c6f636b2e636f6d2f7374617469632f666561747572652e706e67](/uploads/2013/12/687474703a2f2f616374696f6e626172736865726c6f636b2e636f6d2f7374617469632f666561747572652e706e67-500x236.png)](/uploads/2013/12/687474703a2f2f616374696f6e626172736865726c6f636b2e636f6d2f7374617469632f666561747572652e706e67.png)





开源项目林林总总很多，今天说的都是我常用的，多去一些注入GitHub，GoogleCode等网站，可以发现很多开源的优秀项目，这些我们可以先了解收集起来，等我们产品中有类似的功能了，可以拿来使用，帮你更快的完成工作。










