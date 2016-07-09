title: "Android Layout的layout_height等属性为什么会不起作用？"
date: 2013-01-28 16:19:58
tags:
- Android
categories:
- Android
---

有的时候，我们配置好的布局文件，在加载完成添加到我们的Activity中后发现，并没有安装我们设置的属性来布局，比为我们设置了android:layout_marginTop="100dip"，但是运行程序后发现一点作用都没有，相似的还有layout_height等以android:layout_开头的属性设置都没有作用，这类问题以我们使用Adapter的作为数据源的时候作用居多，因为Adapter里有一个方法是getView,这个返回的VIew是一个从XML布局里加载的，一般如下：

    if(convertView==null){
    convertView=LayoutInflater.from(mContext).inflate(R.layout.main, null);
    }
    return convertView;

问题恰恰出在我们的LayoutInflater.from(mContext).inflate(R.layout.main, null);这句代码上，在使用inflate的时候，如果第二个参数(View root)为null,那么将不会加载你的布局文件里的最顶层的那个布局节点的布局相关配置（就是以android:layout_开头的属性）..我们可以看下该方法的实现来说明一下，通过查找源代码，inflate的实现都在这个public View inflate(XmlPullParser parser, ViewGroup root, boolean attachToRoot) 方法里定义。。其中一段：

    if (root != null) {
       if (DEBUG) {
          System.out.println("Creating params from root: " +root);
       }
       // Create layout params that match root, if supplied
       params = root.generateLayoutParams(attrs);
       if (!attachToRoot) {
           // Set the layout params for temp if we are not
            // attaching. (If we are, we use addView, below)
            temp.setLayoutParams(params);
        }
    }


可以看到，当root为null的时候是不会执行params = root.generateLayoutParams(attrs);这段代码的，这段代码就是把xml里的布局配置转为LayoutParams,换句说就是加载我们配置的布局属性，以供布局类（FrameLayout等）在onLayout的时候控制View的大小、位置、对齐等等。。以FrameLayout为例，看下它的generateLayoutParams(attrs)方法。
    
    public LayoutParams generateLayoutParams(AttributeSet attrs) {
            return new FrameLayout.LayoutParams(getContext(), attrs);        
        }


很简单，构造了一个FrameLayout.LayoutParams类，该类集成了MarginParams，增加了一个gravity对其的属性配置。。。

在这里，如果要自定义自己的VIewroup，并且该ViewGroup有一些自定义控制布局的属性设置，就可以通过集成View.MarginParams来扩展布局配置，然后重写generateLayoutParams方法，这样系统框架就会自动使用该布局读取我们在xml中配置的布局属性来控制我们的VIew的位置。。


基于以上分析，我们在使用LayoutInflate的inflate方法的时候一定要保证root参数不能为null，其实这个root就是父View的意思，就是说你把xml转换为一个VIew的时候，该VIew的Parent是root，如果你不想把该View添加到该root里，那么让第三个参数 attachToRoot为false，如果要添加则为true.

说到这个问题了，其实还有一些布局，他们的参数配置要满足一定的条件才会起作用，比如FrameLayout里的View，你要想它的leftMargin生效，必须指定它的layout_gravity为left，同理right对应rightMargin.top和bottom也一样。。在使用时注意即可，多看看源代码。要不然就会莫名起名，不知道哪里的问题。

ViewGroup的三条线:

1. onMeasure 测量View的大小
2. onLayout 对View的布局进行控制
3. draw绘制该View，drawChild绘制子VIew



