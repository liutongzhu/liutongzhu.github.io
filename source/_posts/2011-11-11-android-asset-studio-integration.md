---
author: 飞雪无情
comments: true
date: 2011-11-11 16:18:00+00:00
layout: post
slug: android-asset-studio-integration
title: Android设计应用图标不用愁---Asset Studio Integration来帮你
wordpress_id: 47
categories:
- Android
tags:
- Android
---

[Android Asset Studio](http://android-ui-utils.googlecode.com/hg/asset-studio/dist/index.html)Web版是一个非常好用的Andrid图标资源设计工具，它是由RomanNurik开发的。现在我们已经在ADT14里支持同样的功能。首先，你要打开新建向导（Ctrl-N）,选择Android分类，然后在其下找到名字为“Android Icon Set”的这一项：

![](http://hi.csdn.net/attachment/201111/10/0_13209351362229.gif)

然后，将会弹出“资源选择器”（这些资源其实就是你在布局文件用到的那些图片，比如对于上下文菜单自定义的背景图片，就需要一个为“Background”属性设置一个@drawable的引用）对话框，该对话框里有个按钮(Create New Icon...)，点击它.

![](http://hi.csdn.net/attachment/201111/10/0_1320936348j75Y.gif)

当你打开Android Icon Set向导的时候，首先看到的是一个资源类型选择界面，也就是你想创建哪一类图标：

![](http://hi.csdn.net/attachment/201111/10/0_1320936495Wu61.gif)

在这里，你可以选择创建哪一类图标，并且为这个被创建的图标起一个名字。

如果你选择创建一个启动图标并且单击了下一步，这时你会看到如下界面：

![](http://hi.csdn.net/attachment/201111/10/0_1320936654FoRB.gif)

这个界面包含很多功能。首先你会在右边看到我们正在创建和处理的图标的预览图。正如你所看到的，针对不同分辨率的屏幕它创建了多个图标，这就是刚刚为什么我们指的是一个“图标集合”，而不仅仅是一个“图标”。除了上面显示的，我们还创建了一个高分辨率的“web”版本的图标，你可能会在上传应用到Android Market的时候用到它（译者按：在上传App倒Android
Market的时候，可以上传一个大图标）。

![](http://hi.csdn.net/attachment/201111/10/0_1320937517H8eb.gif)

仔细看下这个图标，你会发现它比我们在上面预览（Preview）那里看到的更有光泽。你可以选择界面上的“Glossy”选项来使得你的图标具有光泽的效果。

![](http://hi.csdn.net/attachment/201111/10/0_1320937955mwIb.gif)

你还可以设置背景的形状。文本的字体、颜色等等。

![](http://hi.csdn.net/attachment/201111/10/0_1320938081E0sR.gif)

然而，使用文本并不能做出非常好的图标，但是请注意，这仅仅是我们为了得到初步的效果而采取的一种很快捷的方式。请注意界面的最上面还有三个功能选项：





	
  * 图片

	
  * 剪贴画

	
  * 文本




理想情况下你会用你自己的“基准”图像（或者如果需要的话会是一个剪贴画），然后使用Asset Studio 向导生成合适的图标。




你会问了：“我已经有了一个自己绘制好的图标了，那么我为什么还要再生成”？原因就是Anroid有不同的版本、不同的主题，这就意味着你会提供不同效果、不同颜色以及不同样式的图标。比如下面我为状态栏和通知栏生成的图标：




![](http://hi.csdn.net/attachment/201111/10/0_1320939254gexG.gif)




你可以看到使用的基准图像只是一个简单的黑白相间的Bugdroid图像，但是我们为Gingerbread(v9)、Honeycomb(v11)以及老的版本都生成了不同样式的图标。对于ActionBar，Asset Studio向导会生成亮、暗两种不同Holo 主题的图标。对于选项卡则会生成“选中”和“未选中”两种不同样式的图标，并且对于v5（Andoid2.0）之前的版本则会新生成不同演示的“未选中”状态的图标（译者按：Asset
Studio对于v5版本前后生成的“为选中”图标是不一样的，从下图可以看出，v5之前的图标发白，v5之后的发灰）。




![](http://hi.csdn.net/attachment/201111/10/0_1320940269VMv2.gif)







使用Asset Studio的一个优势就是在你点击完成按钮后，它可以直接在你当前的项目中生成你设计好的图标。这样你就不用像使用Web版本（译者按：前面提到过，这个工具一开始是Web App，ADT14才加入进来，成为ADT的一部分）一样，还要下载下来并且提取图标放在正确的位置。它还会在图标名字冲突的时候提示你是否覆盖原文件。如果你在上面的资源选择器界面里点击了“Create
Icon”按钮，那么新创建的图标将会在选择器里被选中。







当你点击完成退出向导的时候，包资源管理器会自动更新并且选中新创建的图标。




![](http://hi.csdn.net/attachment/201111/11/0_1321025460X3wc.gif)




和ADT集成的第二个优势就是我们可以知道你的项目工程的minimum SDK target，我们就可以利用这些信息仅仅生成必要的图标。比如你正在基于Android2.0或者以上版本开发一个App，那么我们将不会生成不需要的老的样式的图标，也不需要名字为drawable-mdpi-v5的文件夹等等；生成的图标直接会被放进drawable-mdpi文件夹里。







我们希望这些内容能让你更容易的创建很好的应用。请不要经常使用那个文本功能:（译者按：最好使用Image和Clipart两个）-)我们还仍需要为AOSP贡献更多的剪贴画，这样大家就可以在向导里选择使用（译者按：目前向导的剪贴画只有一个，可以参见前面的图片），所以如果你是个艺术设计者，我们非常欢迎你贡献些剪贴画出来！（请参见[贡献页面](http://tools.android.com/contributing)）







非常感谢Roman Nurik写出了这么好的网页应用并且为移植到ADT提供了帮助。







译自：[http://tools.android.com/recent/assetstudiointegration](http://tools.android.com/recent/assetstudiointegration)
