---
author: 飞雪无情
comments: true
date: 2011-01-20 09:41:53+00:00
layout: post
slug: android-gridview-tutorial
title: Android GridView组件的使用
wordpress_id: 114
categories:
- Android
tags:
- Android
- Widget
---

GridView是一个网络布局的视图，他能让你的元素显示在一个个的格子里。我们的桌面就是一个GridView。。





	
  1. 新建一个HelloGridView的工程

	
  2. 修改main.xml代码如下：

    
    <?xml version="1.0" encoding="utf-8"?>
    <GridView
      xmlns:android="http://schemas.android.com/apk/res/android"
      android:id="@+id/gridview"
      android:layout_width="fill_parent"
      android:layout_height="fill_parent"
      android:columnWidth="90dp"
      android:numColumns="auto_fit"
      android:horizontalSpacing="10dp"
      android:verticalSpacing="10dp"
      android:stretchMode="columnWidth"
      android:gravity="center">
    </GridView>


很简单，就是一个GridView。设置列宽为90dp,这样当我们的numColumns设置为auto_fit时，Android就会自动计算我们手机屏幕的大小以决定每一行展示几个元素。这是很方便。android:horizontalSpacing定义列之间的间隔，android:verticalSpacing定义行之间的间隔。android:stretchMode设置为columnWidth是意味着根据列宽自动缩放。

	
  3. 修改我们Activity HelloGridView如下：

    
    /**
     * 
     * @author 飞雪无情
     * @since 2011-1-20
     */
    public class HelloGridView extends Activity {
        /** Called when the activity is first created. */
    	@Override
    	protected void onCreate(Bundle savedInstanceState) {
    		super.onCreate(savedInstanceState);
    		setContentView(R.layout.main);
    		GridView gridView=(GridView)findViewById(R.id.gridview);
    		gridView.setAdapter(new ImageAdapter(this));
    		//单击GridView元素的响应
    		gridView.setOnItemClickListener(new OnItemClickListener() {
    
    			@Override
    			public void onItemClick(AdapterView<!--?--> parent, View view,
    					int position, long id) {
    				//弹出单击的GridView元素的位置
    				Toast.makeText(HelloGridView.this,mThumbIds[position], Toast.LENGTH_SHORT).show();
    			}
    		});
    	}
    	private class ImageAdapter extends BaseAdapter{
    		private Context mContext;
    
    		public ImageAdapter(Context context) {
    			this.mContext=context;
    		}
    
    		@Override
    		public int getCount() {
    			return mThumbIds.length;
    		}
    
    		@Override
    		public Object getItem(int position) {
    			return mThumbIds[position];
    		}
    
    		@Override
    		public long getItemId(int position) {
    			// TODO Auto-generated method stub
    			return 0;
    		}
    
    		@Override
    		public View getView(int position, View convertView, ViewGroup parent) {
    			//定义一个ImageView,显示在GridView里
    			ImageView imageView;
    			if(convertView==null){
    				imageView=new ImageView(mContext);
    				imageView.setLayoutParams(new GridView.LayoutParams(85, 85));
    				imageView.setScaleType(ImageView.ScaleType.CENTER_CROP);
    	            imageView.setPadding(8, 8, 8, 8);
    			}else{
    				imageView = (ImageView) convertView;
    			}
    			imageView.setImageResource(mThumbIds[position]);
    			return imageView;
    		}
    
    	}
    	//展示图片
    	private Integer[] mThumbIds = {
                R.drawable.sample_2, R.drawable.sample_3,
                R.drawable.sample_4, R.drawable.sample_5,
                R.drawable.sample_6, R.drawable.sample_7,
                R.drawable.sample_0, R.drawable.sample_1,
                R.drawable.sample_2, R.drawable.sample_3,
                R.drawable.sample_4, R.drawable.sample_5,
                R.drawable.sample_6, R.drawable.sample_7,
                R.drawable.sample_0, R.drawable.sample_1,
                R.drawable.sample_2, R.drawable.sample_3,
                R.drawable.sample_4, R.drawable.sample_5,
                R.drawable.sample_6, R.drawable.sample_7
        };
    }


这里我们也是采用的自定义Adapter，可以上一节的自定义Adapter讲解。展示了一些图片，然后点击一个图片的时候会显示这个图片所在的位置。

	
  4. 运行程序，效果图如下：
![](http://dl.iteye.com/upload/attachment/401529/01f43426-e794-3215-84c1-9273836917cc.jpg)



本文附件下载:

