---
layout: post
title: 利用网络存储搭建自己的代码库
date: 2013-07-16 22:33
comments: true
categories: svn 
---

其实不只是代码，任何其他需要进行版本管理的文档等都可以利用网络存储搭建自己的版本库。

有人说，直接把文件放到网盘、云盘就可以了。当然，如果能满足需求这是最简单粗暴、但不安全的方法。

这里介绍两种版本管理库的方法：svn，git，其他版本管理软件搭建可以自行发挥一下了。

利用版本管理库的目的：

	1. 可以进行版本管理
	2. 同步到网盘的文件不是源文件，而是版本库文件
	2. 版本库读取可以设置用户密码，更加安全

注意，以下所用的快盘、Dropbox不是必须的，只要用你有的网盘、云盘都可以。
<!-- more -->
一、快盘+TorialSVN Server
--

本地目录准备

        。创建快盘目录
        。在其中创建一个svn目录（将库目录建在网盘同步目录，这样当有文件变化，就会自动同步了）

本地搭建svn服务

        。通过svn server创建一个deposite
        。创建svn用户、用户组
        。在svn depo下创建项目，配置用户权限

签出本地编辑目录

        。checkout该项目到工程目录（注意，网盘下的svn目录是版本库目录，不能直接编辑。需要签出到其他目录进行编辑，然后提交）
        。将工程中要上传的文件添加到svn ，不希望添加的文件可以添加到ignore list
        。提交

自动上传

		。提交后，版本库目录就会有相应的变化
		。网盘工具会自动将版本库上传

其他机器访问
		
		。利用网盘工具，下载版本库目录
		。利用版本管理工具TorialSVN Server，创建相应的用户，导入版本库目录
		。签出

二、把Dropbox改造为Git私有仓库（转载）
--

	作者: JeremyWei | 可以转载, 但必须以超链接形式标明文章原始出处和作者信息及版权声明
	网址: http://weizhifeng.net/git-with-dropbox.html
	Git Dropbox　

前言
--
Git作为强大的分布式版本控制工具，越来越受欢迎。大量的开源项目可以在Github上发布，不过项目是公共可见的，即人人可以fork。 对于一些用户，他们也有自己的项目，但是还不太想立刻就把项目开源出来，有可能是因为还没有完成，所以他们需要通过Git临时性地管理他们的「私有项目」，Github上虽然有私有项目托管服务，不过性价比不高。

Dropbox（墙）是最流行的云存储服务，通过Dropbox我们可以实现对Git私有项目的托管。

思路
--
我们的思路是在Dropbox客户端的目录中建立Git仓库，然后我们clone此仓库到本地仓库，然后我们进行提交操作，完成提交之后，我们执行push操作， 那么本地的数据会被push到Dropbox客户端目录的仓库中，之后Dropbox客户端会把仓库文件的更改同步到Dropbox服务器。

	+------------+            +-----------+              +---------+
	|  Dropbox   |  --Sync->  |  Dropbox  |   --Clone->  | Working |
	|   Server   |  <-Sync--  |   Client  |   <-Push---  |  Space  |
	+------------+            +-----------+              +---------+
实现
--
我们现在Dropbox的目录中创建一个裸git仓库

	$ cd ~/Dropbox
	$ mkdir git
	$ git init --bare project.git
完成之后，我们clone这个仓库

	$ cd ~
	$ git clone ~/Dropbox/project.git project
	$ cd project
提交并且push

	$ touch README
	$ git add .
	$ git commit -m "init commit"
	$ git push origin master
完成之后，Dropbox会把你push的内容同步到服务器，你通过https://www.dropbox.com/可以查看到仓库的内容。

参考
http://stackoverflow.com/questions/1960799/using-gitdropbox-together-effectively