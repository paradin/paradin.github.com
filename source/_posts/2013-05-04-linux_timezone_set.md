---
layout: post
title: "linux中时区设置"
date: 2013-05-04 14:10
comments: true
categories: linux 
---

源码安装的时候 ./configure 最后经常提示 
make: warning:  Clock skew detected.  Your build may be incomplete.

于是date，发现时区是：EDT

修改时区方法：
 /usr/share/zoneinfo/Asia/Shanghai 复制到 /etc/localtime 即可

修改后时区改为：CST