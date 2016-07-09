---
author: 飞雪无情
comments: true
date: 2011-03-02 07:44:21+00:00
layout: post
slug: sina-weibo-tool-bar-tabhost
title: 新浪微博布局学习——妙用TabHost
wordpress_id: 95
categories:
- Android
tags:
- Android
---

前言

为了更好的开发Android应用程序，除了熟练掌握基本的UI组件和API外，还需要掌握一些技巧，而这些技巧可以通过阅读一些代码来提高，本系列将与大家分享一些新浪微博布局方面的收获，欢迎交流！

声明

欢迎转载，但请保留文章原始出处:)

博客园：http://www.cnblogs.com

农民伯伯： http://www.cnblogs.com/over140

版本

新浪微博 weibo_10235010.apk

正文

一、效果图

![](http://images.cnblogs.com/cnblogs_com/over140/2011/3/2011-3-1_1.png)

红色部分是本文要实现的目标。

二、实现maintabs.xml

    
    <?xml version="1.0" encoding="UTF-8"?>
    <TabHost android:id="@android:id/tabhost" android:layout_width="fill_parent" android:layout_height="fill_parent"
      xmlns:android="http://schemas.android.com/apk/res/android">
        <LinearLayout android:orientation="vertical" android:layout_width="fill_parent" android:layout_height="fill_parent">
            <FrameLayout android:id="@android:id/tabcontent" android:layout_width="fill_parent" android:layout_height="0.0dip" android:layout_weight="1.0" />
            <TabWidget android:id="@android:id/tabs" android:visibility="gone" android:layout_width="fill_parent" android:layout_height="wrap_content" android:layout_weight="0.0" />
            <RadioGroup android:gravity="center_vertical" android:layout_gravity="bottom" android:orientation="horizontal" android:id="@id/main_radio" android:background="@drawable/maintab_toolbar_bg" android:layout_width="fill_parent" android:layout_height="wrap_content">
                <RadioButton   android:text="@string/main_home" android:checked="true" android:id="@+id/radio_button0" android:layout_marginTop="2.0dip" android:drawableTop="@drawable/icon_1_n" style="@style/main_tab_bottom" />
                <RadioButton android:id="@+id/radio_button1" android:layout_marginTop="2.0dip" android:text="@string/main_news" android:drawableTop="@drawable/icon_2_n" style="@style/main_tab_bottom" />
                <RadioButton android:id="@+id/radio_button2" android:layout_marginTop="2.0dip" android:text="@string/main_my_info" android:drawableTop="@drawable/icon_3_n" style="@style/main_tab_bottom" />
                <RadioButton android:id="@+id/radio_button3" android:layout_marginTop="2.0dip" android:text="@string/menu_search" android:drawableTop="@drawable/icon_4_n" style="@style/main_tab_bottom" />
                <RadioButton android:id="@+id/radio_button4" android:layout_marginTop="2.0dip" android:text="@string/more" android:drawableTop="@drawable/icon_5_n" style="@style/main_tab_bottom" />
            </RadioGroup>
        </LinearLayout>
    </TabHost>


styles.xml

    
     <style name="main_tab_bottom">
            <item name="android:textSize">@dimen/bottom_tab_font_size</item>
            <item name="android:textColor">#ffffffff</item>
            <item name="android:ellipsize">marquee</item>
            <item name="android:gravity">center_horizontal</item>
            <item name="android:background">@drawable/home_btn_bg</item>
            <item name="android:paddingTop">@dimen/bottom_tab_padding_up</item>
            <item name="android:layout_width">fill_parent</item>
            <item name="android:layout_height">wrap_content</item>
            <item name="android:button">@null</item>
            <item name="android:singleLine">true</item>
            <item name="android:drawablePadding">@dimen/bottom_tab_padding_drawable</item>
            <item name="android:layout_weight">1.0</item>
        </style>


home_btn_bg.xml

    
     <selector
              xmlns:android="http://schemas.android.com/apk/res/android">
                <item android:state_focused="true" android:state_enabled="true" android:state_pressed="false" android:drawable="@drawable/home_btn_bg_s" />
                <item android:state_enabled="true" android:state_pressed="true" android:drawable="@drawable/home_btn_bg_s" />
                <item android:state_enabled="true" android:state_checked="true" android:drawable="@drawable/home_btn_bg_d" />
                <item android:drawable="@drawable/transparent" />
            </selector>


代码说明：



	
  1. 需要注意的是他这里把TabWidget的Visibility设置成了gone!也就是默认难看的风格不见了：![](http://images.cnblogs.com/cnblogs_com/over140/2011/3/2011-3-1_2.jpg) ，取而代之的是5个带风格的单选按钮.

	
  2. 注意为单选按钮设置的style，其中最重要的是为其background设置了home_btn_bg.xml，也就是自定义了选中效果。


Java文件

    
    public class MainTabActivity extends TabActivity implements
            OnCheckedChangeListener {
    
        private TabHost mHost;
        private Intent mMBlogIntent;
        private Intent mMoreIntent;
        private Intent mInfoIntent;
        private Intent mSearchIntent;
        private Intent mUserInfoIntent;
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            requestWindowFeature(Window.FEATURE_NO_TITLE);
            setContentView(R.layout.maintabs);
    
            // ~~~~~~~~~~~~ 初始化
            this.mMBlogIntent = new Intent(this, HomeListActivity.class);
            this.mSearchIntent = new Intent(this, SearchSquareActivity.class);
            this.mInfoIntent = new Intent(this, MessageGroup.class);
            this.mUserInfoIntent = new Intent(this, MyInfoActivity.class);
            this.mMoreIntent = new Intent(this, MoreItemsActivity.class);
    
            initRadios();
    
            setupIntent();
        }
    
        /**
         * 初始化底部按钮
         */
        private void initRadios() {
             ((RadioButton) findViewById(R.id.radio_button0)).setOnCheckedChangeListener(this);
             ((RadioButton) findViewById(R.id.radio_button1)).setOnCheckedChangeListener(this);
             ((RadioButton) findViewById(R.id.radio_button2)).setOnCheckedChangeListener(this);
             ((RadioButton) findViewById(R.id.radio_button3)).setOnCheckedChangeListener(this);
             ((RadioButton) findViewById(R.id.radio_button4)).setOnCheckedChangeListener(this);
        }
    
        /**
         * 切换模块
         */
        @Override
        public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
            if (isChecked) {
                switch (buttonView.getId()) {
                case R.id.radio_button0:
                    this.mHost.setCurrentTabByTag("mblog_tab");
                    break;
                case R.id.radio_button1:
                    this.mHost.setCurrentTabByTag("message_tab");
                    break;
                case R.id.radio_button2:
                    this.mHost.setCurrentTabByTag("userinfo_tab");
                    break;
                case R.id.radio_button3:
                    this.mHost.setCurrentTabByTag("search_tab");
                    break;
                case R.id.radio_button4:
                    this.mHost.setCurrentTabByTag("more_tab");
                    break;
                }
            }
        }
    
        private void setupIntent() {
            this.mHost = getTabHost();
            TabHost localTabHost = this.mHost;
    
            localTabHost.addTab(buildTabSpec("mblog_tab", R.string.main_home,
                    R.drawable.icon_1_n, this.mMBlogIntent));
    
            localTabHost.addTab(buildTabSpec("message_tab", R.string.main_news,
                    R.drawable.icon_2_n, this.mInfoIntent));
    
            localTabHost.addTab(buildTabSpec("userinfo_tab", R.string.main_my_info,
                    R.drawable.icon_3_n, this.mUserInfoIntent));
    
            localTabHost.addTab(buildTabSpec("search_tab", R.string.menu_search,
                    R.drawable.icon_4_n, this.mSearchIntent));
    
            localTabHost.addTab(buildTabSpec("more_tab", R.string.more,
                    R.drawable.icon_5_n, this.mMoreIntent));
    
        }
    
        private TabHost.TabSpec buildTabSpec(String tag, int resLabel, int resIcon,
                final Intent content) {
            return this.mHost
                    .newTabSpec(tag)
                    .setIndicator(getString(resLabel),
                            getResources().getDrawable(resIcon))
                    .setContent(content);
        }


代码说明



	
  1. 由于TabWidget被隐藏，所以相关的事件也会无效，这里取巧用RadioGroup与RadioButton的特性来处理切换，然后监听事件调用setCurrentTabByTag来切换Activity。

	
  2. 注意即使TabWidget被隐藏，也要为其设置indicator，否则会保持。


三、总结
在这之前如果要做这种效果我恐怕第一时间就会想到用ActivityGroup来做，主要是因为TabHost的TabWidget非常难看，用起
来也不方便。其实从源码可以看出，TabActivity也是继承自ActivityGroup，这里结合了单选按钮和TabHost，各取其长，有时间
可以专门写一个这样的自定义控件:)

四、相关文章

[[Android]使用ActivityGroup来切换Activity和Layout](http://www.cnblogs.com/over140/archive/2010/09/07/1820876.html)

结束

本文中使用的资源均反编译自apk文件，这里主要是讲思路，欢迎大家交流。
