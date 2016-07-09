---
author: 飞雪无情
comments: true
date: 2014-02-20 14:14:29+00:00
layout: post
slug: android-overlay-guide
title: android覆盖式引导
wordpress_id: 225
categories:
- Android
tags:
- Android
---

我们在开发产品的时候,每次发版可能会有一些新的功能需要引导用户使用,以前大部分都是使用截图,然后让设计师把引导的问题修饰好放到图上,现在这张图片就是引导图片,我们只需要用这张图片放到界面上,这样基本上就可以满足引导的作用.

但是这样的情况有个缺点,那就是,手机的分辨率太多,我们不可能每个分辨率都做一张图片,这样图片就会变形,有些引导操作的位置可能就不准确了,而且使用绘图的方式绘制在图片上的文字也会模糊不清.

基于如上的方式,我们可以使用另外一种方式来做引导,这样方式就是使用覆盖式的方式,主要的实现方式是



	
  1. 使用一个透明并且全屏的PopuWindow,这样我们就有一个可以显示向导

	
  2. 获取需要引导的View元素在屏幕上的位置,这样我们就可以准确的给这个View绘制引导信息

	
  3. 然后让PopuWindow显示出来,这样就可以看到我们的引导正好指向需要引导的位置


可以看下如下图的效果,这是实现的示例图.

[![](/uploads/2014/02/device-2014-02-20-213148-500x209.png)](/uploads/2014/02/device-2014-02-20-213148.png)



可以看到,"查看帮助,了解更多"正好指向我们?图片操作按钮,位置很准确,文字也很清晰,并且这种方式实现的在所有分辨率上的手机上位置都是准确的,因为我们准确的获得了目标View的位置.

从上面的描述和效果图中我们也看到了一些关键点,比如全屏背景透明的PopuWindow,这个相信比较简单,宽高FILL_PARENT,背景设置为透明背景就好了.第二个就是目标View的位置,这个是比较难的,一般的引导我们都会在用户打开界面的时候显示,按说在Activity onCreate方法里调用最合适,但是这个时候我们的ContentVIew还没有测量绘制,所以我们根本不知道目标的位置.另外一个办法就是监听View层级的变化.

    
    ViewTreeObserver observer view.getViewTreeObserver();
    observer.addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener(){
        public void onGlobalLayout(){
        //开始显示向导
      }
    });


注册监听后,就可以在收到通知后进行显示向导,这时候每个View都已经测量布局好,可以获得他们的准确位置,获取View在屏幕上的位置通过public void getLocationOnScreen (int[] location)获取其在屏幕上的位置.

获取到这些VIew的位置后,就需要显示我们的引导了,PopuWindow可以设置一个ContentView,我们可以AbsoluteLayout作为ContentView,在这里绝对布局是最好的选择,因为我们获取的目标View的位置都是绝对的,所以使用绝对位置,通过x,y坐标就可以确定引导的位置.

接下来的事情就简单了,我们只需要显示PopuWindow就可以啦,然后控制好事件,比如用户点击的时候收起该引导,让用户继续正常使用产品.引导不能无休止的显示,我们可以通过配置来控制引导是否显示以及显示的次数等等.
