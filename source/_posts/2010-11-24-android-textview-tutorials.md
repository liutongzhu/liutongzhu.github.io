---
author: 飞雪无情
comments: true
date: 2010-11-24 16:19:03+00:00
layout: post
slug: android-textview-tutorials
title: Android TextView小组件的使用--附带超链接和跑马灯效果
wordpress_id: 119
categories:
- Android
tags:
- Android
- Widget
---




这节主要介绍一下TextView的简单用法，以及超链接文本、跑马灯等实例。




##  一：新建HelloTextView 工程


新建一个Hello world详细步骤可以参见


### [Android第一个Android应用，HelloWorld](/2010/11/13/android-helloworld.html)


创建设置如下：



	
  1. Project name:HelloTextView

	
  2. Build Target :android 2.2

	
  3. Application name:HelloTextView

	
  4. Package name:com.flysnow

	
  5. create Activity:HelloTextView

	
  6. min SDK 8


然后运行该应用就可以看到TextView的效果，是显示一行字：“Hello World, HelloTextView!”，这是因为新建的Hello项目自带的一个TextView。


## 二：分析TextView组件


TextView是Android中常用的组件之一，可以用他来显示文字，就像一个标签一样，或者你可以认为是html中的span。对于TextView我们最关心的应该是怎么设置显示的文本，怎样设置字体的大小，字体的颜色，字体的样式，

其实很简单，TextView中提供了大量的属性帮我们配置TextView。



	
  1. 修改xml配置文件实现。我们修改main.xml如下:

    
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        >
    <TextView  
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content"
        android:textColor="#ff0000"
        android:textSize="24sp"
        android:textStyle="bold"
        android:text="@string/hello"
        />
    </LinearLayout>


这里增加了三个属性的设置，分别是android:textColor="#ff0000"设置字体为红色，android:textSize="24sp"设置字体为24sp, android:textStyle="bold"设置字体加粗，预览效果如下图：
![](http://dl.iteye.com/upload/attachment/353237/12c1bbb8-419b-3dbd-b53a-501f73088c50.png)看到我们的TextView的内容已经变成红色，24sp大，加粗。。

	
  2. 2.修改java代码实现。同样我们不修改xml文件，而是通过java编码来实现上面的图示效果，首先我们先给这个TextView分配一个id，也就是这个TextView的标记记号，方便我们找到他。在main.xml的TextView中加入android:id="@+id/text_view"就可以为该TextView分配一个id。。这里@+id/是表示在R类的id类下新增常量字段，这里的常量字段是text_view。下面修改HelloTextView类如下：

    
    package com.flysnow;
    
    import android.app.Activity;
    import android.graphics.Color;
    import android.graphics.Typeface;
    import android.os.Bundle;
    import android.util.TypedValue;
    import android.widget.TextView;
    
    public class HelloTextView extends Activity {
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.main);//设置内容显示的xml布局文件
            TextView textView=(TextView)findViewById(R.id.text_view);//取得我们的TextView组件
            textView.setTextColor(Color.RED);//设置成红色
            textView.setTextSize(TypedValue.COMPLEX_UNIT_SP, 24f);//设置成24sp
            textView.setTypeface(Typeface.defaultFromStyle(Typeface.BOLD));//加粗
        }
    }


最终结果和上图一样的，这说明通过代码和xml配置都可以定制TextView，但是推荐使用xml进行定制，使用java代码控制逻辑，这符合mvc模式，也符合Android的设计思想。




这里说一下度量单位。度量单位有很多，如px,pt,dip,sp等等。不过建议应该使用sp作为字体大小的单位，使用dip作为其他元素的单位。。因为sp是刻度无关的像素，更重要的是他可以根据用户的字体大小的首选项进行缩放，这才是重要的，这样当你调整了整体的字体大小时不至于使得个别字体的大小不一致而影响美观。




##  三：TextView的一些有用的实例





	
  1. TextView的超链接形势。我们应该都见过html中的超链接，加一个a标记就可以让一段文字变成超链接的形式，可以点击到连接的地址。那么TextView可以实现吗？作为强大的TextView当然不会忘记这一点。TextView为我们提供了android:autoLink属性，只要把他设置成“web”,那么该TextView中的是网址形势的文件就会自动变成超链接的形式。好了，耳听为虚，眼见为实，看下面的例子。修改strings.xml为：

    
    <?xml version="1.0" encoding="utf-8"?>
    <resources>
        <string name="hello">我的博客地址是：http://www.flysnow.org    \n我的电话是：400-34534-500\n我的email是12235@163.com</string>
        <string name="app_name">HelloTextView</string>
    </resources>


修改main.xml为：

    
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        >
    <TextView 
    	android:id="@+id/text_view"
    	android:autoLink="web"
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content"
        android:text="@string/hello"
        />
    </LinearLayout>


然后把HelloTextView类中的那一段设置文本颜色、大小和样式的代码注释掉，运行程序就会看到如下图的效果:
![](http://dl.iteye.com/upload/attachment/353258/c4c40145-5e31-3541-8fbd-bfb734a8b3cd.png)
当我们点击蓝色的我的博客的网址的时候，Android系统就会调用默认的web浏览器打开我的博客。
有的朋友已经注意到了，文本里我还写了我的电话和email，难道TextView也支持电话和email超链接吗？没错，的确支持，当我们设置android:autoLink="phone"的时候，文本里的电话就会变成蓝色超链接形式，点击就会打开拨号界面等待你按通话键拨号，email也是同理。。

当我们把 android:autoLink换成phone的时候发现网址不超连接了，换成email也是一样。难道我们不能一下子让网址，电话，email都超链接吗？答案是肯定的，这时候我们可以把 android:autoLink设置成all，这样里面的网址、电话和email就都可以超链接了。
![](http://dl.iteye.com/upload/attachment/353264/3de8c6b3-263b-3c8b-86b8-951de945bce6.png)

	
  2. 跑马丁效果。有时候我们要显示的文本较长，TextView不能完全显示，这时候可以通过这中跑马灯的方式让文本移动展示，达到了既不占用地方又能完全看到文本的目的。这里直接复用农民伯伯的跑马灯代码：

    
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        >
    <TextView 
    	android:id="@+id/text_view"
    	android:autoLink="all"
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content"
        android:text="@string/hello"
        android:ellipsize="marquee" 
        android:focusable="true" 
        android:marqueeRepeatLimit="marquee_forever" 
        android:focusableInTouchMode="true" 
        android:scrollHorizontally="true"/>
    </LinearLayout>


然后为TextView设置一个很长的字符串。运行就可以看到横向移动的效果。

![](http://dl.iteye.com/upload/attachment/353267/fab3be3a-0850-32d1-8b74-893ac8dcb816.png)
设置的属性说明可以参考TextView的文档，这里也有中文版本：[TextView中文API文档](http://www.cnblogs.com/over140/archive/2010/08/27/1809745.html)

这里要进行说明的是：以上设置在大部分情况下都会成功的展示跑马灯样式，但是在一些复杂的布局中就会看不到任何文字。比如我开发的Android应用“我团”，在展示团购详细信息页面，我自定义了一个标题栏让其显示团购的信息，想让其跑马灯的方式显示，但是使用了上述代码后看不到文字，其实是文字被撑下来的，这时候我们设置android:singleLine="true"以单行的方式展示就好了。所以请以后实现跑马灯效果的时候最好加上android:singleLine="true"单行展示。。




## 四：小结


好了，一个TextView也啰嗦了那么多，希望大家能听明白，最后两个实用的例子能帮助大家更好的学习和使用TextView。。这两天JE挂了，没来得及更新，对不起大家了。最近忙于升级“我团”应用，也只能网上10点后才能上网写。可能更新会很慢，请见谅，谢谢各位的支持。





