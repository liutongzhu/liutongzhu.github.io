---
author: 飞雪无情
comments: true
date: 2013-10-11 05:23:02+00:00
layout: post
slug: android-bool-preferences-storage-optimization
title: Android布尔型配置存储优化
wordpress_id: 184
categories:
- Android
tags:
- Android
---

在Android开发的过程中，我们基本上都会遇到是否开启自动备份、是否保存账号、是否自动登陆、是否开启向导等这样的选项功能，对于这类功能，我们一般的做法是采用SharedPreferences类存储Boolean类型的配置文件来实现，比如是否第一次打开，我们会使用SharedPreferences存储Key为firset_time_open的Boolean值，在使用的时候我们取出该值进行判断即可。那么这么做有什么问题呢？问题就在于随着我们的Android产品越来越大，越来越复杂，你会发现我们使用了很多个这样的Boolean类型的配置，每一个配置对应一个key，会有很多个key值声明，并且存储的xml文件中也会有很多个bool的节点来保存我们的配置，这样一来我们会不太好维护，二来xml存储文件会越来越大，读写操作速度就会降低。

其实Boolean值对于计算机来说就是0或者1,所以我们储存的boolean值就可以用位来表示，0代表false，1代表true。这样一个Long型的值就可以表示63个Boolean类型值的存储，而如果使用Boolean就需要63个Key，使用Long存储只需要1个Key就可以搞定。下面让我们看下Long最大值的二进制表示：

    
    //最高位表示正负
    0111111111111111 1111111111111111
    1111111111111111 1111111111111111


每一位无非是0或者1,也就是我们的false或者true，那么只要我们规定好每一位表示的是什么含义就好了。比如第1位表示是否是第一次打开，第2位表示是否保存账号，这样在使用的时候，我们只需获取这个存储的Long值，然后看下它的第1位是0还是1就可以知道用户是不是第一次打开了，同理通过第2位是0还是1也可以知道用户是不是选择了保存账号。以此类推，你可以位3-63位定义不同的含义来实现产品中的功能。

通过上面的分析，大家应该都已经了解了原理，也可能会看出关键的地方，就是我们怎么样设置每一位的值而又可以保证其他位的值不变呢，这是个关键。比如我们设置保存账号位true，那么你在设置的时候不能改变第1位是否第一次开发的值。想要实现这个功能，首先要设置好一个掩码(Mask),每一位都有自己的掩码，然后通过与或非操作来设置每一位的true或者false。这个掩码的规则就是：需要改变的那一位是1,其他位都0,这样才好进行设置等操作，比如第1位是否第一次打开和第2位是否保存账号的掩码就是：

    
    //第1位是否第一次打开掩码
    0000000000000000 0000000000000000 
    0000000000000000 0000000000000001
    
    //第2位是否保存账号掩码
    0000000000000000 0000000000000000 
    0000000000000000 0000000000000010



    
    	private final static long MASK_FIRST_TIME_OPEN=1;
    	private final static long MASK_SAVE_ACCOUNT=1<<1;
    	//etc <<2\<<3.....<<62;


这样我们设置第1位为1的时候只需要让第1位的掩码和当前值进行或(|)操作即可，设置第1位为0的时候只需要把第1位先取反(~)然后和当前值进行与(&)操作即可：

    
    //设置第1位位1,或操作(|)
    0000000000000000 0000000000000000 
    0000000000000000 0000000000000001
                                    |//或
    0000000000000000 0000000000000000 
    0000000000000000 0100000100000000
                                    =//等于
    0000000000000000 0000000000000000 
    0000000000000000 0100000100000001
    
    //设置第1位位0,掩码取反(~)后与操作(&)
    0000000000000000 0000000000000000 
    0000000000000000 0000000000000001
                                    ~//取反
                                    =//等于
    1111111111111111 1111111111111111 
    1111111111111111 1111111111111110
                                    &//与操作
    0000000000000000 0000000000000000 
    0000000000000000 0100000100000000
                                    =//等于
    0000000000000000 0000000000000000 
    0000000000000000 0100000100000000



    
    	/**
    	 * 是否第一次打开
    	 * @return
    	 */
    	public static boolean isFirstTime(){
    		return (getBooleanOneGroup()&MASK_FIRST_TIME_OPEN)!=0;
    	}
    	/**
    	 * 设置是否第一次打开
    	 * @param value
    	 */
    	public static void setFirstTime(boolean value){
    		long originValue=getBooleanOneGroup();
    		setBooleanOneGroup(value?(MASK_FIRST_TIME_OPEN|originValue):(~MASK_FIRST_TIME_OPEN&originValue));
    	}


上面的代码是Android中实现获取是否第一次打开 ，以及设置是否第一次打开值的实现。设置的原理上面已经说了，或者的原理相比简单多了，只需要把当前值和掩码做与(&)操作即可，结果不等于0就是true,等于0就是false。

以前是以第1位为例子说明的实现方法，对于每一组来说可以有63位可供存储，每一组都是一个Long类型的存储配置，现第一次使用如下方式进行存储：

    
    	//每组最大移位62位,第1组63个位使用完，可以再定义第二组
    	private final static String KEY_BOOLEAN_ONE_GROUP="boolean_one_group";
            //下面是第一组的布尔型设置，用完了可以有第二组、第三组等等
    	private static long getBooleanOneGroup(){
    		return SP.getLong(KEY_BOOLEAN_ONE_GROUP, 0);
    	}
    	private static void setBooleanOneGroup(long value){
    		EDITOR.putLong(KEY_BOOLEAN_ONE_GROUP, value);
    		EDITOR.commit();
    	}


上面是第一组的方式，第一组63个存储用完之后再按照相同结构定义第二组、第三组等等。下面给说第一组的第1位和第2位的完整代码实现，大家参照此格式进行扩充。

    
    public class BooleanPreferences {
    	private final static SharedPreferences SP=ApplicationContext.context.getSharedPreferences("boolean_preferences", Context.MODE_PRIVATE);
    	private final static Editor EDITOR=SP.edit();
    
    	//每组最大移位62位,第1组63个位使用完，可以再定义第二组
    	private final static String KEY_BOOLEAN_ONE_GROUP="boolean_one_group";
    	private final static long MASK_FIRST_TIME_OPEN=1;
    	private final static long MASK_SAVE_ACCOUNT=1<<1;
    	//etc <<2\<<3.....<<62;
    
    	/**
    	 * 是否第一次打开
    	 * @return
    	 */
    	public static boolean isFirstTime(){
    		return (getBooleanOneGroup()&MASK_FIRST_TIME_OPEN)!=0;
    	}
    	/**
    	 * 设置是否第一次打开
    	 * @param value
    	 */
    	public static void setFirstTime(boolean value){
    		long originValue=getBooleanOneGroup();
    		setBooleanOneGroup(value?(MASK_FIRST_TIME_OPEN|originValue):(~MASK_FIRST_TIME_OPEN&originValue));
    	}
    	/**
    	 * 是否保存账号
    	 * @return
    	 */
    	public static boolean isSaveAccount(){
    		return (getBooleanOneGroup()&MASK_SAVE_ACCOUNT)!=0;
    	}
    	/**
    	 * 设置是否保存账号
    	 * @param value
    	 */
    	public static void setSaveAccount(boolean value){
    		long originValue=getBooleanOneGroup();
    		setBooleanOneGroup(value?(MASK_SAVE_ACCOUNT|originValue):(~MASK_SAVE_ACCOUNT&originValue));
    	}
    
    	//下面是第一组的布尔型设置，用完了可以有第二组、第三组等等
    	private static long getBooleanOneGroup(){
    		return SP.getLong(KEY_BOOLEAN_ONE_GROUP, 0);
    	}
    	private static void setBooleanOneGroup(long value){
    		EDITOR.putLong(KEY_BOOLEAN_ONE_GROUP, value);
    		EDITOR.commit();
    	}
    }


记住。每一位的Mask掩码不能重复，不然就会把其他位的值改变，影响程序的功能，所以要严格按照代码的规范顺序定义掩码，依次左移1位、左移2位、左移3位等等，一直到左移62位，然后开始定义第二组，再从1、左移1位、左移2位开始，依次类推。
