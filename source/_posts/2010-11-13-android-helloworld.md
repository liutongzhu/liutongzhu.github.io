---
author: 飞雪无情
comments: true
date: 2010-11-13 14:01:04+00:00
layout: post
slug: android-helloworld
title: 第一个Android应用，HelloWorld
wordpress_id: 139
categories:
- Android
tags:
- Android
---

这节主要内容是使用ADT创建一个Helloword实例，并进行概要分析，中间会有AVD的创建。




## 一：使用ADT创建Helloword应用




使用ADT我们能很方便的创建一个Android的Helloworld应用，关于ADT的配置请看[
](/2010/11/11/setup-android-for-window.html)




[Window下搭建开发平台](/2010/11/11/setup-android-for-window.html)




这里只讲ADT创建Android应用，关于CMD下的ant方式请看Android开发手册。






	
  1. 打开Eclipse工具，新建一个项目->Others->Android->Android Project然后点击Next进入下一步.
![](http://dl.iteye.com/upload/attachment/347774/45909298-5921-3eb6-a1dd-71012dd3408b.png)



	
  2. 在Project name中输入Helloword，Build Target的列表里选择Android2.2，在Application name输入应用的名字Helloworld ，然后在Package name输入你的应用的包名com.flysnow.helloworld,Create Activity里输入HelloWorld，min SDK输入8.这里我们不创建测试工程，点击finsh就完成了项目的创建。
![](http://dl.iteye.com/upload/attachment/347776/e4946ab1-4525-37fe-b86a-92e9fdb96ac8.png)

![](http://dl.iteye.com/upload/attachment/347778/d7b19fac-77e5-37d4-8774-709cdbde2f6d.png)

	
  3. ![](http://dl.iteye.com/upload/attachment/347780/703b6595-7366-35a5-89ac-46253c495361.png)
Activity是Android的主要元素，你可以把Activity理解为一个手机屏幕界面，一个Android应用是有多个Activity组成的，也就是多个界面。min SDK Version就是你的应用最低运行在什么API等级的手机上。Android的API有7个等级。下面是API等级和Android版本对应列表
![](http://dl.iteye.com/upload/attachment/347782/dcac6a13-c363-3aef-b729-01a03c1b256f.png)




## 二：创建AVD




一个Android要想运行起来必须要有一个环境，这个环境就是模拟器。使用 AVD manager可以轻松的创建一个AVD（配置好的模拟器）。






	
  1. 单击Eclipse工具栏上的AVD Manager图标打开AVD Manager


![](http://dl.iteye.com/upload/attachment/347786/22416f00-c704-34e5-ace0-98a5402f381b.png)



	
  2. 点击右侧的“New”按钮打开创建AVD界面。在Name输入一个AVD的名字，这里使用Android2.2以注明是使用的是2.2版本的SDK，好的命名容易区分。Target出选择Android 2.2-API 8接着创建一个100M的SD卡，用于以后装一些注入音乐视频之类的文件。，最后就是在Skin选择皮肤了，就是你打算让你的模拟器长社么样，这里使用默认的就好，然后点击Create AVD就创建了一个AVD，这会有一段时间（特别是SD卡大的时候），之间最好别有任何操作，等会就好。
![](http://dl.iteye.com/upload/attachment/347793/0c9c139a-29fe-37dd-ac98-d1d57142489b.png)




## 三：运行Helloword应用




有个AVD我们就可以运行Android应用了。找到我们的Helloword的项目，右击选择“Run”，选择Android Application 就可以运行Helloword应用了，这个过程有点长，稍等一下就可以看到模拟器的界面了。。好了让我们看看多么漂亮。
![](http://dl.iteye.com/upload/attachment/347795/1a3fcead-cd37-3058-89b0-4204de13f585.png)




有的读官说了怎么还看不到我们自己的Helloword应用运行的效果啊，这是应为我们的模拟器还处于锁定状态，我们都知道手机都可以加键盘锁的，有的是按通话键打开，有的是按星号打开，但是我们的Android模拟器是滑动打开的，让我们用鼠标左键按住屏幕上的“锁”图标，然后向右滑动，然后松开鼠标，ok，打开了，很绚丽吧，等一会就看到我们的Helloword应用的效果了。
![](http://dl.iteye.com/upload/attachment/347799/291df35a-46b6-3909-ab4b-c0a2f1bbe353.png)





## 四：Android 模拟器的一些设置和小技巧





	
  1. 更改模拟器语言为中文。模拟器默认是英文的，我们需要设置成中文，这样看着才方便。我们可以通过Android的语言设置达到这个目的。在模拟器的菜单中找到Setting，然后向下滚动找到Language&keyboard单击。
![](http://dl.iteye.com/upload/attachment/347801/15c5b93e-d8ca-3df3-9040-ae41af1c3a47.png)
然后在打开的界面的最上面的找到Select Language单击，然后打开一个语言选择列表，找到中文(简体)选择就ok了。。你的模拟器就变成中文的啦。。
![](http://dl.iteye.com/upload/attachment/347803/a30de346-b74a-3b45-92e7-365039647e74.png)


	
  2. 有人说我设置成中文后，在输入文字的地方怎么不能输入中文啊。其实这是因为模拟器默认的输入法是Android 键盘。模拟器自带的有谷歌输入法，我们只要选择谷歌输入法就可以输入中文了。在你输入文字的文本框处长按鼠标，会弹出一个“编辑文字”的弹出框，
![](http://dl.iteye.com/upload/attachment/347805/0e7db327-c2c0-3cee-87b6-b902b18add26.png)
选择输入法，就可以看到输入法列表了，有“Android 键盘，谷歌输入法”，还有一个日本的输入法。
![](http://dl.iteye.com/upload/attachment/347808/6bbe06c4-6b5d-34df-a1a6-7cf69b6a5d57.png)
我们选择“谷歌输入法”即可。。这时候可以输入中文啦。。

	
  3. 由于模拟器每次启动都比较慢，我们可以在打开模拟器之后不要再关闭，这样每次运行Android应用 ADT就会自动使用这个模拟器，这样就不用每次都启动了。。

	
  4. Min SDK Version和模拟器。还记得我们创建Helloworld应用时最后输入的Min SDK Version吗？我们输入的是8，这个数字代表我们这个应用只能运行在Android SDk版本为2.2的机器上。这个Min SDK Version和模拟器也有关系的，如果你的机器上有多个模拟器，且每个Android SDK版本的模拟器都有，那么当你运行应用的时候，ADT就会根据Min SDK Version的值选择相应版本的模拟器。加入你的Min SDK Version是3，而且你已经创建了Android1.5的模拟器，那么你运行应用的时候，ADT就会打开Android版本的那个模拟器。。方便我们测试，智能吧。。




## 五：小结




这一节主要讲的就是怎么创建一个最简单的Android应用，并且对模拟器的创建和一些设置、常用的技巧做了一些介绍，目的就是尽量的熟悉ADT的创建和模拟器的使用。
