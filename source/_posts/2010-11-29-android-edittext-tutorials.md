---
author: 飞雪无情
comments: true
date: 2010-11-29 08:51:00+00:00
layout: post
slug: android-edittext-tutorials
title: Android EditText使用详解-包含很多教程上看不到的功能演示
wordpress_id: 118
categories:
- Android
tags:
- Android
- Widget
---

标题有点大，说是详解，其实就是对EditText的一些常用功能的介绍，包括密码框，电话框，空白提示文字等等的讲解，尽量的介绍详细一点，也就是所谓的详解了。。呵呵

## 一：新建HelloEditText工程

新建一个Hello world详细步骤可以参见

[Android 第一个Android应用，HelloWorld](/2010/11/13/android-helloworld.html)

创建设置如下：

1.  Project name:HelloEditText
2.  Build Target :android 2.2
3.  Application name:HelloEditText
4.  Package name:com.flysnow
5.  create Activity:HelloEditText
6.  min SDK 8

这时候运行还看不到EditText，因为我们还没有加上,修改main.xml如下：

	<?xml version="1.0" encoding="utf-8"?>
		<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
			android:orientation="vertical"
			android:layout_width="fill_parent"
			android:layout_height="fill_parent"
			>
		<EditText
			android:id="@+id/edit_text"  
			android:layout_width="fill_parent" 
			android:layout_height="wrap_content" 
			android:text="这是一个EditText"/>
	</LinearLayout>
 
 这里添加了一个id为"edit_text"的EditText，设置默认显示为本为“这是一个EditText”。。运行效果如下：

![](http://dl.iteye.com/upload/attachment/355666/8abc632a-3e6c-3c5f-9dc1-90aac6470f96.png)


## 二：EditText简介


EditText是一个非常重要的组件，可以说它是用户和Android应用进行数据传输窗户，有了它就等于有了一扇和Android应用传输的门，通过它用户可以把数据传给Android应用，然后得到我们想要的数据。

EditText是TextView的子类，所以TextView的方法和特性同样存在于EditText中，具体的TextView的介绍可以参考上一节[Android TextView小组件的使用--附带超链接和跑马灯效果 ](/2010/11/25/android-textview-tutorials.html)


##  三：长度和空白提示文字,提示文字颜色,是否可编辑等


EditText有一些属性可以设置EditText的特性，比如最大长度，空白提示文字等。



	
  1. 有时候我们有一些特属的需要，要求只能在EditText中输入特定个数的字符，比如身份证号、手机号吗等。这时候就可以通过android:maxLength属性来设置最大输入字符个数，比如android:maxLength=“4”就表示最多能输入4个字符，再多了就输入不进去了。

	
  2. 空白提示文字。有时候我们需要说明你定义的这个EditText是做什么用的，比如让输入“用户名”，或者输入“电话号码”等，但是你又不想在EditText前面加一个TextView来说明这是输入“用户名”的，因为这会使用一个TextView，那么怎么办呢？EditText为我们提供了android:hint来设置当EditText内容为空时显示的文本，这个文本只在EditText为空时显示，你输入字符的时候就消失了，不影响你的EditText的文本。。修改main.xml如下：

    
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        >
    <EditText
    	android:id="@+id/edit_text"  
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content"
        android:maxLength="40"
        android:hint="请输入用户名..."/>
    </LinearLayout>


运行应用就会看到如下的效果：

![](http://dl.iteye.com/upload/attachment/355690/1fe441fe-e9fc-32f2-8fcf-53e0445d654c.png)
看看吧，简洁明了还不用新增一个TextView说明，也不影响用户操作。

	
  3. 上面列出了空白时的提示文字，有的人说了，我不想要这个灰色的提示文字，和我的应用整体风格不协调，那也行啊，我们可以换颜色，怎么换呢，就是通过android:textColorHint属性设置你想要的颜色。修改main.xml如下:

    
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        >
    <EditText
    	android:id="@+id/edit_text"  
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content"
        android:maxLength="40"
        android:hint="请输入用户名..."
        android:textColorHint="#238745"/>
    </LinearLayout>


运行程序效果如下：

![](http://dl.iteye.com/upload/attachment/355702/7751967e-633b-3c69-bdc0-f135b1a25a25.png)
看到了吧，颜色已经变了。。

	
  4. 还有一个比较实用的功能，就是设置EditText的不可编辑。设置android:enabled="false"可以实现不可编辑，可以获得焦点。这时候我们看到EditText和一个TextView差不多：![](http://dl.iteye.com/upload/attachment/355705/dccc8638-134c-3532-9d44-03fbb394669b.png)

	
  5. 实现类似html中Textarea的文本域。在Android中没有专门的文本域组件，但是可以通过设置EditText的高来实现同样的文本域功能。修改main.xml如下:

    
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        >
    <EditText
    	android:id="@+id/edit_text"  
        android:layout_width="fill_parent" 
        android:layout_height="200dip"/>
    </LinearLayout>


运行程序效果如下：

![](http://dl.iteye.com/upload/attachment/355707/a1e61bd2-1162-3b7a-8a5c-3dfe880e3866.png)




## 四：输入特殊格式的字符


在我们开发程序的时候不免会输入一些特属个数的字符，比如密码（输入框的字符要加密显示），电话号码（比如数字和-）,数字等，这些都算是一些特属格式的字符，强大的EditText同样为我们提供了输入这些特属格式字符的设置。



	
  1. 密码文本框。密码输入也是Android应用常用的功能，通过配置EditText的android:password="true"就可以实现这一密码输入功能，修改main.xml如下：

    
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        >
    <EditText
    	android:id="@+id/edit_text"  
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content"
        android:password="true"/>
    </LinearLayout>


运行效果如下：

![](http://dl.iteye.com/upload/attachment/355709/b4831d6e-2f6a-3b1e-a8ae-982083b1e596.png)
可以看到我们输入的字符已经被“.”这样的掩码所代替。

	
  2. 手机中发短信打电话是必不可少的，所以用于专门输入电话号码的文本框也是大有用途，有了他我们对是否是电话号码的校验就容易的多了(因为字符是正确的，只要校验格式
).通过设置android:phoneNumber="true"就可以把EditText变成只接受电话号码输入的文本框，连软键盘都已经变成拨号专用软键盘了，所以不用再担心输入其他字符了。修改main.xml如下： 

    
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        >
    <EditText
    	android:id="@+id/edit_text"  
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content"
        android:phoneNumber="true"/>
    </LinearLayout>


运行程序效果如下：

![](http://dl.iteye.com/upload/attachment/355713/525c31d6-adbe-39e2-8edb-afb6431d657f.png)
注意看软键盘，已经变成拨号专用的啦.

	
  3. 有时候我们只想输入数字，不想输入字母，EditText为我们提供了android:numeric来控制输入的数字类型，一共有三种分别为integer（正整数）、signed（带符号整数）和decimal(浮点数)。这里以signed类型的为例，修改main.xml如下：

    
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        >
    <EditText
    	android:id="@+id/edit_text"  
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content"
        android:numeric="signed"/>
    </LinearLayout>


运行效果如下：

![](http://dl.iteye.com/upload/attachment/355719/88cc320b-3d8f-3cd7-9dc0-19c8da6df9dd.png)
注意这里的软键盘变成“数字键盘”的变化.




## 五：为文本指定特定的软键盘类型


前面我们通过指定为电话号码特定格式，然后键盘类型变成了拨号专用的键盘，这个是自动变的，其实我们也可以通
过android:inputType来设置文本的类型，让输入法选择合适的软键盘的。。android:inputType有很多类型，这里使用date类型来演示,修改main.xml如下：

    
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        >
    <EditText
    	android:id="@+id/edit_text"  
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content"
        android:inputType="date"/>
    </LinearLayout>


运行效果如下：

![](http://dl.iteye.com/upload/attachment/355729/bbfe54c6-a87a-3ab6-908d-32644cda7d31.png)


## 六：Enter键图标的设置


软键盘的Enter键默认显示的是“完成”文本，我们知道按Enter建表示前置工作已经准备完毕了，要去什么什么啦。比如，在一个搜索中，我们输入要搜索的文本，然后按Enter表示要去搜索了，但是默认的Enter键显示的是“完成”文本，看着不太合适，不符合搜索的语义，如果能显示“搜索”两个字或者显示一个表示搜索的图标多好。事实证明我们的想法是合理的，Android也为我们提供的这样的功能。通过设置android:imeOptions来改变默认的“完成”文本。这里举几个常用的常量值：



	
  1. actionUnspecified  未指定，对应常量EditorInfo.IME_ACTION_UNSPECIFIED.效果：![](http://dl.iteye.com/upload/attachment/355737/bd9d7b7c-2a93-37b3-afd9-cda2f3593058.png)

	
  2. actionNone 没有动作,对应常量EditorInfo.IME_ACTION_NONE 效果：![](http://dl.iteye.com/upload/attachment/355737/bd9d7b7c-2a93-37b3-afd9-cda2f3593058.png)

	
  3. actionGo 去往，对应常量EditorInfo.IME_ACTION_GO 效果：![](http://dl.iteye.com/upload/attachment/355757/a8db4447-ec91-39aa-a546-c117db16cf55.png)

	
  4. actionSearch 搜索，对应常量EditorInfo.IME_ACTION_SEARCH 效果： ![](http://dl.iteye.com/upload/attachment/355769/c35d6b78-d5b9-314a-9698-144bc12d0af3.png)

	
  5. actionSend 发送，对应常量EditorInfo.IME_ACTION_SEND 效果：![](http://dl.iteye.com/upload/attachment/355788/c168252f-bb3d-3831-8e3c-8bced16b12a1.png)

	
  6. actionNext 下一个，对应常量EditorInfo.IME_ACTION_NEXT 效果：![](http://dl.iteye.com/upload/attachment/355798/aa6bd8b1-8ab3-38d9-b162-01ce10f4f998.png)

	
  7. actionDone 完成，对应常量EditorInfo.IME_ACTION_DONE 效果：![](http://dl.iteye.com/upload/attachment/355804/79979746-7373-3377-bb7e-3dd4bdede464.png)


下面已搜索为例，演示一个实例，修改main.xml如下：

    
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        >
    <EditText
    	android:id="@+id/edit_text"  
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content"
        android:imeOptions="actionSearch"/>
    </LinearLayout>


修改HelloEditText如下：

    
    package com.flysnow;
    
    import android.app.Activity;
    import android.os.Bundle;
    import android.view.KeyEvent;
    import android.widget.EditText;
    import android.widget.TextView;
    import android.widget.Toast;
    import android.widget.TextView.OnEditorActionListener;
    
    public class HelloEditText extends Activity {
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.main);
            EditText editText=(EditText)findViewById(R.id.edit_text);
            editText.setOnEditorActionListener(new OnEditorActionListener() {
    			@Override
    			public boolean onEditorAction(TextView v, int actionId, KeyEvent event) {
    				Toast.makeText(HelloEditText.this, String.valueOf(actionId), Toast.LENGTH_SHORT).show();
    				return false;
    			}
    		});
        }
    }


运行程序，点击回车（也就是搜索图标软键盘按钮）会显示该actionId.我们上面的每一个设置都会对应一个常量，这里的actionId就是那个常量值。

![](http://dl.iteye.com/upload/attachment/355851/c9f95967-fe2f-3850-a96a-17bc220fcc32.png)


## 七：EditText的取值、全选、部分选择、获取选中文本


下面通过一个例子来演示EditText的取值、全选、部分选择和获取选中文本.main.xml修改如下：

    
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        >
    <EditText
    	android:id="@+id/edit_text"  
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content"
        android:imeOptions="actionSearch"/>
    <Button 
    	android:id="@+id/btn_get_value"
    	android:text="取值"
    	android:layout_width="wrap_content"
    	android:layout_height="wrap_content"/>
    <Button 
    	android:id="@+id/btn_all"
    	android:text="全选"
    	android:layout_width="wrap_content"
    	android:layout_height="wrap_content"/>
    <Button 
    	android:id="@+id/btn_select"
    	android:text="从第2个字符开始选择"
    	android:layout_width="wrap_content"
    	android:layout_height="wrap_content"/>
    <Button 
    	android:id="@+id/btn_get_select"
    	android:text="获取选中文本"
    	android:layout_width="wrap_content"
    	android:layout_height="wrap_content"/>
    </LinearLayout>


HelloEditText修改如下：

    
    package com.flysnow;
    
    import android.app.Activity;
    import android.os.Bundle;
    import android.text.Editable;
    import android.text.Selection;
    import android.view.KeyEvent;
    import android.view.View;
    import android.view.View.OnClickListener;
    import android.widget.Button;
    import android.widget.EditText;
    import android.widget.TextView;
    import android.widget.Toast;
    import android.widget.TextView.OnEditorActionListener;
    /**
     * EditText演示
     * @author 飞雪无情
     * @since 2010-11-29
     */
    public class HelloEditText extends Activity {
        /** Called when the activity is first created. */
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.main);
            final EditText editText=(EditText)findViewById(R.id.edit_text);
            //监听回车键
            editText.setOnEditorActionListener(new OnEditorActionListener() {
    			@Override
    			public boolean onEditorAction(TextView v, int actionId, KeyEvent event) {
    				Toast.makeText(HelloEditText.this, String.valueOf(actionId), Toast.LENGTH_SHORT).show();
    				return false;
    			}
    		});
            //获取EditText文本
            Button getValue=(Button)findViewById(R.id.btn_get_value);
            getValue.setOnClickListener(new OnClickListener() {
    			@Override
    			public void onClick(View v) {
    				Toast.makeText(HelloEditText.this, editText.getText().toString(), Toast.LENGTH_SHORT).show();
    			}
    		});
            //让EditText全选
            Button all=(Button)findViewById(R.id.btn_all);
            all.setOnClickListener(new OnClickListener() {
    			@Override
    			public void onClick(View v) {
    				editText.selectAll();
    			}
    		});
            //从第2个字符开始选择EditText文本
            Button select=(Button)findViewById(R.id.btn_select);
            select.setOnClickListener(new OnClickListener() {
    			@Override
    			public void onClick(View v) {
    				Editable editable=editText.getText();
    				Selection.setSelection(editable, 1,editable.length());
    			}
    		});
          //获取选中的文本
            Button getSelect=(Button)findViewById(R.id.btn_get_select);
            getSelect.setOnClickListener(new OnClickListener() {
    			@Override
    			public void onClick(View v) {
    				int start=editText.getSelectionStart();
    				int end=editText.getSelectionEnd();
    				CharSequence selectText=editText.getText().subSequence(start, end);
    				Toast.makeText(HelloEditText.this, selectText, Toast.LENGTH_SHORT).show();
    			}
    		});
        }
        /**
         * 交换两个索引
         * @param start 开始索引
         * @param end 结束索引
         */
    	protected void switchIndex(int start, int end) {
    		int temp=start;
    		start=end;
    		end=temp;
    	}
    }


运行效果如下：

![](http://dl.iteye.com/upload/attachment/356058/4926c2d1-36a0-3cb6-b02f-e743e2313045.png)
可以通过输入文字和点击下面的按钮测试。


## 八：小结


这结详细介绍了EditText的大部分特性和常用功能，如常用的密码框，获取值等等。这几天忙的没更新，这次更新个长的。可以够消化一阵子的。
