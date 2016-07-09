---
author: 飞雪无情
comments: true
date: 2013-11-07 14:27:21+00:00
layout: post
slug: unsupportedoperationexception-gles20canvas-clippath
title: java.lang.UnsupportedOperationException android.view.GLES20Canvas.clipPath
wordpress_id: 194
categories:
- Android
tags:
- Android
- API
- Widget
---

今天自定义控件使用Canvas绘图的时候遇到的这个错误，看错误的描述，是不支持这个clipPath方法的。而且在2.3的机器上没有问题，是一部4.0的机器报了这个问题。查看这个Canvas的子类实现GLES20Canvas，发现这是在开启硬件加速时使用的。看来还是和硬件加速有关。打开手机的开发者选项，发现硬件加速没有被开启，应用里也没有开启，有点奇怪了，可能是手机优化自动开启了硬件加速。

硬件加速这个特性是从API 11加入的，可以大大加快View的绘制，是个很不错的功能，所以我们不可能在我们的程序里直接关闭这个功能这样我们的应用就使用不了这个特性了，所以在Application标签里设置android:hardwareAccelerated="false'不可行。所以可以在用到这个自定义View里的Activity里使用android:hardwareAccelerated=“false”来配置，这样其他的Activity可以继续使用同步加速的特性。不过你的自定义View用到的地方比较多，一个个Activity配置也不太方便，所以最好的办法是在你的自定义View里禁用View级别的硬件加速。可以通过以下代码来实现

    
    if(Build.VERSION.SDK_INT>=Build.VERSION_CODES.HONEYCOMB){//API 11及其之上
        setLayerType(LAYER_TYPE_SOFTWARE,null);//禁用硬件加速
    }



