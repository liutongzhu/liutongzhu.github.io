---
author: 飞雪无情
comments: true
date: 2011-03-17 08:41:02+00:00
layout: post
slug: android-intents-and-intent-filters-two
title: Android Intents and Intent Filters(二)
wordpress_id: 92
categories:
- Android
tags:
- Android
- Intent
---

## Category（类别）检测


类别在中是通过标记定义的,Category和Action一样，他们的名字都是一个字符串定义，但是我们在代码中可以使用对应的类别常量，在xml文件定义中只能使用定义好的字符串。Android的Intent类中提供了很多内置的类别定义，一中类别代表一个意思，可以参考说明使用。。比如android.intent.category.LAUNCHER标表示你的应用会展示在启动列表页面，经常和android.intent.action.MAIN搭配使用

下面通过一个例子来说明Category的检测,项目名为Intents,应用名为Intents and Filters，运行在Android2.2版本上.主启动Activity为IntentsTestList。

IntentsTestList代码如下:

    
    /**
     * Intents测试列表类
     * @author 飞雪无情
     * @since 2011-3-14
     */
    public class IntentsTestList extends ListActivity {
    	private String ACTION_VIEW="com.flysnow.intent.ACTION_VIEW";
    	private String CATEGORY_MAN="com.flysnow.intent.CATEGORY_MAN";
    	private String CATEGORY_SHOP="com.flysnow.intent.CATEGORY_SHOP";
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
          //定义ListAdapter
    		setListAdapter(new SimpleAdapter(this, getData(),
    				android.R.layout.simple_list_item_1, new String[] { "title" },new int[] {android.R.id.text1}));
    		getListView().setTextFilterEnabled(true);
    
    		filter.addCategory(CATEGORY_MAN);
    		//注册一个广播
            super.registerReceiver(new BroadcastReceiver() {
    
    			@Override
    			public void onReceive(Context context, Intent intent) {
    				Toast.makeText(context, "该Broadcast的Intent Filter设置了Category和Action", Toast.LENGTH_SHORT).show();
    			}
    		},filter);
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
    		addItem(data, "1个Category检测-Activity", new Intent(ACTION_VIEW).addCategory(CATEGORY_MAN),true);
    		addItem(data, "1个Category检测-Broadcast", new Intent(ACTION_VIEW).addCategory(CATEGORY_MAN),false);
    		addItem(data, "2个Category检测-Activity", new Intent(ACTION_VIEW).addCategory(CATEGORY_MAN).addCategory(CATEGORY_SHOP),true);
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
    	private IntentFilter filter=new IntentFilter(ACTION_VIEW);
    
    }


以上代码主要是一个ListView，列出了三个测试项，1个Category的测试和2个Category的测试，注册了一个广播.
为了测试新建了2个Activity，分别是CategoryActivity和Category2Activity,代码如下：

    
    /**
     * @author 飞雪无情
     * @since 2011-3-14
     */
    public class CategoryActivity extends Activity {
    
    	@Override
    	protected void onCreate(Bundle savedInstanceState) {
    		super.onCreate(savedInstanceState);
    		TextView text=new TextView(this);
    		text.setText("该Activity的Intent Filter值设置了Action和1个Category，不包含android.intent.category.DEFAULT");
    		setContentView(text);
    	}
    
    }



    
    /**
     * @author 飞雪无情
     * @since 2011-3-14
     */
    public class Category2Activity extends Activity {
    
    	@Override
    	protected void onCreate(Bundle savedInstanceState) {
    		super.onCreate(savedInstanceState);
    		TextView text=new TextView(this);
    		text.setText("该Activity的Intent Filter值设置了Action和2个Category，不包含android.intent.category.DEFAULT");
    		setContentView(text);
    	}
    
    }


很简单只是一段文字的说明

AndroidManiftest.xml修改如下：

    
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
            <activity android:name=".CategoryActivity" android:label="1个Category检测">
            	<intent-filter>
            		<action android:name="com.flysnow.intent.ACTION_VIEW"></action>
            		<category android:name="android.intent.category.DEFAULT"></category>
            		<category android:name="com.flysnow.intent.CATEGORY_MAN"></category>
            	</intent-filter>
            </activity>
             <activity android:name=".Category2Activity" android:label="2个Category检测">
            	<intent-filter>
            		<action android:name="com.flysnow.intent.ACTION_VIEW"></action>
            		<category android:name="android.intent.category.DEFAULT"></category>
            		<category android:name="com.flysnow.intent.CATEGORY_MAN"></category>
            		<category android:name="com.flysnow.intent.CATEGORY_SHOP"></category>
            	</intent-filter>
            </activity>
        </application>
    </manifest>


为CategoryActivity添加了2个Category，一个是默认的（隐式Intent必须），一个是自定义的。而相应的Category2Activity则有三个Category。

我们运行测试，效果图如下：![](http://dl.iteye.com/upload/attachment/439568/21480d9b-d2fb-35cd-a240-5b045eacc7bd.png)
当点击“1个Category检测-Activity”的时候，会弹出

![](http://dl.iteye.com/upload/attachment/439572/ccf8c038-7289-3d78-aa21-fecdbe95497c.png)
这是，因为我们的Intent定义了一个Category，这个Category在CategoryActivity和Category2Activity里都有，都能匹配上，所以就会弹出这两个Activity供我们选择，而当我们单击《2个Category检测-Activity》的时候就会直接打开Category2Activity，这是因为这个选项里的Intent有2个Category，只有Category2Activity才能匹配上。。。通过例子我们可以总结到：Intent中所包含的所有Category必须在一个组件的intent-filter中有定义，一个都不能少，否则不能通过检测。。但是intent-filter的可以有额外的Category
.再次提醒：

Android对所有传递给

Context.startActivity()的隐式intent至少包含"android.intent.category.DEFAULT"

未完待续
