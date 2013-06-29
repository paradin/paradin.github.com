---
layout: post
title: "python程序监视自身退出"
date: 2013-06-27 14:10
comments: true
categories: python
---

SAE上更新代码时，服务都可以无缝切换，无需人工重启服务

那么，怎么能够在服务重启前进行持久化操作呢

需要监视服务何时重启

python提供了一个绑定程序退出时处理函数的功能[[参考](http://docs.python.org/2/library/atexit.html)]：

	atexit.register(func[, *args[, **kargs]])
<!-- more -->
程序结束，如调用sys.exit() 或者主模块执行结束时，就会执行注册的clean functions


	def goodbye():
	    print 'Bye...'
	
	atexit.register(goodbye)

需要注意的是，可以重复注册相同的clean functions，这可能是我们不希望的，以下方法可以防止重复注册：

	def goodbye():
	    print 'Bye...'
	
	for idx, handler in enumerate(atexit._exithandlers):
	    if handler[0].func_name == 'goodbye':
	        del atexit._exithandlers[idx]
			break
	
	atexit.register(goodbye)
	print atexit._exithandlers



