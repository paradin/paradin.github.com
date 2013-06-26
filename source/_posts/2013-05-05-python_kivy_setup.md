---
layout: post
title: "致没有搞好的kivy环境"
date: 2013-05-05 14:10
comments: true
categories: python 
---

        继续研究kivy，上次没搞通是环境安装没装好

        按照说明安装需要的包，遇到问题：
        。 要求安装python-dev，但是发现版本是2.5，没装
        。 要安装libgles2 ，找不到， 搜了下这个包， 在apt源增加一个地址
        。 用aptitude -f install 进行安装后，pip安装kivy，发现把gcc 给删了，然后环境烂了，经常提示 package broken
        。 尝试过apt-get clean    apt-get autoremove 都无法解决
        。 查看pip.log 发现标准库都不存在了， stdio.h都找不到

        怀疑环境安装还是有问题
        。 有些库提示无用可以删除，但是看要安装的库对其又有依赖