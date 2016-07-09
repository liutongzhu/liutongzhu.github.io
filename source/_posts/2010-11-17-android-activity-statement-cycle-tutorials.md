---
author: 飞雪无情
comments: true
date: 2010-11-17 01:02:20+00:00
layout: post
layout: post
slug: android-activity-statement-cycle-tutorials
title: Android Activity的生命周期
wordpress_id: 120
categories:
- Android
tags:
- Android
---

通过上一节“Android系列之四：Android项目的目录结构”我们已经知道了什么是Activity，那么为什么我们创建一个Activity的导出类的时候为什么都要覆盖Activity的onCreate方法呢，为什么会在onPause()方法中保存一些当前Activity中的变化，要弄明白这些就要先了解Activity的生命周期，也就是一个Activity才开始到结束都要经过那些状态，下面通过一个例子了解Activity的
声明周期.





## 一：Activity的生命周期方法


Android提供了很多Activity的生命周期方法，比如我们常用的onCreate、onPause、onResume等。这里主要介绍粗粒度的周期方法，诸如onPostCreate、onPostResume等这些细粒度的周期方法可以参考Android的API文档，在你需要更细层次的控制的时候可以使用这些细粒度的方法。粗粒度的周期方法有以下几个：onCreate()、onStart()、onResume()、onPause()、onStop()、onDestroy()，从名字上就可以看出来这些方法在什么时候执行。


## 二：测试Activity的生命周期方法的执行顺序


为了能更明白上这几个周期放的执行顺序，我们新建一个HelloWorld项目，在Activity中覆盖这几个方法，打印出日志就可以看出来执行顺序了



	
  1. 新建HelloWorld项目，详细步骤可以参见：
[Android第一个Android应用，HelloWorld](/2010/11/13/android-helloworld.html)

	
  2. 修改main.xml内容为：

    
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        >
    <TextView  
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content" 
        android:text="第一个Activity"
        />
    <Button 
    	android:id="@+id/second"
    	android:layout_width="wrap_content"
    	android:layout_height="wrap_content"
    	android:text="打开第二个Activity"/>
    </LinearLayout>


这里主要是为增加一个文本显示和一个按钮用于显示信息和操作。

	
  3. 新建布局文件second.xml，内容如下：

    
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout
      xmlns:android="http://schemas.android.com/apk/res/android"
      android:orientation="vertical"
      android:layout_width="fill_parent"
      android:layout_height="fill_parent">
      <TextView  
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content" 
        android:text="第二个Activity"
        />
    <Button 
    	android:id="@+id/exit"
    	android:layout_width="wrap_content"
    	android:layout_height="wrap_content"
    	android:text="退出"/>
    </LinearLayout>


这里主要是为增加一个文本显示和一个退出按钮用于退出当前Activity。

	
  4. 新建一个Activity，名字为SecondActivity，内容如下:

    
    public class SecondActivity extends Activity {
    	private final static String TAG="SecondActivity";
    	@Override
    	protected void onCreate(Bundle savedInstanceState) {
    		super.onCreate(savedInstanceState);
    		Log.v(TAG, "onCreate");
    		setContentView(R.layout.second);
    		//退出按钮
    		Button btnExit=(Button)findViewById(R.id.exit);
    		//为退出按钮设置单击事件
    		btnExit.setOnClickListener(new OnClickListener() {
    			@Override
    			public void onClick(View v) {
    				finish();//关闭当前Activity，也就是退出
    			}
    		});
    	}
    	@Override
    	protected void onStart() {
    		super.onStart();
    		Log.v(TAG, "onStart");
    	}
    	@Override
    	protected void onResume() {
    		super.onResume();
    		Log.v(TAG, "onResume");
    	}
    	@Override
    	protected void onPause() {
    		super.onPause();
    		Log.v(TAG, "onPause");
    	}
    	@Override
    	protected void onStop() {
    		super.onStop();
    		Log.v(TAG, "onStop");
    	}
    	@Override
    	protected void onDestroy() {
    		super.onDestroy();
    		Log.v(TAG, "onDestroy");
    	}
    
    }


我在各个周期方法了都加了日志信息，便于跟踪Activity的运行过程

	
  5. 修改HelloWorld类，内容如下：

    
    public class HelloWorld extends Activity {
    	private final static String TAG="HelloWorld";
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            Log.v(TAG, "onCreate");
            setContentView(R.layout.main);
            //打开第二个Activity的按钮
            Button btnSecond=(Button)findViewById(R.id.second);
            //设置单击事件
            btnSecond.setOnClickListener(new OnClickListener() {
    			@Override
    			public void onClick(View v) {
    				startActivity(new Intent(HelloWorld.this,SecondActivity.class));
    				finish();//关闭当前Activity
    			}
    		});
        }
    	@Override
    	protected void onStart() {
    		super.onStart();
    		Log.v(TAG, "onStart");
    	}
    	@Override
    	protected void onResume() {
    		super.onResume();
    		Log.v(TAG, "onResume");
    	}
    	@Override
    	protected void onPause() {
    		super.onPause();
    		Log.v(TAG, "onPause");
    	}
    	@Override
    	protected void onStop() {
    		super.onStop();
    		Log.v(TAG, "onStop");
    	}
    	@Override
    	protected void onDestroy() {
    		super.onDestroy();
    		Log.v(TAG, "onDestroy");
    	}
    }




	
  6. 运行程序，分析结果，发现当程序启动的时候，日志信息为下图:![](http://dl.iteye.com/upload/attachment/349655/baddee2d-f46b-347d-b1f4-10b902def717.png)
由此可见当打开一个Activity的时候，其周期方法执行顺序为：onCreate()->onStart()->onResume(),现在点击“打开第二个Activity”按钮，看日志的输出如下图:

![](http://dl.iteye.com/upload/attachment/349660/2fcfc919-cae3-36a5-9536-0bae63cc0636.png)
当应用从Helloworld这个Activity启动SecondActivity的时候，Android会先执行HelloWorld的onPause方法，然后依次执行SecondActivity的onCreate()->onStart()->onResume()方法
当SecondActivity呈现到屏幕上的时候再一次执行Helloworld的onStop()->onDestroy(),把HelloWorld从Activity栈中移除销毁。这里值得提的就是HelloWorld 中finish方法，因为执行了他所以
HelloWorld才会从Activity栈中移除销毁，这样当你按“返回”键返回的时候就回不到HelloWorld 这个Activity的界面了，而是直接回到的Android的应用程序列表
。




## 三：分析结果


根据上面例子可见一个Activity在启动的时候会执行onCreate()->onStart()->onResume()，在结束（或离开）的时候会执行onPause()->onStop()->onDestroy(),这就是一个Activity的生命周期。
因此我们要在onCreate方法里把Activity的需要的东西准备好，也就是初始化；在onResume里对Activity里的东西做一些调整；在onPause做一些清理和保存工作(保存持久状态),因为这是最后的
机会，因为onPause完成之前Android不会结束托管Activity类的进程，而之后进程可能被结束。总结一下这几个周期方法的作用：



	
  1. onCreate():创建Activity调用，用于Activity的初始化，还有个Bundle类型的参数，可以访问以前存储的状态。

	
  2. onStart():Activity在屏幕上对用户可见时调用

	
  3. onResume():Activity开始和用户交互的时候调用，这时该Activity是在Activity栈的顶部。

	
  4. onPause():Activity被暂停时调用，也就是你要对你能看到的这个Activity说byebye的时候调用,这里可以做一些清理和保存工作

	
  5. onStop():Activity被停止或者Activity变成不可见时调用

	
  6. onDestroy():Activity被从内存中移除，一般发生在执行finish方法时或者Android回收内存的时候


好了，最后让我们看一个API提供的Activity的状态图吧，看了他相信你对Activity的生命周期会更了解，如下图：

![](http://dl.iteye.com/upload/attachment/349672/df78b0d1-8998-38ce-a5b7-95632d1192ac.png)


## 四：小结


这节主要是通过一个例子分析Activity声明周期，并对常用生命周期方法做了一些说明，应该什么时候使用他们。到这里Android的很基础的东西已经说完了，
下面就直接Android的UI组件介绍了。。

下期预告：TextView的介绍--包含跑马灯效果
