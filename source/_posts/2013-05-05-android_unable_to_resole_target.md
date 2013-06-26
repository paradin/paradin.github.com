---
layout: post
title: "解决Unable to resolve target 'android-XX'"
date: 2013-05-05 14:10
comments: true
categories: android 
---

将低版本的代码导入eclipse时，常遇到这样的问题：

	Unable to resolve target 'android-XX'
<!--more-->
这是原代码中project.properties 的 Project target 设置与当前eclipse环境设置不一致所致。

解决这个问题:

	只要把project.properties文件用记事本打开
	将 Project target.target=android-7 改为你当前支持的AVD版本即可

	AVD(Android Virtual Device)，是Android的模拟器。
	具体介绍和命令参数参照http://apps.hi.baidu.com/share/detail/49251071


一般 android-8 对应的android sdk 是2.2， android-10对应的是2.3

参考: http://mishar-china-hotmail-com.iteye.com/blog/969178