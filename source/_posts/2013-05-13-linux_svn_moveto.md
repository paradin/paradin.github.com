---
layout: post
title: "svn目录的拷贝方法"
date: 2013-05-13 14:10
comments: true
categories: linux 
---

svn目录中都有一个隐藏目录".svn"，这给复制带来不便。

记录下拷贝目录时过滤文件的方法：

当不需要的文件类型较为单一时
==
可以通过完全复制然后删除指定类型的文件完成

	cp -r test/ test2
	find test2/ -name '.svn' | xargs rm -rf

xargs是给命令传递参数的一个过滤器，可以将前一个命令产生的输出作为后一个命令的参数

需要的文件为单一类型
==
带目录结构复制

	mkdir test3
	find test/ -name '*.txt' | xargs tar czf test3.tgz
	tar zxvf test3.tgz -C test3

tree 命令可以查看目录树形结构