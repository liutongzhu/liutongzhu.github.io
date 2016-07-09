---
author: 飞雪无情
comments: true
date: 2011-03-29 02:02:48+00:00
layout: post
slug: android-intents-and-intent-filters-three
title: Android Intents and Intent Filters(三)
wordpress_id: 90
categories:
- Android
tags:
- Android
- Intent
---

## 数据(data)检测


data标记也是在intent-filter中定义的，大致格式如下:

    
    <intent-filter>
            		<action android:name="android.intent.action.VIEW"></action>
            		<category android:name="android.intent.category.DEFAULT"></category>
            		<data android:mimeType="text/plain"></data>
            	</intent-filter>


每个data定义一个URI和数据类型(MIME)，URI由4个属性来定义，分别是android:scheme,android:host,android:port,android:path..这个四个属性构成如下格式的URI:
scheme://host:port/path

如：content://com.flysnow.intent:8080/show/view。其中content就是scheme，com.flysnow.intent就是host，8080就是port,show/view就是path...如果有经常使用ContentProvider的应该熟悉。。我们经常定义的authority不就是host+port吗？还有这几个元素都是可选的，但是不是随便用就可以的，port要依赖于host，没有host，port就会被忽略，不起作用，同样，如果要使用host+port(authority)就必须指定scheme。而path则依赖于scheme和authority。。

还有一个很重要的类型就是mimeType,这个属性用于指定内容的类型，也就是这个组件可以处理哪些类型的内容。。如text/plain表示无格式文本类型，mimeType也支持通配符，使用text/*则表示所有文本类型。通过使用它，你可以很方便的开发出关联打开诸如txt文件，pdf文件的应用。后面的两个自理将会演示txt文件查看器，图片查看器的例子。。MIME可以参考http://www.w3school.com.cn/media/media_mimeref.asp。这里有所有的内容类型的定义。。


### 开发实例-拨打电话，text阅读器和图片查看器




### 


下面通过一个例子来演示data的检测,项目名为Intents,应用名为Intents and Filters，运行在Android2.2版本上.主启动Activity为IntentsTestList。例子包括以下演示：



	
  1. 通过发送intent的方式“打开拨号界面并输入电话123456”。

	
  2. 创建一个Text文件阅读器

	
  3. 创建一个图片查看器


首先我们实现第一项，修改IntentsTestList类如下：

    
    /**
     * Intents测试列表类
     * @author 飞雪无情
     * @since 2011-3-14
     */
    public class IntentsTestList extends ListActivity {
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
          //定义ListAdapter
    		setListAdapter(new SimpleAdapter(this, getData(),
    				android.R.layout.simple_list_item_1, new String[] { "title" },new int[] {android.R.id.text1}));
    		getListView().setTextFilterEnabled(true);
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
    		addItem(data, "打开拨号界面并输入电话123456", new Intent(Intent.ACTION_DIAL, Uri.parse("tel://123456")), true);
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


这时我们运行程序，单击“打开拨号界面并输入电话123456”就会打开系统的自带的拨号界面，并且默认已经录入了要拨打的号码“123456”。效果图如下：
![](http://dl.iteye.com/upload/attachment/449159/2d492720-aea1-316f-a85f-52e465d8b106.png)

![](http://dl.iteye.com/upload/attachment/449161/b0112c45-6c10-351d-a538-80b8b0a34b6c.png)


###   然后我们实现第二功能-txt文件阅读器


新建TextWatcherActivity代码如下：

    
    /**
     * 显示文本的Activity
     * @author 飞雪无情
     * @since 2011-3-24
     */
    public class TextWatcherActivity extends Activity {
    	private TextView mTextView;
    
    	@Override
    	protected void onCreate(Bundle savedInstanceState) {
    		super.onCreate(savedInstanceState);
    		mTextView=new TextView(this);
    		setContentView(mTextView);
    	}
    
    	@Override
    	protected void onStart() {
    		super.onStart();
    		Intent txtIntent=getIntent();
    		Uri data=txtIntent.getData();
    		String txt;
    		try {
    			txt = readTxt(data);
    		} catch (IOException e) {
    			txt="打开txt文件异常";
    		}
    		mTextView.setText(txt);
    	}
    	/**
    	 * 读取txt文本
    	 * @param txtUri
    	 * @return
    	 * @throws IOException
    	 */
    	private String readTxt(Uri txtUri) throws IOException{
    		BufferedReader bufferedReader=new BufferedReader(new InputStreamReader(getContentResolver().openInputStream(txtUri),Charset.forName("GBK")));
    		StringBuilder txt=new StringBuilder();
    		String buf="";
    		while((buf=bufferedReader.readLine())!=null){
    			txt.append(buf).append("\n");
    		}
    		return txt.toString();
    	}
    
    }


然后在AndroidManifest.xml中加入如下定义:

    
    <activity android:name=".TextWatcherActivity"
            	android:label="查看TXT文件">
            	<intent-filter>
            		<action android:name="android.intent.action.VIEW"></action>
            		<category android:name="android.intent.category.DEFAULT"></category>
            		<data android:mimeType="text/plain"></data>
            	</intent-filter>
            </activity>


这样在单击txt文件的时候就可以选择我们的这个Activity对txt文件处理，显示其内容.我们新建一个1.txt文件，写上一些内容，放在我们的sd卡中，使用文件管理工具查看这个txt文件，会弹出如下图的提示，看到我们刚刚做的《TXT阅读器》了吧。

![](http://dl.iteye.com/upload/attachment/449475/079e5ed7-7342-39bf-95af-c95ec024681c.png)

选择“查看TXT文件”，就可以看到我们的txt内容：

![](http://dl.iteye.com/upload/attachment/449477/e1fb042a-0555-3e43-b225-7fe7fce94dad.png)


### 最后实现第三个功能--图片查看器


新建ImageWatcherActivity，代码如下：

    
    /**
     * 显示文本的Activity
     * @author 飞雪无情
     * @since 2011-3-24
     */
    public class ImageWatcherActivity extends Activity {
    	private final String IMAGE_URI_KEY="imageUriKey";
    	private Uri image;
    	private ImageView mImageView;
    
    	@Override
    	protected void onCreate(Bundle savedInstanceState) {
    		super.onCreate(savedInstanceState);
    		mImageView=new ImageView(this);
    		setContentView(mImageView);
    	}
    
    	@Override
    	protected void onStart() {
    		super.onStart();
    		Intent txtIntent=getIntent();
    		image=txtIntent.getData();
    		//对于大图片未做优化处理
    		mImageView.setImageURI(image);
    	}
    
    	@Override
    	protected void onRestoreInstanceState(Bundle savedInstanceState) {
    		super.onRestoreInstanceState(savedInstanceState);
    		image=savedInstanceState.getParcelable(IMAGE_URI_KEY);
    		mImageView.setImageURI(image);
    	}
    
    	@Override
    	protected void onSaveInstanceState(Bundle outState) {
    		super.onSaveInstanceState(outState);
    		outState.putParcelable(IMAGE_URI_KEY,image);
    	}
    
    }


然后在AndroidManifest.xml中加入如下定义:

    
    <activity android:name=".ImageWatcherActivity"
            	android:label="查看图片文件">
            	<intent-filter>
            		<action android:name="android.intent.action.VIEW"></action>
            		<category android:name="android.intent.category.DEFAULT"></category>
            		<data android:mimeType="image/*"></data>
            	</intent-filter>
            </activity>


这样在单击图片文件的时候就可以选择我们的这个Activity对txt文件处理并且显示。

![](http://dl.iteye.com/upload/attachment/449481/9b6c7d30-4ed9-3fb4-8615-42dea2f0e02c.png)

![](http://dl.iteye.com/upload/attachment/449483/dba27c5a-45df-3339-9fa6-e2135cff7fb6.png)


### 数据(data)检测小结


对于data的匹配，如果说怎么怎么匹配，在什么情况下通过可能会比较难以理解，这里以一种简单的方式来解说。

假定我们定义的Intent Filter 的data标签为集合A，传递的Intent中包含的data为集合B，当B是A的子集时就通过了（Action和Category也得检测通过）。如果B为空（不配置data），那么A也得为空（不配置data）才能通过
。更详细（繁琐）的介绍请参考doc


### Intents and Intent Filters总结


Android提供了以Intent的方式调用Android设备的内置Google应用，比如打电话，调用Google浏览器打开网页，搜索等。关于这方便的介绍可以参考Android开发文档《Intents List: Invoking Google Applications on Android Devices》这一节的介绍，很详细。docs/guide/appendix/g-app-intents.html。

Intent是一个很好的设计，它提供了一种在各个组建之间通信的方式，也为我们使用其他的应用的功能提供了可能，这样如果我们想在自己的应用打开一个网页，我们就不用特意迁入一个webview，我们直接调用Android内的浏览器打开即可。。

最后值得一提的是PackageManager这个类中为我们提供了一系列的query...()方法，可以让我们根据我们定义的Intent查询特定的匹配Intent Filter标记的所有组件。。有兴趣的可以研究一下。。
