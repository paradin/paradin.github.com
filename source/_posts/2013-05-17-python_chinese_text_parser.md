---
layout: post
title: "[转载]python汉字处理的工具"
date: 2013-05-17 14:10
comments: true
categories: python 
---

下面这个小工具包含了：

	判断unicode是否是汉字，数字，英文，或者其他字符。 
	全角符号转半角符号。 
	unicode字符串归一化等工作。 
	还有一个能处理多音字的汉字转拼音的程序，还在整理中。
<!--more-->

	#!/usr/bin/env python
	# -*- coding:GBK -*- 
	"""汉字处理的工具:
	判断unicode是否是汉字，数字，英文，或者其他字符。
	全角符号转半角符号。"""
	__author__="internetsweeper <zhengbin0713@gmail.com>"
	__date__="2007-08-04"

	def is_chinese(uchar):
		"""判断一个unicode是否是汉字"""
		if uchar >= u'\u4e00' and uchar<=u'\u9fa5':
			return True
		else:
			return False

	def is_number(uchar):
		"""判断一个unicode是否是数字"""
		if uchar >= u'\u0030' and uchar<=u'\u0039':
			return True
		else:
			return False

	def is_alphabet(uchar):
		"""判断一个unicode是否是英文字母"""
		if (uchar >= u'\u0041' and uchar<=u'\u005a') or (uchar >= u'\u0061' and uchar<=u'\u007a'):
			return True
		else:
			return False

	def is_other(uchar):
		"""判断是否非汉字，数字和英文字符"""
		if not (is_chinese(uchar) or is_number(uchar) or is_alphabet(uchar)):
			return True
		else:
			return False

	def B2Q(uchar):
		"""半角转全角"""
		inside_code=ord(uchar)
		if inside_code<0x0020 or inside_code>0x7e: #不是半角字符就返回原来的字符
			return uchar
		if inside_code==0x0020: #除了空格其他的全角半角的公式为:半角=全角-0xfee0
			inside_code=0x3000
		else:
			inside_code+=0xfee0
		return unichr(inside_code)

	def Q2B(uchar):
		"""全角转半角"""
		inside_code=ord(uchar)