---
author: 飞雪无情
comments: true
date: 2011-01-08 08:59:56+00:00
layout: post
slug: android-listview-tutorials
title: Android ListView组件的使用
wordpress_id: 116
categories:
- Android
tags:
- Android
- Widget
---

ListView是Android开发中非常常用的组件，ListView可以用来显示一个列表，我们可以对这个列表操作，比如点击列表要做什么等等。这篇文章主要通过一个示例来展示ListView的用法。


请加入，大家一块学习。


## 一、不使用xml布局文件创建一个ListView





	
  1. 创建一个名称为HelloListView的Android工程，可以参见
[Android 第一个Android应用，HelloWorld](/admin/blog/810785)

	
  2. 要使用ListView，需要让你的Activity继承于ListActivity，这个和以前的有区别，以前的都是继承Activity，该ListActivity继承Activity，扩展了很多常用的用于操作ListView的方法，使用很方便。

	
  3. 现修改HelloListView类如下

    
    public class HelloListView extends ListActivity {
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            //setContentView(R.layout.main);
            //设置一个Adapter
            setListAdapter(new ArrayAdapter(this,android.R.layout.simple_list_item_1,COUNTRIES));
        }
        //数据数据
        private  static final String[] COUNTRIES=new String[]{"中国","俄罗斯","英国","法国"};
    }




	
  4. 运行程序效果如下：![](http://dl.iteye.com/upload/attachment/385759/1864e29c-2014-3275-80d2-760c9de2fa23.jpg)
看到了吧，我们的数据已经通过列表的形式显示了出来！！！

这里主要的是使用了一个Adapter—适配器，你可以把他看成是ListView的数据源，ListView要展示的数据都是已Adapter的形势传递给ListView的。这个Adapter很重要，Android的用于传给集合控件（ListView,Spinner,GridView等）的数据都是以Adapter的形势，这样的好处就是只要掌握了Adapter，就可以很随意的给这些集合控制传递数据，因为他们使用的都是Adapter。适配器适配器关键就是适配，只需公布一个Adapter，就全搞定了。Android已经给我们实现了一些常用的适配器，如刚刚使用的数组器，还有简单适配器等,如果这些不能满足，我们还可以通过自定义适配器来实现自己的适配器。其实一种适配器就对应了一个集合控件中的一个元素的布局展示。




## 二:使用xml来自定义ListView





	
  1. 上个例子我们并没有使用在main.xml中定义一个ListView的形势来布局ListView，而是使用的ListActivity中默认的ListView来演示的。

	
  2. 下面就使用我们在main.xml中自定义的ListView,这样我们可以很方便的控制ListView展示的布局，大小，背景色等属性。当然上个例子中我们一样可以通过getListView获取ListView后使用它的方法来改变布局、大小和背景色等。

	
  3. 修改main.xml为：

    
    <?xml version="1.0" encoding="utf-8"?>
    <ListView xmlns:android="http://schemas.android.com/apk/res/android"
    	android:id="@android:id/list"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:background="#FF0000FF"
        >
    </ListView>


这里要注意的是我们设置ListView的id为”@android:id/list”,意思是我们引用Android已经为我们定义好的一个id，名字是list，如果你不定义成这样，这个ListView是不能被ListActivity识别的。然后就是添加了一个蓝色的背景，看看我们这个自定义的ListView是否起了作用。

	
  4. 修改HelloListView类为：

    
    public class HelloListView extends ListActivity {
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.main);
            //设置一个Adapter
            setListAdapter(new ArrayAdapter(this,android.R.layout.simple_list_item_1,COUNTRIES));
        }
        //数据数据
        private  static final String[] COUNTRIES=new String[]{"中国","俄罗斯","英国","法国"};
    }


比上个例子只是多了setContentView(R.layout.main);是为了让Android识别我们定义的ListView。

	
  5. 运行效果图：![](http://dl.iteye.com/upload/attachment/385771/602d0716-3fea-315b-a846-a61c9a78c793.jpg)
背景已经变成了蓝色，说明我们在xml中定义的ListView起作用了。




## 三：自定义要展示的元素





	
  1. 前面的两个例子都是展示一行文字，如果我们想图文混排怎么做呢，比如前面是文字，后面是图片。要实现这种功能就需要我们自定义Adapter啦。

	
  2. 新建布局文件item.xml,内容如下：

    
    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout
      xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="fill_parent"
      android:layout_height="wrap_content">
      <TextView	
      	android:id="@+id/text"
      	android:layout_alignParentLeft="true"
      	android:layout_width="wrap_content"
      	android:layout_height="wrap_content"/>
      <ImageView 
      	android:id="@+id/image"
      	android:layout_alignParentRight="true"
      	android:layout_width="wrap_content"
      	android:layout_height="wrap_content"/>
    </RelativeLayout>


这里主要定义一个TextView和一个ImageView，用于显示列表每一行的文本和图片

	
  3. 修改HelloListView类如下：

    
    public class HelloListView extends ListActivity {
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.main);
            //设置一个Adapter,使用自定义的Adapter
            setListAdapter(new TextImageAdapter(this));
        }
        /**
         * 自定义视图
         * @author 飞雪无情
         *
         */
        private class TextImageAdapter extends BaseAdapter{
            private Context mContext;
        	public TextImageAdapter(Context context) {
    			this.mContext=context;
    		}
            /**
             * 元素的个数
             */
    		public int getCount() {
    			return texts.length;
    		}
    
    		public Object getItem(int position) {
    			return null;
    		}
    
    		public long getItemId(int position) {
    			return 0;
    		}
    		//用以生成在ListView中展示的一个个元素View
    		public View getView(int position, View convertView, ViewGroup parent) {
    			//优化ListView
    			if(convertView==null){
    				convertView=LayoutInflater.from(mContext).inflate(R.layout.item, null);
    				ItemViewCache viewCache=new ItemViewCache();
    				viewCache.mTextView=(TextView)convertView.findViewById(R.id.text);
    				viewCache.mImageView=(ImageView)convertView.findViewById(R.id.image);
    				convertView.setTag(viewCache);
    			}
    			ItemViewCache cache=(ItemViewCache)convertView.getTag();
    			//设置文本和图片，然后返回这个View，用于ListView的Item的展示
    			cache.mTextView.setText(texts[position]);
    			cache.mImageView.setImageResource(images[position]);
    			return convertView;
    		}
        }
        //元素的缓冲类,用于优化ListView
        private static class ItemViewCache{
    		public TextView mTextView;
    		public ImageView mImageView;
    	}
      //展示的文字
        private  String[] texts=new String[]{"天气","我团","背景"};
        //展示的图片
        private int[] images=new int[]{R.drawable.img1,R.drawable.img2,R.drawable.img3};
    }


这里的主要地方是自定义了一个Adapter，我们只需要继承BaseAdapter即可，BaseAdapter已经实现了Adapter的大部分方法，我们继承后只需要实现部分的方法即可。必须实现的就是getCount和getView方法，前一个是返回ListView中有多少个元素，后一个是生成要展示的View。ListView在每添加一个View是就会调用Adapter的getView方法，所以我们有必要对这个方法做优化，例子中就做了部分的优化，一般面试的时候会被问到ListView优化，回答例子中的这些代码就差不多了。

	
  4. 运行，我们会看到效果如图：![](http://dl.iteye.com/upload/attachment/385773/07462b29-e24f-387b-9649-842cfbfb0359.jpg)




## 四、ListView列表中的元素的单击事件响应





	
  1. 如果我们要想单击一个ListView的元素使其作出相应的响应怎么办呢？比如弹出什么，打开什么，展示什么等等，那么我们只需要实现单击事件的函数既可。

	
  2. 要实现ListView的单击事件有两种方法，一个是使用getListView().setOnClickListener(l);设置，这个和平时我们用的控件设置是一样的，不做介绍，下面介绍第二个，就是重写ListActivity的onListItemClick方法，其实这个的最后处理也是ListView的setOnClickListener进行监听调用的。

	
  3. 在HelloListView类中增加如下代码：

    
    @Override
    protected void onListItemClick(ListView l, View v, int position, long id) {
    		Toast.makeText(this, "你单击了"+texts[position], Toast.LENGTH_SHORT).show();




	
  4. 运行单击其中的一行效果如下图：
![](http://dl.iteye.com/upload/attachment/385776/b52954ad-12b5-34dd-8c8c-b2bb03312427.jpg)




## 五、小结


到这里ListView应该算是会使用了，起码常用的功能会的，当然还有很多没有说到的，比如ListView的分割部分，headView和footView以及ListView的分页等等，这些就需要我们自己下去好好摸索了。
