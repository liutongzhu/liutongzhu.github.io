---
author: 飞雪无情
comments: true
date: 2010-11-12 09:26:54+00:00
layout: post
slug: android-setup-config-for0linux
title: Linux下搭建Android开发环境
wordpress_id: 140
categories:
- Android
tags:
- Android
---

上一次说到在window下搭建Android开发环境，总体比较简单。这一次就说说在Linux搭建Android开发环境，这里主要以很流行的Ubuntu 操作系统为例，其他的大同小异。




## 一：准备工作




        其实准备工作和上一篇的window下搭建的差不多，这里不再进行说明，请参见[Window下搭建Android开发环境](/2010/11/11/setup-android-for-window.html)
需要注意的是JDK以及Eclipse要下载Linux版本的.





## 二：JDK配置




JDK的配置网上说的很多了，但是还是有些人配置了之后不起作用，这里我再详细说一下，这里以我的JDK再/usr/locallib/目下下为例，如果你想把你的JDK也放到该目录下，那么使用 sudo cp
命令拷贝即可。






	
  1. 首先要修改权限。在shell下执行 sudo chmod +x jdk-6u20-linux-i586.bin
,修改完权限后使用ls查看会发现JDK变成绿色的拉，这就ok了，见下图：
![](http://dl.iteye.com/upload/attachment/347394/19f38961-b4b7-3727-a526-1f3aae01e107.jpg)


	
  2. 然后就可以安装了，执行sudo ./jdk-6u20-linux-i586.bin
就开始安装JDK了，遇到协议和同意协议的时候回车或者输入Y回车即可。

	
  3. 安装好后我们就可以开始配置环境变量了。执行 sudo gedit /etc/profile 打开配置文件在文件尾部加入以下文本：

    
    JAVA_HOME=/usr/local/lib/jdk1.6.0_20
    JRE_HOME=/usr/local/lib/jdk1.6.0_20/jre
    CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib
    PATH=$PATH:$JAVA_HOME/bin:/home/flysnow/bin
    export PATH JAVA_HOME  JRE_HOME CLASSPATH







保存关闭。。但是这个时候我们输入javac还是会报错的，因为这个配置必须重启才能生效。重启后输入java -version就可以看到版本信息了。




![](http://dl.iteye.com/upload/attachment/347404/f74708b5-b16b-34e9-b9b7-d818bc82bf8a.jpg)






	
  4. JDK安装的最后以下补充。有的时候我们输入java -version 显示并不是Java HotSpot（TM）Client VM，而是其他的诸如Open JDK 什么的,这是因为你机器里默认安装的有其他的JDK，那么我们怎么才能让我们自己的JDK设置成默认的呢。其实很简单，只需要执行以下命令即可。

    
    update-alternatives --install /usr/bin/java java /usr/local/lib/jdk1.6.0_20/bin/java 500
    update-alternatives --install /usr/bin/javac javac /usr/local/lib/jdk1.6.0_20/bin/javac 500


上面的命令是将我们安装的jdk加入java选单。然后接着执行

    
    update-alternatives --config java


这是为系统选择默认的JDK，也就是我们的。这时候你再执行Java -version就会看到使用的是你自己的JDK了。 注：以及均基于JDK目录在/usr/local/lib下，读官们可以根据自己的实际情况而定.




## 三：Android SDK配置




SDK 的配置和JDK大同小异，比JDK的配置更加简单。这里以SDK目录在~/Dev/Frame/Android下为例进行配置。






	
  1. 首先你已经解压了sdk 目录。然后使用
sudo gedit /etc/profile
打开配置文件，加入android后的配置文件应该是如下这样的： 

    
     JAVA_HOME=/usr/local/lib/jdk1.6.0_20
    ANDROID_HOME=/home/flysnow/Dev/Frame/Android/android-sdk
    JRE_HOME=/usr/local/lib/jdk1.6.0_20/jre
    CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib
    PATH=$PATH:$JAVA_HOME/bin:/home/flysnow/bin:$ANDROID_HOME/tools
    export PATH JAVA_HOME ANDROID_HOME JRE_HOME CLASSPATH







读官们可以参考的修改自己的。然后保存重启，sdk就配置好了。





## 四：Eclipse的安装 ADT的安装 SDK的API，DOC的下载




这些和Window下一样，可以参考
[Window下搭建Android开发环境](/2010/11/11/setup-android-for-window.html)






## 五：小结




linux下和window的配置步骤基本上一样，都是下载软件，配置环境变量等，linux下的难点还是对于shell命令的掌握以及对于环境变量的配置，说白了还是对于linux系统掌握的程度，是否能熟练使用？也就是linux的基本功。
