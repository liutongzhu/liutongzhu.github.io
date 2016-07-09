---
author: 飞雪无情
comments: true
date: 2011-05-21 14:43:55+00:00
layout: post
slug: android-thread-notice-item
title: Android中关于线程使用的几点注意事项
wordpress_id: 89
categories:
- Android
tags:
- Android
---

我们都知道Android是单线程模型，这意味着Android UI操作并不是线程安全的并且这些操作必须在UI线程中执行，所以你单纯的new一个Thrad并且start()不行的，因为这违背了Android的单线程模型。

很幸运的是Android为我们提供了在其他线程中访问UI线程的方法，相信大家都见过，这几个就是Activity的runOnUiThread方法，View的post和postDelayed方法，以及最常用的Hanlder和AsyncTask，这里我推荐使用Hanlder和AsyncTask，尤其是AsyncTask，因为使用他们你会发现你的代码很容易被理解，因为他们都有一些具有特定职责的方法，尤其是AsyncTask，有预处理的方法onPreExecute，有后台执行任务的方法doInBackground，有更新进度的方法publishProgress，有返回结果的方法onPostExecute等等，这就不像post这些方法，把所有的操作都一大坨的写在一个Runnable里。

有了Android为我们提供了这些方法我们就可以很好的解决一些长时间处理的任务了，但是在使用的时候我们还必须注意以下几点：



	
  1. 这些方法或者类必须在在UI线程中创建和调用

	
  2. 其实这些方法和类最终的实现都是Android的Message、MessageQueue和Looper的机制，所以不要期待你会马上看到结果（效果），因为这是一个Loop一直循环出MessageQueue中的Message执行的过程，如果你没有看到效果，那么等等吧，因为还没有轮到你。

	
  3. 有线程（多个）的地方就会有并发，会有资源共享冲突，所以在使用的时候谨慎点吧，说不准你的一个线程中使用的变量已经被另一个线程改的面目全非了。。




后语：忙的很久没有写博客了，搭黑写了平时总结的线程的注意事项，demo就不写了，相信大家都会，此外多了解点Android的消息机制对于这个理解比较好的多。。
