---
layout: post
title: "sae python 微信公众项目单元测试框架"
date: 2013-05-13 14:10
comments: true
categories: sae python
---

开发的伴侣就是单元测试，没有测试用例的开发，或者说是没有自动测试的开发是噩梦般的。

在sae python微信公众项目的开发过程中，建立单元测试框架可以大大提高开发效性、项目的稳定性。
<!--more-->
下面提供我自己使用的简单sae python测试框架(weixin_unitest.py)：

	﻿# -*- coding: utf-8 -*-
	#/usr/bin/env python
	
		
	import sys, urllib, httplib, time, hashlib, random
	
	# 配置
	interface_url = '***.sinaapp.com:80' # 注意不能加http://
	interface_path = '/your path'
	Token = 'your token'
	
	messages = {
	    # 用户关注消息
	    'subscribe' : '''<xml><ToUserName><![CDATA[your name]]></ToUserName>
	    <FromUserName><![CDATA[tester name]]></FromUserName>
	    <CreateTime>123456789</CreateTime>
	    <MsgType><![CDATA[event]]></MsgType>
	    <Event><![CDATA[subscribe]]></Event>
	    <EventKey><![CDATA[EVENTKEY]]></EventKey>
	    </xml>''',
	
	    # 用户发送文本信息
	    'text': '''<xml><ToUserName><![CDATA[your name]]></ToUserName>
	    <FromUserName><![CDATA[test name]]></FromUserName> 
	    <CreateTime>1348831860</CreateTime>
	    <MsgType><![CDATA[text]]></MsgType>
	    <Content><![CDATA[test text]]></Content>
	    <MsgId>1234567890123456</MsgId>
	    </xml>'''
	
	}
	
	def make_post(action):
	    '''模拟用户行为产生的消息提交给接口程序'''
	
	    conn = httplib.HTTPConnection(interface_url)
	
	    headers = { "Content-type": "text/xml",
	                "Content-Length": "%d" % len(messages[action])}
	
	    # 生成签名相关变量
	    timestamp = int(time.time())
	
	    nonce = random.randint(1,100000)
	
	    signature = makeSignature(Token, timestamp, nonce)
	
	    params = urllib.urlencode({'signature': signature, 'timestamp': timestamp, 'nonce': nonce})
	
	    conn.request("POST", interface_path + "?" +params, "", headers)
	
	    print messages[action]
	    conn.send(messages[action])
	
	    response = conn.getresponse()
	
	    print response.status, response.reason
	     
	    print response.read()
	
	    conn.close()
	
	    
	    
	def makeSignature(Token, timestamp, nonce):
	    '''生成签名'''
	    try:
	        Token = int(Token)
	    except Exception, e:
	        pass
	
	    sorted_arr = map(str, sorted([Token, timestamp, nonce]))
	
	    sha1obj = hashlib.sha1()
	    sha1obj.update(''.join(sorted_arr))
	    hash = sha1obj.hexdigest()
	
	    return hash
	
	def listAction():
	    print("======Supported actions:======")
	    for i in messages.keys():
	        print(i)
	    print("==============================")
	
	if __name__ == '__main__':
	    if len(sys.argv) < 2:   
	        print (u"Please input your action")
	        listAction()
	    else:
	        if (messages.has_key(sys.argv[1])):
	            make_post(sys.argv[1])
	        else:
	            print("No this action")
	            listAction()

运行：

	python weixin_unitest.py subscribe
	python weixin_unitest.py text

结合SAEPython（sae服务器），在本地完成开发测试就很方便了。

广告
=
关注易生活，关注微信公众：EasyTool