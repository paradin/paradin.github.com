---
layout: post
title: "python日期字符串解析"
date: 2013-05-13 14:10
comments: true
categories: python 
---

用python + 正则表达式 实现的日期字符串解析工具：
<!--more-->
	# -*- coding: utf-8 -*-
	#
	# 关注易生活，我是小E
	# 微信：EasyTool
	#
	import re
	from datetime import date
	
	
	DATE_PATTERNS = {
	u'2013-05-16':u'^(\d{4})[\-\/\.年](\d{1,2})[\-\/\.月](\d{1,2})日{,1}',
	u'20130516':u'^(\d{4})(\d{1,2})(\d{1,2})',
	u'2013-05':u'^(\d{4})[\-\/\.年](\d{1,2})[\-\/\.月]{,1}()',
	u'2013':u'^(\d{4})[\-\/\.年]{,1}()()',
	u'05月':u'^()(\d{1,2})[月]()',
	u'05-16':u'^()(\d{1,2})[\-\/\.月](\d{1,2})日{,1}'
	}
	
	DATE_PATTERNS_TEST = [
	u'2013-05-16',
	u'2013年05月16',
	u'2013年05月16日',
	
	u'20130516',
	
	u'2013-05', 
	u'2013年05', 
	u'2013-05月',
	
	u'2013',
	u'2013年',
	
	u'05月',
	
	u'05-16',
	u'05月16',
	u'05月16日',
	
	u'05'
	]
	
	
	LANG_PRINT = 'utf-8'
	
	def parse_date(dt_str):
	    year, month, day = u'', u'', u''
	
	    for pat_key in DATE_PATTERNS.keys():
	        dt_arrs = re.findall(DATE_PATTERNS[pat_key], dt_str)
	        if len(dt_arrs)>0:
	            dt_arr = dt_arrs[0]
	            if len(dt_arr)>0: year  = dt_arr[0]
	            if len(dt_arr)>1: month = dt_arr[1]
	            if len(dt_arr)>2: day   = dt_arr[2]
	            break
	
	    return year, month, day
	
	def test_all():
	    for dt_str in DATE_PATTERNS_TEST:
	        dt_tuple = parse_date(dt_str)
	        if dt_tuple is not None:
	            print u'dt_str {0} parse result: {1} {2} {3}'.format(dt_str, dt_tuple[0], dt_tuple[1], dt_tuple[2]).encode(LANG_PRINT)
	        else:
	            print u'error date format {0}'.format(dt_str).encode(LANG_PRINT)
	
	
	if __name__ == '__main__':
	    test_all()

期间遇到问题：
==
print 内容需要是utf-8格式，否则会异常

print是标准输出sys.stdout

查看标准输出的编码：  print sys.stdout.encoding
