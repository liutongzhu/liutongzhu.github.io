---
author: 飞雪无情
comments: true
date: 2013-11-07 14:48:52+00:00
layout: post
slug: goagent-403-not-have-permiss
title: goagent 403  not have permiss
wordpress_id: 197
categories:
- Linux
tags:
- goagent
- 代理
- 翻墙
---

今天上班，像往常一样打开Google，咦，竟然提示


<blockquote>403. That’s an error.
Your client does not have permission to get URL /2 from this server. That’s all we know.</blockquote>


奇怪了，因为我使用goagent翻墙上网，第一反应就是又被禁了。赶紧去[goagent网站](https://code.google.com/p/goagent/)去看，发现好多人反馈这个bug。看了一圈，发现有人给出了解决办法，修改goagent客户端中的proxy.ini里的profile 为 google_hk，以前的默认配置是profile = google_cn，现在改成profile = google_hk就好了。也就是换个配置。。改了后重启goagent，发现果然好了。。继续使用。



刚刚又去Goagent网站，发现发布了一则公布，这个倒没留意是什么时候发布的。公告如下



	
  1. 403 情况请改成 profile = google_hk 或者更新到最新版(不需要上传服务端也可用） [2013-11-07 19:25 更新]。

	
  2. 速度慢的话，建议重新部署并使用新的 appid


goagent是个好东西，翻墙必备，还可以看youtube视频，喜欢的朋友可以参考官网教程架设[https://code.google.com/p/goagent/wiki/InstallGuide](https://code.google.com/p/goagent/wiki/InstallGuide)
