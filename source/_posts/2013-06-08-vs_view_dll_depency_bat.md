---
layout: post
title: "查看dll依赖脚本"
date: 2013-06-08 18:10
comments: true
categories: vs 
---

软件项目逐渐庞大，版本日益更新

难免在安装包中残留一些老的dll

为了检查dll的依赖关系，可以用[dependency.exe](http://download.csdn.net/detail/powerlly/4544320)工具，然后人肉检查（略感蛋疼。。。）

还有命令行的方法
<!-- more -->
启动VS命令行：

![](https://dl.dropboxusercontent.com/u/54941343/vs_cmd.png)

定位到.dll所在目录

执行如下批处理脚本：

	set workPath="."
	for /r "%workPath%\" %%i in ("*.dll") do (dumpbin /DEPENDENTS %%i >> dep.txt)

PS： 如果是**动态加载的**dll ，这方法就查不出来了