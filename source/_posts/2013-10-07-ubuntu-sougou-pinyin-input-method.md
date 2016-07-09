---
author: 飞雪无情
comments: true
date: 2013-10-07 02:24:25+00:00
layout: post
slug: ubuntu-sougou-pinyin-input-method
title: Ubuntu下使用搜狗拼音输入法
wordpress_id: 175
categories:
- Linux
tags:
- Linux
- Ubuntu
---

对于经常使用Ubuntu操作系统并且不会使用五笔输入法的屌丝来说，选择一种好的拼音输入法能让你事半功倍。在window下我们都知道搜狗的输入法比较好用，更有一些用习惯了搜狗输入法的朋友想在Ubuntu等其他Linux发行版下也想使用搜狗输入法，这是完全可行的，通过Fcitx这个输入法框架就可以实现。

Fcitx [ˈfaɪtɪks] 是一个支持扩展的输入法框架。当前它支持 Linux 和 Unix 系统，例如 freebsd。它包括了三个内置的输入法引擎，拼音，区位和基于码表的输入法。详细介绍参见[fcitx-im.org](http://fcitx-im.org)。

下面以Ubuntu为例介绍如何安装Fcitx，并且配置使用搜狗输入法，其他的Linux发行版的安装可以参考Fcitx官方网站。


##  添加Fcitx源


首先添加Fcitx的ppa源，使用如下命令添加：

    
    sudo add-apt-repository ppa:fcitx-team/nightly


然后更新apt

    
    sudo apt-get update


稍等一会就会提示更新完成。


##  安装搜狗拼音输入法


源更新完成后就可以安装搜狗输入法了，使用如下命令安装：

    
    sudo apt-get install fcitx-sogoupinyin


在终端中输入以上命令完后回车就开始下载安装包进行安装了，根据你的网速不同所需的事件也不太一样，一般几分钟就好了，稍等一下。安装完成后就可以切换到我们刚刚安装的fcitx输入法，然后卸载原来使用的ibus输入法：

    
    #切换到Fcitx输入法
    im-switch -s fcitx
    #卸载原来的ibus输入法
    sudo apt-get remove ibus


然后重启电脑即可。重启后就可以看到搜狗输入法了，如果没有看到可以使用如下命令重启fcitx:

    
    sudo fcitx -r


除此之外，你也可以安装google拼音输入法，步骤和搜狗的差不多，安装后可以在右上角的输入法管理那里切换输入法，也可以选择皮肤等等，fcitx是一个强大的输入法框架，有很多功能，自己可以尝试一下。
