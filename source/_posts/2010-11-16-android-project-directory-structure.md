---
author: 飞雪无情
comments: true
date: 2010-11-16 01:12:19+00:00
layout: post
slug: android-project-directory-structure
title: Android项目的目录结构
wordpress_id: 121
categories:
- Android
tags:
- Android
---

这两天家里晚上断网，不能晚上写了，请各位读官见谅。上一次我们使用ADT创建了一个Android的HelloWorld项目，并且介绍了模拟器以及模拟器的配置，这节就重点讲讲Android项目的目录结构，看看和我们常见的Java项目的目录结构有什么不同。




## 一：Android项目的目录结构





	
  1. Android项目结构概览。其实Android项目的目录结构和Java的差不多，没有太大变化，如下图：
![](http://dl.iteye.com/upload/attachment/348966/4a17123a-21bf-335f-8d0a-3e81b69531b1.png)

	
  2. 可以看到src文件夹里的是源文件，Android2.2是引用的类库，这些和java的都一样，那么下面的这个gen是什么呢？这个里面的类就是ADT自动生成的啦，一般只有一个R.java文件，是不能修改的，那么他是怎么生成的呢，看到下面的res文件夹了吗？R.java就是根据资源文件夹res
生成的,R.java里放的就是res中资源文件索引，用一个常量标记res中的一个资源文件，便于我们在代码中引用。

	
  3. 现在让我们看看res文件夹，这下面又有几种类别，其中drawable是放图片的，有drawable-hdpi,drawable-ldpi,drawable-mdpi
之分，对应的是高密度图片，低密度图片，和中等密度的图片，如果你要使用一个图片，你根据密度的不同做了三种相同的图片分别放到这三个文件夹里，那么你的应用运行在不同分辨率机器上的时候，就会根据该机器的分辨率自动选择合适的图片，怎么样智能吧！下图是密度和手机尺寸对应关系表。
![](http://dl.iteye.com/upload/attachment/348968/7e81aab5-3172-310d-a41f-8c68742225c0.png)
Layout中放的就是布局文件了，Android支持通过xml生成视图，这样视图就和逻辑控制的代码分离了便于管理。
Values中放的就是我们常用的字符串，颜色值，数组等
其实资源文件还有多种，这里生成的HelloWorld结构中没有，比如动画,菜单等。
接下来的这个AndroidManifest.xml很重要，每个Android项目都有一个，这是Android的配置文件，我们可以在这里配置应用的属性，定义
Activity，声明使用的权限等等，这里就不具体介绍，后面的章节会讲到default.properties也是一个配置文件。




## 二：Android项目文件的具体讲解





	
  1. 首先看这个HelloWorld类。

    
    public class HelloWorld extends Activity {
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.main);
        }
    }


他继承了Activity，前面讲了，一个Activity可以想象成一个手机的屏幕，用于展示一屏的内容，所以所有要展示内容的屏幕都要继承Activity才能实现，接着覆盖了onCreate()方法对该Activity进行初始化setContentView(R.layout.main);
设置了使用main.xml这个布局文件作为当前Activity的内容展示.main.xml就是放在res下，layout下面的文件xml布局文件,
我们可以直接使用R.layout.main进行直接的引用他，这也是Android亮点的地方，省得我们为了引用一个xml文件再使用File类去读取，我们要做的只是把这个xml文件的索引给Android，他会自动的帮我们找到它并使用.

	
  2. main.xml布局文件

    
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        >
    <TextView  
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content" 
        android:text="@string/hello"
        />
    </LinearLayout>


布局文件的内容不多,开始就是一个LinearLayout组件的定义，然后在这个LinearLayout放了一个用于显示文字的TextView。现在来看一下几个参数:

	
    * LinearLayout一个线性布局面板，只能垂直和水平布局，android:orientation="vertical"代表里面的子元素只能垂直排列，而使用android:orientation="horizontal"就标识里面的子元素水平排列..

	
    * android:layout_width定义当前视图占的宽度，这里是fill_parent即为充满整个屏幕。而设置成wrap_content会根据当前视图的大小只能的改变宽度

	
    * android:layout_height是定义视图的高度，这里也是填充整个屏幕。而设置成wrap_content会根据当前视图的大小只能的改变高度。

	
    * android:text是这是TextView要显示的文本，可以是字符串，也可以是一个字符串的引用，这里是一个引用，引用的是strings.xml定义好的名字为hello的字符串




	
  3. string.xml介绍。

    
    <?xml version="1.0" encoding="utf-8"?>
    <resources>
        <string name="hello">Hello World, HelloWorld!</string>
        <string name="app_name">HelloWorld</string>
    </resources>


这里我们看看就明白了，只要是定义一个个的K-V的键值对，供其他地方使用。比如上面的main中对hello的引用。这对字符的统一管理和国际化有很大的意义。

	
  4. AndroidManifest.xml的介绍

    
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.flysnow"
          android:versionCode="1"
          android:versionName="1.0">
        <application android:icon="@drawable/icon" android:label="@string/app_name">
            <activity android:name=".HelloWorld"
                      android:label="@string/app_name">
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />
                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
            </activity>
    
        </application>
        <uses-sdk android:minSdkVersion="8" />
    
    </manifest>


这是项目的重要配置文件，诸如Activity，权限，Intent等都在这里配置

	
    * Package:定义了该应用的包。

	
    * android:versioCode定义了应用的版本号

	
    * android:versionName定义了应用的版本名字

	
    * application标签定义了一个应用，一个项目最多有一个Application标签。

	
    * android:icon="@drawable/icon"定义了应用的图标引用资源文件中的icon图片

	
    * android:label="@string/app_name"定义了应用的名称

	
    * activity标签定义了一个Activity，你的每一个Activity必须在这里定义，否则不能运行.

	
    * Android:name定义了Activity的类名,这里的.HelloWorld是以上面的Package定义为基础的，也就是Package(com.flysnow)加上这个android:name(.HelloWorld)要能定位到这个Activity(com.flysnow.HelloWorld)，否则就是找不到.

	
    * android:label定义了该Activity的标题

	
    * intent-filter定义一个Intent过滤器，用于标记对应的Activity，以便Android系统能找到该Activity，定义的是隐性的Intent，主要使用两个子标签action和category来区分每个Intent.


最后的 <uses-sdk android:minSdkVersion=_"8"_ /> 就是定义应用的最低 SDK 的级别


三：小结

为了把Android项目的各个部分说清楚，啰嗦的多了一点，希望个为看官耐心看完.
