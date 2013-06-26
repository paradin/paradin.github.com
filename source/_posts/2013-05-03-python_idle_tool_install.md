---
layout: post
title: "colinux中安装设置idle环境"
date: 2013-05-03 14:10
comments: true
categories: python 
---

colinux 安装idle，又装了一个python2.5

无奈，强制将/usr/bin/python改成link python2.7，会造成环境不一致的问题

于是打算安装两个环境，默认的就用2.5，再安装设置python27
<!--more-->
相应的安装easy_install27, pip27, virtualenv27

因为linux是用的colinux，进行图像开发需要display device

通过在宿主机开启Xming服务：

	安装目录中X0.hosts文件中添加colinux主机ip;
	运行Xming.

在colinux中设置连接宿主机显示：

	$ export DISPLAY=192.168.0.100:0.0

启动编辑器

	$ idle
