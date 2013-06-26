---
layout: post
title: "python 字符串编码"
date: 2013-05-30 14:10
comments: true
categories: python 
---

在pythong开发过程中经常遇到编码问题，原因在于没有正确理解编码解码过程

通过以下命令过程帮助理解下，各自体会吧。
<!--more-->
宗旨：**由Unicode中转进行编码、解码**

	
	>>> u = '\uffef'
	>>> print u
	\uffef
	>>> u = u'\uffef'
	>>> print u
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	UnicodeEncodeError: 'gbk' codec can't encode character u'\uffef' in position 0: illegal multibyte sequence
	>>> str(u)
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	UnicodeEncodeError: 'ascii' codec can't encode character u'\uffef' in position 0: ordinal not in range(128)
	>>> sys.stdout.encoding
	'cp936'
	>>> x = u.encode('utf-8')
	>>> x
	'\xef\xbf\xaf'
	>>> x.decode('utf-8')
	u'\uffef'
	>>> x.encode('utf-8')
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	UnicodeDecodeError: 'ascii' codec can't decode byte 0xef in position 0: ordinal not in range(128)
	>>> u.decode('utf-8')
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	  File "E:\Program Files\Python27\lib\encodings\utf_8.py", line 16, in decode
	    return codecs.utf_8_decode(input, errors, True)
	UnicodeEncodeError: 'ascii' codec can't encode character u'\uffef' in position 0: ordinal not in range(128)
	>>> u.encode('utf-8')
	'\xef\xbf\xaf'
	
	>>> print x
	锟
	>>> str(x)
	'\xef\xbf\xaf'
	>>> repr(x)
	"'\\xef\\xbf\\xaf'"
	>>> repr(u)
	"u'\\uffef'"
	>>> repr(str)
	"<type 'str'>"
	>>> repr(unicode)
	"<type 'unicode'>"
	
	>>> unicode(x)
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	UnicodeDecodeError: 'ascii' codec can't decode byte 0xef in position 0: ordinal not in range(128)
	>>> unicode(u)
	u'\uffef'
	>>> isinstance(u, str)
	False
	>>> isinstance(u, unicode)
	True
	>>> isinstance(x, str)
	True
	>>> isinstance(x, unicode)
	False
	
	>>> import re
	>>> re.search(u'\uff', u)
	  File "<stdin>", line 1
	SyntaxError: (unicode error) 'unicodeescape' codec can't decode bytes in position 0-XXX escape
	>>> re.search(u'ff', u)
	>>> re.search(r'\uff', u)
	>>> re.search(ur'\uff', u)
	  File "<stdin>", line 1
	SyntaxError: (unicode error) 'rawunicodeescape' codec can't decode bytes in position\uXXXX
	
	
	
	>>> len(None)
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	TypeError: object of type 'NoneType' has no len()
	>>> len('aaa')
	3
	>>> len([])
	0
	>>> len(())
	0
	>>> len({})
	0