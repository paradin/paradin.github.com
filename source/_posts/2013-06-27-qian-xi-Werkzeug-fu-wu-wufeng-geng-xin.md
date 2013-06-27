---
layout: post
title: "浅析Werkzeug服务无缝更新"
date: 2013-06-27 22:56
comments: true
categories: python 
---

一直以来对于SAE上python应用无缝更新好奇

今天就来分析一下

<!--more-->
应用启动入口
==
我用的是Flask

从dev_server.py中 WsgiWorker可以找到运行服务的入口：

		from werkzeug.serving import run_simple
        run_simple(...)

注意其中参数： ***use_reloader = True***，后面会分析到

带重启功能启动
==
use_reloader条件决定了直接启动服务还是带有自动重启功能启动

	def run_with_reloader(main_func, extra_files=None, interval=1):
    """Run the given function in an independent python interpreter."""
    # ********如果是真正需要运行的进程********
    if os.environ.get('WERKZEUG_RUN_MAIN') == 'true':
        # ********启动服务线程********
        thread.start_new_thread(main_func, ())
        try:
            # ********检测文件变化并重启********
            reloader_loop(extra_files, interval)
        except KeyboardInterrupt:
            return
    try:
        # ********第一次启动，按reloader方式启动子进程********
        sys.exit(restart_with_reloader())
    except KeyboardInterrupt:
        pass


	def restart_with_reloader():
	    """Spawn a new Python interpreter with the same arguments as this one,
	    but running the reloader thread.
	    """
	    while 1:
	        _log('info', ' * Restarting with reloader...')
	        args = [sys.executable] + sys.argv
	        new_environ = os.environ.copy()

	        # ********标记当前启动为真正要运行的进程********
	        new_environ['WERKZEUG_RUN_MAIN'] = 'true'
	
	        # a weird bug on windows. sometimes unicode strings end up in the
	        # environment and subprocess.call does not like this, encode them
	        # to latin1 and continue.
	        if os.name == 'nt':
	            for key, value in new_environ.iteritems():
	                if isinstance(value, unicode):
	                    new_environ[key] = value.encode('iso-8859-1')
	
	        # ********创建新的子进程，执行参数和当前进程一致********
	        # ********应该是直到子进程退出才会返回********
	        exit_code = subprocess.call(args, env=new_environ)
	        if exit_code != 3:
	            return exit_code

问题
==
通过以上分析，解释了我在本地起SAEPython服务时，通过日志，看到应用被初始化两次的原因

第二次启动有这么一条日志：
		
		 * Restarting with reloader...
		
这让的话，初始应用初始化过两次，而第一次初始化的应用没有用

如何解决这个问题呢？

解决应用实例化两个的问题
==

1. 创建一个启动应用
2. 由这个启动应用以自动重启方式启动真实应用

同真实模块一样创建一个启动应用模块dummy

	realapp/
	  |
	  - __init__.py
	dummy/
	  |
	  - __init__.py


index.wsgi文件这么写

		import sae, os

		if os.environ.get('WERKZEUG_RUN_MAIN') == 'true':
			print 'realy application'
			from realapp import app
			application = sae.create_wsgi_app(app)
		else:
		    print 'dummy application'
		    from dummy import dummy_app
		    application = sae.create_wsgi_app(dummy_app)

这样在本地启动SAEPython，应用创建两个实例的问题就解决了。

遗留问题
==
发布到SAE后，以上方法不生效

	os.environ.get('WERKZEUG_RUN_MAIN') != 'true'

有知道原因的吗？