---
author: 飞雪无情
comments: true
date: 2013-11-21 14:39:11+00:00
layout: post
slug: android-httpclitent-session-keep
title: Android HttpClient Session保持
wordpress_id: 200
categories:
- Android
tags:
- Android
- Http
- HttpClitent
---

现在单机版本的App已经不多了，基本上都会和服务器进行通信。Android提供Http访问的方式有两种，一种是HttpClient，一种是UrlConnection.这里要说的是HttpClient。

在Http访问中，我们可能会遇到一些这样的场景，比如服务端要求几次Http请求组成一次完整的客户端服务器通信，比如同步。现在的App很多有有账号系统，会提供把用户数据备份的功能，那么这么在线备份功能就是这样的一个场景，它包括登陆、数据上传，结果返回、提交确认或者终止等这样的顺序步骤，每一步都是一次Http访问请求。服务器的设计(负载均衡等)需要要求客户端在这依次完整的数据备份的几次Http访问请求中保持同一个Session.便于服务器的协调和操作，就和用户用浏览器打开网页去完整一件事情一样，这就需要我们在每次Http请求的时候保持Session。

其实保持HttpClient的请求Session，就是保证在每次Http请求访问的请求头信息的Cookie参数中，携带同一个JSESSIONID，只要保证每次是一样的，服务器就认为是同一Session，这就保持了Seesion会话。那么这个JSESSIONID是怎么来的呢，是服务器通过响应头信息给的，服务器在给客户端设置Cookie时，会通过响应头信息中的Set-Cookie参数传递回来，只要有新设置的Cookie，就是通过这个传递过来，如果没有则是null，那么我们就可以在请求完之后返回的信息里读取这个Set-Cookie头信息，然后保存起来，在下次请求的时候给服务器发送过去，就实现了保持Seesion的目的。代码如下：

    
    	
        //定义储存Cookie信息
        private String cookies;
    
    	public void executeGet(String url) throws ClientProtocolException, IOException{
    		HttpClient httpClient=new DefaultHttpClient();
    
    		HttpGet httpGet=new HttpGet(url);
    		setRequestCookies(httpGet);
    
    		HttpResponse response=httpClient.execute(httpGet);
    		appendCookies(response);
    	}
    	/**
    	 * 设置请求的Cookie头信息
    	 * @param reqMsg
    	 */
    	private void setRequestCookies(HttpMessage reqMsg) {
    		if(!TextUtils.isEmpty(cookies)){
    			reqMsg.setHeader("Cookie", cookies);
    		}
    	}
    	/**
    	 * 把新的Cookie头信息附加到旧的Cookie后面
    	 * 用于下次Http请求发送
    	 * @param resMsg
    	 */
    	private void appendCookies(HttpMessage resMsg) {
    		Header setCookieHeader=resMsg.getFirstHeader("Set-Cookie");
    		if (setCookieHeader != null
    				&& TextUtils.isEmpty(setCookieHeader.getValue())) {
    			String setCookie=setCookieHeader.getValue();
    			if(TextUtils.isEmpty(cookies)){
    				cookies=setCookie;
    			}else{
    				cookies=cookies+"; "+setCookie;
    			}
    		}
    	}


上面的代码就是一个简单的例子，我们在发送我们的请求之前，把现存的Cookie通过请求头信息发送给服务器，然后再服务器返回的头信息中解析，看是否有需要的更新的Cookie即可，这样我们就达到了保持Seession目的。

当我们需要重新设置Seesion的时候，请求我们保存的cookie即可，这样代码就会重新获取。如果都有个账号并发备份，那么一个账号分配一个cookie变量存储即可，也就是把上面的代码放到一个如HttpManager管理的类里，每个账号的备份都可以new个HttpManager实例，这样每个账号都可以单独处理自己的Cookie，互不干扰。


