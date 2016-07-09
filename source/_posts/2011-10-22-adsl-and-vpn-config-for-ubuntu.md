---
author: 飞雪无情
comments: true
date: 2011-10-22 16:06:17+00:00
layout: post
slug: adsl-and-vpn-config-for-ubuntu
title: Ubuntu下ADSL拨号和VPN 访问网站
wordpress_id: 48
categories:
- Linux
tags:
- Ubuntu
---

## 1.更新NetworkManager


添加更新源：deb http://ppa.launchpad.net/network-manager/trunk/ubuntu maverick main

deb-src http://ppa.launchpad.net/network-manager/trunk/ubuntu maverick main

执行sudo apt-get update更新，然后打开更新管理器进行更新软件。


## 2.启用NetworkManager。


修改/etc/NetworkManager/NetworkManager.conf文件里的managed为true保存。修改/etc/network/interfaces文件，只保留最上面两行的auto lo iface lo inet loopback,然后删除/etc/resolv.conf，最后重启networkmanager sudo /etc/init.d/network-manager restart。


## 3.添加DSL和VPN配置


看到面板里的网络链接，编辑他，添加DSL链接，之用输入用户名和密码即可，VPN链接选用pptp协议，输入服务器地址和用户名密码即可
