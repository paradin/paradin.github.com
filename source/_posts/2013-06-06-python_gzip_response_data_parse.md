---
layout: post
title: "解析网页抓取所得gzip压缩数据"
date: 2013-06-06 14:10
comments: true
categories: python
---

在进行网页数据抓取时，会遇到压缩数据，可能主要出于如下考虑：

1. 简单的防抓取方法
	
	没有经过分析就对抓取的网页内容直接进行文本解析，肯定会遇到问题，就像我开始的时候一样。。。

2. 压缩数据减少流量

	这个一般用于提供数据接口，将数据压缩后可以大大减少流量

以下提供对抓取数据的处理方法：
<!-- more -->
1. 检测反馈内容是否压缩，并对压缩内容解压

2. 检测数据文本编码格式，解压后返回unicode文本

检测编码格式所用工具：[chardet](https://pypi.python.org/pypi/chardet)

	# -*- coding: utf-8 -*-
	
	import gzip, StringIO, codecs, chardet
	from utils import *
	
	def getEncoding(txt):
	    """ 检查文本内容的编码方式
	    """
	    try:
	    	coding = chardet.detect(txt)['encoding']
	    	
	    	if 'None' == coding:
	    	    return None
	    	else:
	    	    return coding
	    	    
	    except Exception as ex:
	    	print 'fail getEncoding {0}'.format(ex)
	    	return None
	
	def readResponse(resp):
	    """ 读取网页抓取返回内容
	    """
	    if resp.getcode() == 200:
	    	encoding = resp.headers.get('Content-Encoding')
	    	
	    	info = resp.read()
	    	
	    	if('gzip' == encoding):
	            print 'read response compress gzip'
	    	    try:
	    	    	gf = gzip.GzipFile(fileobj=StringIO.StringIO(info), mode="r")
	    	    	info = gf.read()
	    	    except Exception as ex:
	    	    	print 'fail read response, gzip'
	    	    	info = ''
	
	    if info!='':
	        # 删除BOM头
	        info = info.strip(codecs.BOM_UTF8)
	        code = getEncoding(info)
	        if code:
	            info = info.decode(code)
	            print 'read response, encoding {0}'.format(code)
	        else:
	            print 'fail parse text encode'
	
	    return info

参考链接：

1. [Handling compressed data](http://www.diveintopython.net/http_web_services/gzip_compression.html)
2. [关于 Content-Encoding: gzip](http://blog.knownsec.com/2012/04/about-content-encoding-gzip/)