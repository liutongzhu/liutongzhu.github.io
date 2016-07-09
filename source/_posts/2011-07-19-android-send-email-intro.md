---
author: 飞雪无情
comments: true
date: 2011-07-19 14:50:52+00:00
layout: post
slug: android-send-email-intro
title: Android中我为什么发不了邮件--Android邮件发送详解
wordpress_id: 87
categories:
- Android
tags:
- Android
- Email
---

Android中我为什么发不了邮件？？？我手机里明明有邮件客户端的，可我为什么不能调用它发送邮件？？？相信这是很多人会问的问题，手机里有Email客户端，可是偏偏在自己的应用里调用不了，抑或是不知道怎么调用，还有的是一直认为自己写对了，可是偏偏不能调用，无奈之下只能却网上找段代码粘贴上，ok，可以了。。可是你知道别人的代码为什么可以？你知道调用Email是怎么工作的吗？如果你又像给多人发邮件，还想发送附件，怎么做？又迷糊了吧？所以需要搞懂原理才能一通百通，举一反三，而这就是这篇博文要写的，也是你可以在这篇博文中学到的。

在这里你可以学到：



	
  1. 什么是Intent和Intent Fileter

	
  2. 如何发送Email

	
  3. 如何发送特定的标题和内容的Emil

	
  4. 如何发送给多人

	
  5. 如何抄送和密送

	
  6. 如何发送附件




## 一：什么是Intent和Intent Fileter


关于这个详细介绍请参考 :

[Intents and Intent Filters(一)](/2011/03/14/android-intents-and-intent-filters-one.html)

[Intents and Intent Filters(二)](/2011/03/17/android-intents-and-intent-filters-two.html)

[Intents and Intent Filters(三)](/2011/03/29/android-intents-and-intent-filters-three.html)


## 二：如何发送Email


通过上面就会知道Android应用之间的通信是通过Intent这个东西，如果手机中有实现了你想要功能的应用那么你就不用麻烦去实现一个拉。直接使用就可以，比如这里的发送邮件。

我们新建一个工程SendEmail，基于Android1.6实现。然后定义布局文件和代码如下：

    
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        >
    <Button
    	android:id="@+id/send_btn"  
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content" 
        android:text="Send Action"
        />
    </LinearLayout>




    
     /** 发送邮件测试界面
     * @author 飞雪无情
     * ＠since 2011-7-19
     */
    public class SendEmailActivity extends Activity implements View.OnClickListener{
    	private Button mSendBtn;
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.main);
            mSendBtn=(Button)findViewById(R.id.send_btn);
            mSendBtn.setOnClickListener(this);
        }
    	@Override
    	public void onClick(View v) {
    		switch(v.getId()){
    		case R.id.send_btn:
    			startSendEmailIntent();
    			break;
    		}
    	}
    	private void startSendEmailIntent(){
    		Intent data=new Intent(Intent.ACTION_SENDTO);
    		data.setData(Uri.parse("mailto:qq10000@qq.com"));
    		data.putExtra(Intent.EXTRA_SUBJECT, "这是标题");
    		data.putExtra(Intent.EXTRA_TEXT, "这是内容");
    		startActivity(data);
    	}
    }


这是我们运行应用，点击按钮就会看到如下效果:

![](http://dl.iteye.com/upload/attachment/520422/f01623aa-77eb-3a1f-bf30-306092c83e0d.png)
![](http://dl.iteye.com/upload/attachment/520425/ed85ef04-3760-33a4-a0e7-b4fc2701eaca.png)


## 三：分析发送代码


主要的发送代码如下：

    
    Intent data=new Intent(Intent.ACTION_SENDTO);
    data.setData(Uri.parse("mailto:qq10000@qq.com"));
    data.putExtra(Intent.EXTRA_SUBJECT, "这是标题");
    data.putExtra(Intent.EXTRA_TEXT, "这是内容");
    startActivity(data);





	
  1.  首先构造一个Action为SENDTO的Action

	
  2. 其次创建一个Uri，他是一个mailto的邮件链接，也就是接收地址.

	
  3. 再通过Intent.EXTRA_SUBJECT和Intent.EXTRA_TEXT传递标题和文本的数据，记住，一定要使用这些常量，这是标准，否则Email客户端不认识，那样你传递的标题和文本就不起作用了。

	
  4. 然后就是startActivity进行打开，系统会根据Intent自动匹配找到能处理这个intent的应用，如图列表，有很多个，我们这里选择网易掌上邮，就打开了一个邮件发送界面，如我们所愿，邮件地址、标题和内容都在了，很成功.

	
  5. intent中的数据传递是以Extra的方式进行put后传递的，然后在使用的时候getter取出使用。




## 四：发送给多人以及抄送和密送


发送给多人以及抄送和密送都很简单，查找Intent类中的Extra常量，发送有这么三个常量:Intent.EXTRA_EMAIL,Intent.EXTRA_CC,Intent.EXTRA_BCC。这三个分别用于传递“接受人地址列表”、“抄送人地址列表”和“密送人地址列表”，传递的都是String[]类型的Email地址，如果数组中有多个地址，就可以发送给多人。发送的函数修改如下，我们看下效果:

    
    private void startSendEmailIntent(){
    		Intent data=new Intent(Intent.ACTION_SENDTO);
    		data.setData(Uri.parse("mailto:qq10000@qq.com"));
    		data.putExtra(Intent.EXTRA_EMAIL, new String[]{"ls8707@163.com","ls8708@163.com"});
    		data.putExtra(Intent.EXTRA_CC, new String[]{"ls8709@163.com"});
    		data.putExtra(Intent.EXTRA_BCC, new String[]{"ls810@163.com"});
    		data.putExtra(Intent.EXTRA_SUBJECT, "这是标题");
    		data.putExtra(Intent.EXTRA_TEXT, "这是内容");
    		startActivity(data);
    	}


![](http://dl.iteye.com/upload/attachment/520438/c837b159-0616-3890-8344-2dd841b43585.png)


## 五，发送附件


Email'客户端的发送Activity提供了两个Intent Filter，一个用于发送普通邮件，一个用户发送带有附件的邮件。区分很很容易的，发送普通邮件的那个Intent Filter配置的Action SENDTO，而带有附件的邮件则是SEND,所以是2个不一样，但是哪些比如附加“标题”，“内容”，“抄送”，“发送多人”等等还是通用的。布局文件新增一个按钮，用于发送带有附件的邮件，Activity代码也做修改。

    
    <Button
    	android:id="@+id/send_to_btn"  
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content" 
        android:text="SendTo Action"/>




    
     /* * 发送邮件测试界面
     * @author 飞雪无情 
     * ＠since 2011-7-19
     */
    public class SendEmailActivity extends Activity implements View.OnClickListener{
    	private Button mSendBtn;
    	private Button mSendToBtn;
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.main);
            mSendBtn=(Button)findViewById(R.id.send_btn);
            mSendToBtn=(Button)findViewById(R.id.send_to_btn);
            mSendBtn.setOnClickListener(this);
            mSendToBtn.setOnClickListener(this);
        }
    	@Override
    	public void onClick(View v) {
    		switch(v.getId()){
    		case R.id.send_btn:
    			startSendEmailIntent();
    			break;
    		case R.id.send_to_btn:
    			startSendToEmailIntent();
    			break;
    		}
    	}
    	private void startSendEmailIntent(){
    		Intent data=new Intent(Intent.ACTION_SENDTO);
    		data.setData(Uri.parse("mailto:qq10000@qq.com"));
    		data.putExtra(Intent.EXTRA_EMAIL, new String[]{"ls8707@163.com","ls8708@163.com"});
    		data.putExtra(Intent.EXTRA_CC, new String[]{"ls8709@163.com"});
    		data.putExtra(Intent.EXTRA_BCC, new String[]{"ls810@163.com"});
    		data.putExtra(Intent.EXTRA_SUBJECT, "这是标题");
    		data.putExtra(Intent.EXTRA_TEXT, "这是内容");
    		startActivity(data);
    	}
    	private void startSendToEmailIntent(){
    		Intent data=new Intent(Intent.ACTION_SEND);
    		data.putExtra(Intent.EXTRA_EMAIL, new String[]{"ls8707@163.com"});
    		data.putExtra(Intent.EXTRA_SUBJECT, "这是标题");
    		data.putExtra(Intent.EXTRA_TEXT, "这是内容");
    		data.putExtra(Intent.EXTRA_STREAM, Uri.parse(""));
    		data.setType("text/plain");
    		startActivity(data);
    	}
    }


运行应用，我们看到效果图里最小面的附件标识，说明已经可以发送附件了.

![](http://dl.iteye.com/upload/attachment/520458/5ebe4f65-d647-3e0e-8a28-c351e8b5fccb.png)

发送附件最重要的就是Action必须为SEND，type一定要设置setType(""),还要为Intent.EXTRA_STREAM赋值，类型为Uri，一个附件的Uri。这样才能发送附件。


## 六，客户端Email应用的发送Activity配置


这是Android1.6自带Email客户端的配置，如下：

    
    <activity
                android:name=".activity.MessageCompose"
                android:label="@string/app_name"
                android:enabled="false"
                >
                <intent-filter>
                    <action android:name="android.intent.action.VIEW" />
                    <action android:name="android.intent.action.SENDTO" />
                    <data android:scheme="mailto" />
                    <category android:name="android.intent.category.DEFAULT" />
                    <category android:name="android.intent.category.BROWSABLE" />
                </intent-filter>
                <intent-filter android:label="@string/app_name">
                    <action android:name="android.intent.action.SEND" />
                    <data android:mimeType="text/plain" />
                    <data android:mimeType="image/*" />
                    <data android:mimeType="video/*" />
                    <category android:name="android.intent.category.DEFAULT" />
                </intent-filter>
            </activity>




从以上代码总结:



	
  1. 一个Activity定义了2个Intent Filter用于发送不同类型邮件。

	
  2. SENDTO发送普通邮件，SEND可以发送普通和带有附件的，不过最好主要用于发送附件。

	
  3. 使用SENDTO时，必须为Intent设置Data，也就是Uri，内容为"mailto:emailaddr",否则不能匹配，因为有schema必须为mailto的限制

	
  4. 使用SEND时必须要设置type，也就是使用setType()方法，type只能为以前xml定义的4个。不过我测试了text/*也可以，其他的没测试.

	
  5. 发送附件只能使用SEND这个Action.







附加Email客户端中提取Email地址，抄送，标题，附件等信息的源代码分析.






    
    void initFromIntent(Intent intent) {
    
            //提取接受人地址列表
            String[] extraStrings = intent.getStringArrayExtra(Intent.EXTRA_EMAIL);
            if (extraStrings != null) {
                addAddresses(mToView, extraStrings);
            }
    //提取抄送人地址列表
            extraStrings = intent.getStringArrayExtra(Intent.EXTRA_CC);
            if (extraStrings != null) {
                addAddresses(mCcView, extraStrings);
            }
    //提取密送人地址列表
            extraStrings = intent.getStringArrayExtra(Intent.EXTRA_BCC);
            if (extraStrings != null) {
                addAddresses(mBccView, extraStrings);
            }
    //提取标题
            String extraString = intent.getStringExtra(Intent.EXTRA_SUBJECT);
            if (extraString != null) {
                mSubjectView.setText(extraString);
            }
    
            //提取Uri中的接受人地址
            final Uri dataUri = intent.getData();
            if (dataUri != null) {
                if ("mailto".equals(dataUri.getScheme())) {
                    initializeFromMailTo(dataUri.toString());
                } else {
                    String toText = dataUri.getSchemeSpecificPart();
                    if (toText != null) {
                        addAddresses(mToView, toText.split(","));
                    }
                }
            }
    
           //提取邮件内容
    
            CharSequence text = intent.getCharSequenceExtra(Intent.EXTRA_TEXT);
            if (text != null) {
                mMessageContentView.setText(text);
            }
    
          //提取附件，可以看出Action必须是SEND.EXTRA_STREAM中有数据
         //type不能为null,这样才能提取倒附件.
    
            if (Intent.ACTION_SEND.equals(intent.getAction()) && intent.hasExtra(Intent.EXTRA_STREAM)) {
                String type = intent.getType();
                Uri stream = (Uri) intent.getParcelableExtra(Intent.EXTRA_STREAM);
                if (stream != null && type != null) {
                    if (MimeUtility.mimeTypeMatches(type, Email.ACCEPTABLE_ATTACHMENT_SEND_TYPES)) {
                        addAttachment(stream);
                    }
                }
            }
    
            // Finally - expose fields that were filled in but are normally hidden, and set focus
    
            if (mCcView.length() > 0) {
                mCcView.setVisibility(View.VISIBLE);
            }
            if (mBccView.length() > 0) {
                mBccView.setVisibility(View.VISIBLE);
            }
            setNewMessageFocus();
            mDraftNeedsSaving = false;
        }






