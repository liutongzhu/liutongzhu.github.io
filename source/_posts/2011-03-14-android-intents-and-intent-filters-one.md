---
author: 飞雪无情
comments: true
date: 2011-03-14 10:01:05+00:00
layout: post
slug: android-intents-and-intent-filters-one
title: Android Intents and Intent Filters(一)
wordpress_id: 93
categories:
- Android
tags:
- Android
- Intent
---

看过前面的几节教程的应该都会留意到一个很重要的东西，那就是Intent。Intent是什么呢？我们都知道Android有四大核心组件 Activity、Service、Broadcast Receiver和Content Provider，略去Content Provider不提，那么剩下的三个组件之间的通信考什么？这就是Intent！！它不仅可以在同一个应用中起传递信息的作用，还是可以在不同的应用进行传递信息。这就使得我们的应用和系统中的其他应用进行交互有了可能，进而使得整个Android开发变得更加精彩。想想吧，我们自己的应用可以调用系统中的通话应用进行拨号、可以调用短信应用发短信，这是多么美妙的一件事情啊！！！


##  在这里你将会学到：





	
  1. 什么是Intent和Intent Filter

	
  2. Intent和Intent Filter有什么用

	
  3. 怎么启动（传递）一个Intent

	
  4. Intent里包含什么

	
  5. 什么是显式Intent以及什么隐式Intent

	
  6. Intent是怎样匹配的




## 一：前言


在正式介绍Intent之前，我们先看下上面说的Activity、Service和Broadcast Receiver是怎样传递Intent。对于这三个组件而言，他们都有自己独立的传递Intent的机制：



	
  1. Activity:对于Activity来说，它主要是通过Context.startActivity()或Activity.startActivityForRestult()来启动一个存在的Activity做一些事情。当使用Activity.startActivityForResult()启动一个Activity时，可以使用Activity.setResult()返回一些结果信息，可以在Activity.onActivityResult()
中得到返回的结果.

	
  2. Service:对于Service来说，它主要是通过Context.startService()初始化一个Service或者传递消息给正在运行的Service。同样，也可以通过Context.bindService()建立一个调用组件和目标服务之间的连接。

	
  3. Broadcast Receiver:我们可以通过Context.sendBroadcast()Context.sendOrderedBroadcast()以及Context.sendStickyBroadcast()这些方法，传递Intent给感兴趣的广播。


消息之间的传递是没有重叠的，比如调用startActivity()传播一个Intent，只会传播给Activity，而不会传播给Service和Broadcast Receiver，反过来也是这样的。


## 二：Intent对象


一个Intent对象包含了很多数据的信息，比如要执行的动作，类别，数据，附加信息等等，下面就一一列列出一个Intent中包含的信息。


### 组件名称


这个组件名称字段其实就是一个ComponentName类，它包含了一个目标组件的全限定名，比如com.flysnow.intent.Activity1，这就是一个全限定名的Activity。组件名字可以通过setComponent()、setClass()或者setClassName()设置，如果设置了Intent目标组件的名字，那么这个Intent就会被传递给特定的组件，也就是我们说的显式Intent.如果不设置，则是隐式的Intent，Android系统将根据Intent Filter中的信息进行匹配.


### Action(动作)


一个Intent的Action在很大程度上说明这个Intent要做什么，是查看(View)、删除(Delete)、编辑(Edit)等等。Action一个字符串命名的动作，Android中预定义了很多Action，可以参考Intent类查看,下面是文档中的几个动作

![](http://dl.iteye.com/upload/attachment/435153/de61628d-7112-32f4-a6b2-902e1eb72ea1.png)
当然，我们也可以自定义Action，比如com.flysnow.intent.ACTION_ADD,定义Action的时候最好能表明意思，要做什么，这样我们的Intent中的数据才好填充。Intent对象的getAction()可以获取动作，使用setAction()可以设置动作。


### Data（数据）


Data，其实就是一个URI，用于执行一个Action时所用到的数据的URI和MIME。不同的Action有不同的数据规格，比如ACTION_EDIT动作，数据就可以能包含一个用于编辑文档的URI,如果是一个ACTION_CALL动作，那么数据就是一个包含了tel:6546541的数据字段，所以上面提到的自定义Action时要规范命名。数据的URI和类型对于Intent的匹配是很重要的，Android往往根据数据的URI和MIME找到能处理该Intent的最佳目标组件。


### Category(类别)


Category指定了用于处理Intent的组件的类型信息，一个Intent可以添加多个Category，使用addCategory()方法即可，使用removeCategory()删除一个已经添加的类别。Android的Intent类里定义了很多常用的类别，可以参考使用。


### Extras(附加信息)


有些用于处理Intent的目标组件需要一些额外的信息，那么就可以通过Intent的put..()方法把额外的信息塞入到Intent对象中，用于目标组件的使用，一个附件信息就是一个key-value的键值对..Intent有一系列的put和get方法用于处理附加信息的塞入和取出。


### Flag(标识)


Android有很多标识，用于标记如何启动一个活动，是NEW_TASK还是其他等等，以及启动后怎么对待这个活动。。可以参考Intent类中的FLAG常量字段。


## 三：Intent的解析


介绍完了Intent对象的构成，就要介绍Android系统是怎样解析Intent的，Android是怎么传递Intent到目标组件的呢？


### Intent分类


Intent可以分为两类，一类是显式的Intent，一类是隐式的Intent，上面有过提及。显示的Intent就是指定了组件名字的，隐式的就是没有指定Intent的组件名字，需要Android根据Intent中的Action、data、Category等来解析匹配。而目标组件（Activity、Service、Broadcast Receiver）怎通过设置他们的Intent Filter来界定其处理的Intent。如果一个组件没有定义Intent Filter，那么它只能接受处理显示的Intent，只有定义了Intent Filter的组件才能同时处理隐式和显示的Intent。


### Action检测


为了对Action的作用进行检测，我们使用一个例子来演示Action的作用。项目名为Intents,应用名为Intents and Filters，运行在Android2.2版本上.主启动Activity为IntentsTestList

IntentsTestList代码如下：

    
    /**
     * Intents测试列表类
     * @author 飞雪无情
     * @since 2011-3-14
     */
    public class IntentsTestList extends ListActivity {
    	private String ACTION_VIEW="com.flysnow.intent.ACTION_VIEW";
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
          //定义ListAdapter
    		setListAdapter(new SimpleAdapter(this, getData(),
    				android.R.layout.simple_list_item_1, new String[] { "title" },new int[] {android.R.id.text1}));
    		getListView().setTextFilterEnabled(true);
    
    		//注册一个广播
            super.registerReceiver(new BroadcastReceiver() {
    
    			@Override
    			public void onReceive(Context context, Intent intent) {
    				Toast.makeText(context, "该Broadcast的Intent Filter值只设置了Action", Toast.LENGTH_SHORT).show();
    			}
    		},new IntentFilter(ACTION_VIEW));
        }
    	@Override
    	protected void onListItemClick(ListView l, View v, int position, long id) {
    		Intent intent=(Intent)getData().get(position).get("intent");
    		Boolean isActivity=(Boolean)getData().get(position).get("isActivity");
    		if(isActivity){
    			startActivity(intent);
    		}else{
    			sendBroadcast(intent);
    		}
    
    	}
    
    	/**
    	 * 返回ListView需要的数据
    	 * @return ListView需要的数据
    	 */
    	private List<map<string,object>> getData() {
    		List<map<string,object>> data=new ArrayList<map<string,object>>();
    		addItem(data, "空Action的Activity", new Intent(),true);
    		addItem(data, "Action检测-Activity", new Intent(ACTION_VIEW),true);
    		addItem(data, "Action检测-Broadcast", new Intent(ACTION_VIEW),false);
    		return data;
    	}
    	/**
    	 * 给ListView添加数据
    	 * @param data 存储数据的List
    	 * @param name 要显示的Title
    	 * @param intent 单击某一项时要启动的Activity
    	 * @param isActivity 启动的是否是Activity，true是，false为广播
    	 */
    	private void addItem(List<map<string,object>> data, String name, Intent intent,boolean isActivity) {
            Map<string, object=""> temp = new HashMap<string, object="">();
            temp.put("title", name);
            temp.put("intent", intent);
            temp.put("isActivity", isActivity);
            data.add(temp);
        }
    }


这里主要是使用ListActivity列出3个测试，一个测试空的Intent,一个测试只有Action的Intent--Activity版，一个测试只有Action的Intent--Broadcast版。。。

为了测试startActivity()，我们新建一个Activity名字为ActionActivity代码如下：

    
    /**
     * @author 飞雪无情
     * @since 2011-3-14
     */
    public class ActionActivity extends Activity {
    
    	@Override
    	protected void onCreate(Bundle savedInstanceState) {
    		super.onCreate(savedInstanceState);
    		TextView text=new TextView(this);
    		text.setText("该Activity的Intent Filter值只设置了Action");
    		setContentView(text);
    	}
    
    }


很简单，只有一段文本的展示。

AndroidManifest.xml修改如下:

    
    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.flysnow.intent"
          android:versionCode="1"
          android:versionName="1.0">
        <uses-sdk android:minSdkVersion="8" />
    
        <application android:icon="@drawable/icon" android:label="@string/app_name">
            <activity android:name=".IntentsTestList"
                      android:label="@string/app_name">
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />
                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
            </activity>
            <activity android:name=".ActionActivity" android:label="Action检测">
            	<intent-filter>
            		<action android:name="com.flysnow.intent.ACTION_VIEW"></action>
            		<action android:name="android.intent.action.VIEW"></action>
            		<category android:name="android.intent.category.DEFAULT"></category>
            	</intent-filter>
            </activity>
    
        </application>
    </manifest>


很简单，为我们的ActionContext加上了两个action标签和一个category标签,加category标签设置成android.intent.category.DEFAULT是为了让这个Activity可以接收隐式的Intent请求，这是Android规定的，对于Activity，使用Context.startActivity()传递隐式Intent，默认是包含了android.intent.category.DEFAULT的，所以定义的Activity要想接收，必须在Intent Filter里添加android.intent.category.DEFAULT这个Category。

运行入下图：
![](http://dl.iteye.com/upload/attachment/435312/5a7c0913-7b0f-3035-8b65-abe0a68dd21f.png)
当点击“空Action的Activity”的时候，会报异常，因为Intent什么都没有设置，没有任何的Activity能处理它
。当点击“Action检测-Activity”的时候会打开我们的ActionActivity这个Activity，点击“Action检测-Broadcast”会弹出Toast提示，说明已经被接收到。。从上面我们可以看出，当Intent设置了Action时，只要对应的组件的Intent Filter中包含该Action的定义，那么这个组件就会接收该Intent。。。


因为内容较多，，所以进行了分节。。。这样也会使得看本章带的Demo的时候不至于迷糊，因为这个Demo就只是实现了这一个功能。如果多个功能测试的话，就会在Demo改来改去，导致Demo代码不清晰了。未完待续，敬请期待！！！
