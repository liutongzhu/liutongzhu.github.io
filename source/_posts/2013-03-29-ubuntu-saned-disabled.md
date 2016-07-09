---
author: 飞雪无情
comments: true
date: 2013-03-29 04:06:37+00:00
layout: post
slug: ubuntu-saned-disabled
title: ubuntu启动错误saned disabled edit /etc/default/saned checking battery state
wordpress_id: 39
categories:
- Linux
tags:
- Ubuntu
---

昨天Ubuntu 12.04LTS 64位版本升级内核后，发现图形界面启动不了。

然后重启，按上下键查看启动信息，发现卡在了saned disabled edit /etc/default/saned。。

Goole了一把，发现是没有gnome-shell.于是就使用CTRL+ALT+F1进入tty1，通过

sudo apt-get install gnome-shell安装，然后sudo reboot重启。

发现还是进入不了图形界面，再次查看日志，发现卡在了 checking battery state

这个原因是因为gdm的问题，安装gdm

sudo apt-get install gdm

在安装过程中会有配置选择，我们选择LightDM，

然后sudo reboot即可。。

然后就如愿的进入了图形界面。。。
