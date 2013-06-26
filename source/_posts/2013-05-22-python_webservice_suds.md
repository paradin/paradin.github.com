---
layout: post
title: "python webservice调用-suds"
date: 2013-05-22 14:10
comments: true
categories: python 
---

python也可以调用webservice : suds
<!--more-->
在本地sae中可以运行
        
		from suds.client import Client
        url =  "http://host:port/service.wsdl"  
        
		#根据wsdl创建一个WebService的Client  
        client = Client(url)
        
		# 查看webservice提供的服务
        print client
        
		# 调用接口
        client.service.Interface(...)
        
		# 构造复杂数据
        monitor1 = client.factory.create('MonitorEntry')

但是上传sae后，运行失败
        
	。suds需要写临时文件，应该是wsdl文件
    。找了个解决办法：
    	在suds/cache.py中用 
	。但是sae声称 get_tmp_dir 暂不支持