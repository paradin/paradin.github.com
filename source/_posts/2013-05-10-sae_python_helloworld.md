---
layout: post
title: "hello sae python"
date: 2013-05-10 14:10
comments: true
categories: sae 
---

看看如何在sae上部署第一个python应用吧
<!--more-->
首先在sae上创建一个python应用

创建好后其实是没有完成创建的，无法进行代码管理

需要先svn签出、签入

SAE采用svn来作为代码部署工具

1. 检出应用helloworld目录

		svn co https://svn.sinaapp.com/helloworld

2. 创建版本目录
	
	进入helloworld目录，创建一个目录1作为默认版本，切换到目录1。

		cd helloworld
		~/helloworld$ mkdir 1
		~/helloworld$ cd 1

3. 创建应用配置文件config.yaml，内容如下：

		name: helloworld
		version: 1

4. 创建index.wsgi，内容如下：

		import sae

		def app(environ, start_response):
		    status = '200 OK'
		    response_headers = [('Content-type', 'text/plain')]
		    start_response(status, response_headers)
		    return ['Hello, world!']

		application = sae.create_wsgi_app(app)

5. 部署应用

	提交刚刚编辑的代码，就可以完成应用在SAE上的部署。

		svn add 1/
		~/helloworld$ svn ci -m "initialize project"