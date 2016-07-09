---
author: 飞雪无情
comments: true
date: 2011-12-10 09:12:00+00:00
layout: post
slug: android-float-widget
title: 基于Android的浮动组件，可以用于应用中的新功能展示等等。
wordpress_id: 43
categories:
- Android
tags:
- Android
---

## 前言


在开发Android应用时,加新功能是必不可少的，我们加入了新的功能，有的一看界面就可以看出来，但是有的新功能就比较隐蔽，也就是用户很难知道你添加了这个新功能，这个时候就需要用户在打开我们的应用时给出一些提示，说明我们在哪里添加了新功能，点击哪里可以看到这个新功能。这时我们第一时间想到的可能是Toast，因为它用法简单，又不影响用户操作，但是它有个缺点，就是不能明确的指示是哪里添加了新功能，除非你用文字描述出来。为此，我基于Toast编写了一个小组件FloatTextToast（下面遇到的这个名字代替我写的这个组件），他和Toast的用法一样简单，并且弥补了Toast的缺点，也更显得更好看。


## 效果图




![](http://hi.csdn.net/attachment/201112/10/0_13234942045Irb.gif)




## 你可以学到








	
  1. Toast的基本用法

	
  2. Android的消息机制，如何创建自己的消息队列

	
  3. 怎样在Activity启动时获取一个View的width、height、top、left等属性




## 基本思路











	
  1. 首先你要有一个处理好的9 PNG的图片，用于自适应文字显示，关于9 PNG处理可以参考Android Doc

	
  2. 要显示在哪个View的下面，就要知道这个目标View的位置

	
  3. 把要显示的文本放在一个TextView里，使用Toast的setView方法设置Toast要显示的View

	
  4. 根据得到的位置，最后就是使用Toast的setGravity方法把要显示的内容放到正确的位置显示出来即可。




总的来说首先就是要知道目标View，根据targetView计算出要显示提示的位置，然后根据位置使用Toast把提示的文本显示出来。但是这里有几个难点，下面就一一解决







## Activity加载完成时获取targetVIew的宽高和位置属性






我们加入了新的功能提示，自然会在用户打开这个界面的时候就提示，但是在UI没有渲染完成绑定倒Window上的时候，是不能获取倒targetView的width、height和position的，那么我们怎么才能知道targetView的这些属性呢？Activity的onAttachedToWindow回调方法是不能用的，况且它是在API 5加上的，以前的API中并没有。不过我们还有一种方法，那就是在显示提示的时候获取targetView的属性，如果获取不到（为0）就一直获取，直到获取到为止，这其实是一个轮询。为了达到这一目的，我们在开发者调用FloatTextToast.show()的时候使用Android的Message机制轮询获取一个targetView的属性，如果获取到，就会显示提示文字了。在此之前先看下FloatTextToast构造函数，可以对它有个大概的了解，防止后面的代码中出现的成员变量不认识。

    
    private FloatTextToast(Context context,View targetView) {
    		this.mTargetView = targetView;
    		this.mContext= context;
    		mToast=new Toast(mContext);
    		mContentView=new TextView(mContext);
    		mContentView.setBackgroundResource(R.drawable.float_text_toast_bg);
    		mContentView.setTextColor(Color.BLACK);
    		mContentView.setTextSize(TypedValue.COMPLEX_UNIT_DIP,16);
    		mToast.setView(mContentView);
    
    		//初始化一个Handler线程
    		mHandlerThread=new HandlerThread("FloatTextToast");
    		mHandlerThread.start();
    		mHandler=new FloatTextToastHandler(mHandlerThread.getLooper());
    	}







## 自定义自己的消息循环机制




要想在一个自定义的组件中使用Message机制，一定要有自己的Looper机制，我们不能使用Activity的Looper，因为主Looper可能会有其他的Message需要处理，这就会导致我们的show方法会延迟调用，这样效果就不好了，所以要有一个专门的Looper来处理此Message。要声明自己的Looper，就需要HandlerThread这个类的配合了，这可是个好东西，使用它你会很容易的创建一个自己的线程用于处理你Message。使用方法很简单，如下代码：






    
    //初始化一个Handler线程
    		mHandlerThread=new HandlerThread("FloatTextToast");
    		mHandlerThread.start();
    		mHandler=new FloatTextToastHandler(mHandlerThread.getLooper());


这样就声明了一个HandlerThread并且让它运行，运行之后我们就可以获取一个属于该Thread的Looper，然后把Message发送给这个Looper，那么这个线程就可以处理你发送的消息了。。看看我们的自定义Handler








    
    private class FloatTextToastHandler extends Handler{
    
    		public FloatTextToastHandler(Looper looper) {
    			super(looper);
    		}
    
    		@Override
    		public void handleMessage(Message msg) {
    			switch(msg.what){
    			case WHAT_SHOW:
    				showInHandler();
    			}
    		}
    
    	}


它需要传递一个Looper作为构造参数声明，意思就是使用这个Looper处理我发send的Message的意思。上面的代码








    
    mHandler=new FloatTextToastHandler(mHandlerThread.getLooper());


正是我们使用自己开启的线程处理我们的Message的意思。下面看下我们的showInHandler()方法是怎么处理的。








    
    /**在Handler调用的show方法，主要为了等待{@link #mTargetView}的位置*/
    	private void showInHandler(){
    		int[] targetPos=getTargetViewPos();
    		if(targetPos[0]==0&&targetPos[1]==0){
    			mHandler.sendEmptyMessageDelayed(WHAT_SHOW, 100);
    		}else{
    			final Rect contentPos=getContentViewPos(targetPos);
    			mToast.setGravity(Gravity.LEFT|Gravity.TOP, contentPos.left, contentPos.top);
    			mToast.show();
    		}
    	}


该方法其实就是在获取targetVIew的位置，如果获取不到，则向自定义的Looper里发送一个Message重新调用该函数，如果得到了位置，那么就调用Toast的setGravity方法设置好要显示文本的位置，然后显示即可。






## 获取要显示文本的位置






要获取显示的位置，就要知道targetVIew的位置以及它的宽、高，这样就能计算要显示文本的位置了。View组件都有一个函数，可以把自己在Window里的坐标转换为一个数组。

    
    private int[] getTargetViewPos(){
    		final int[] targetPos=new int[2];
    		mTargetView.getLocationInWindow(targetPos);
    		return targetPos;
    	}


这样，就返回了targetView的xy坐标了。光有targetView的坐标还不够，还要有contentView最终要显示的位置。








    
    /**
    	 * 计算获取浮动文本显示的位置，把浮动文本放在targetView的中心处
    	 * @return 一个包含top和left的Rect
    	 */
    	private  Rect getContentViewPos(int[] targetPos){
    		final Rect windowVisibleRect=new Rect();
    		final View targetView=mTargetView;
    		final TextView contentView=mContentView;
    		//状态栏高度
    		targetView.getWindowVisibleDisplayFrame(windowVisibleRect);
    		int statusBarHeight=windowVisibleRect.top;
    
    		//背景图那个三角箭头的位置
    		final TextPaint textPaint=contentView.getPaint();
    		int contentW=(int)textPaint.measureText((String)contentView.getText());
    		int arrowPos=(int)(contentW*(30.0/160));
    
    		final Rect rect = new Rect();
            rect.left = targetPos[0]+targetView.getWidth()/2-arrowPos;
            rect.top = targetPos[1]-statusBarHeight + targetView.getHeight();
            return rect;
    	}


这个函数的功能就是让文本显示在targetView的下方的横向中间的位置，也就是文本的背景尖角三角要指向targetView横向中间的位置，这样才好看些。为了这个才需要使用Paint测量文本的宽度，所以这也是该组件的一个缺陷，不能显示String格式之外的字符，比如SpannableString。






## 完整的组件代码






上面是对组件代码的拆分讲解，是为了说明我们当时实现这个组件的想法以及步骤，下面就整体把代码列出来，明了的看一下。

    
    /**
     * 浮动的文本显示。根据一个提供的View，可以把文本显示到该View的下面.
     * 可以设置显示的时间，多了该时间后自动消失。目前只支持纯文本{@link String}类型的显示
     * 因为要计算显示文本的宽度。
     * @author michael_li(飞雪无情)
     * @since 2011-12-10 下午04:57:36
     */
    public class FloatTextToast {
    	public static final int LENGTH_LONG=Toast.LENGTH_LONG;
    	public static final int LENGTH_SHORT=Toast.LENGTH_SHORT;
    	private static final int WHAT_SHOW=1;
    
    	private Context mContext;
    	private View mTargetView;
    	private Toast mToast;
    	private  TextView mContentView;
    
    	private HandlerThread mHandlerThread;
    	private FloatTextToastHandler mHandler;
    	private FloatTextToast(Context context,View targetView) {
    		this.mTargetView = targetView;
    		this.mContext= context;
    		mToast=new Toast(mContext);
    		mContentView=new TextView(mContext);
    		mContentView.setBackgroundResource(R.drawable.float_text_toast_bg);
    		mContentView.setTextColor(Color.BLACK);
    		mContentView.setTextSize(TypedValue.COMPLEX_UNIT_DIP,16);
    		mToast.setView(mContentView);
    
    		//初始化一个Handler线程
    		mHandlerThread=new HandlerThread("FloatTextToast");
    		mHandlerThread.start();
    		mHandler=new FloatTextToastHandler(mHandlerThread.getLooper());
    	}
    	/**
    	 * 生成一个FloatTextToast
    	 * @param context Activity 上下文
    	 * @param targetView  目标View，浮动文本要显示在哪个View下面
    	 * @param text 要显示的文本
    	 * @param duration 浮动文本显示的时间 {@link #LENGTH_LONG} {@link #LENGTH_SHORT}
    	 * @return 一个FloatTextToast，可以调用{@link #show()}显示
    	 */
    	public static FloatTextToast makeText(Context context,View targetView, String text, int duration) {
    		final FloatTextToast floatToast=new FloatTextToast(context,targetView);
    		final TextView contentView=floatToast.mContentView;
    		contentView.setText(text);
    		floatToast.mToast.setDuration(duration);
    		return floatToast;
    	}
    	/**
    	 * 显示浮动文本
    	 */
    	public void show(){
    		mHandler.sendEmptyMessage(WHAT_SHOW);
    	}
    	/**在Handler调用的show方法，主要为了等待{@link #mTargetView}的位置*/
    	private void showInHandler(){
    		int[] targetPos=getTargetViewPos();
    		if(targetPos[0]==0&&targetPos[1]==0){
    			mHandler.sendEmptyMessageDelayed(WHAT_SHOW, 100);
    		}else{
    			final Rect contentPos=getContentViewPos(targetPos);
    			mToast.setGravity(Gravity.LEFT|Gravity.TOP, contentPos.left, contentPos.top);
    			mToast.show();
    		}
    	}
    	private int[] getTargetViewPos(){
    		final int[] targetPos=new int[2];
    		mTargetView.getLocationInWindow(targetPos);
    		return targetPos;
    	}
    	/**
    	 * 计算获取浮动文本显示的位置，把浮动文本放在targetView的中心处
    	 * @return 一个包含top和left的Rect
    	 */
    	private  Rect getContentViewPos(int[] targetPos){
    		final Rect windowVisibleRect=new Rect();
    		final View targetView=mTargetView;
    		final TextView contentView=mContentView;
    		//状态栏高度
    		targetView.getWindowVisibleDisplayFrame(windowVisibleRect);
    		int statusBarHeight=windowVisibleRect.top;
    
    		//背景图那个三角箭头的位置
    		final TextPaint textPaint=contentView.getPaint();
    		int contentW=(int)textPaint.measureText((String)contentView.getText());
    		int arrowPos=(int)(contentW*(30.0/160));
    
    		final Rect rect = new Rect();
            rect.left = targetPos[0]+targetView.getWidth()/2-arrowPos;
            rect.top = targetPos[1]-statusBarHeight + targetView.getHeight();
            return rect;
    	}
    	private class FloatTextToastHandler extends Handler{
    
    		public FloatTextToastHandler(Looper looper) {
    			super(looper);
    		}
    
    		@Override
    		public void handleMessage(Message msg) {
    			switch(msg.what){
    			case WHAT_SHOW:
    				showInHandler();
    			}
    		}
    
    	}
    }


此组件和Toast的实现方法一样，所以上手不难，只需使用makeText静态方法生成一个即可








    
    FloatTextToast.makeText(Context context, View targetView, String text, int duration).show()


就这么简单，传进去几个参数，show出即可，和Toast一样好用。






## 小结




这里主要是通过类之间的组合编写一个一个FloatTextToast组件，便于在应用中提示一些信息，不光局限于新功能的提示，还有其他的点击查看个人信息等等，就如上面的效果图一样。这里主要的难点就在于Activity启动获取targetView的状态，这里采用了不受影响的自定义的消息机制，能及时的获取targetView的状态。这里也采用的Toast的队列机制，这样就能够更好的一个个的提示，让用户看完一个再显示另外一个，不至于一下子全显示出来，而用户没有时间看。这里还采用了Paint用于测量文本的真实宽度，所以也有了一些缺陷，如果哪位有更好的方法，也可以留言告知我，不胜感激。




![](http://hi.csdn.net/attachment/201112/10/0_1323508315BC94.gif)
