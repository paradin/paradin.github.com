---
layout: post
title: "通过adb连接手机或者模拟器进行python开发"
date: 2013-05-05 14:10
comments: true
categories: python 
---

adb remote control
==

sl4a 可以开启python-server，通过远程adb进行开发、调试，然后发布到手机
<!--more-->
在pc上通过adb, 连接手机上的python-server

	public server开启不了
	只能usb开始private server，但是连着usb后，手机自动卸载sdcard

	期间一直找不到设备，后来发现是usb驱动没装好


usb连接手机，开启模拟器
==

1. 查看当前连接的手机或模拟器

		c:/>adb devices
		List of devices attached
		015EF45B0D01200F        device
		emulator-5554   device

2. 连接指定机器安装 sl4a，已经下载 sl4a_r6.apk

		c:/>adb -s <name> install sl4a_r6.apk

3. 进入机器shell

		c:/>adb -s <name> shell
		$

4. 开启private server
	
	手机的话su切换到高级用户

		$ su  
		#



	Start a private server.

		# am start -a com.googlecode.android_scripting.action.LAUNCH_SERVER -n com.googlecode.android_scripting/.activity.ScriptingLayerServiceLauncher

	Start a private server on a particular port (in this case, 45001)
		
		# am start -a com.googlecode.android_scripting.action.LAUNCH_SERVER -n com.googlecode.android_scripting/.activity.ScriptingLayerServiceLauncher --ei com.googlecode.android_scripting.extra.USE_SERVICE_PORT 45001

	Start a public server.

		# am start -a com.googlecode.android_scripting.action.LAUNCH_SERVER -n com.googlecode.android_scripting/.activity.ScriptingLayerServiceLauncher --ez com.googlecode.android_scripting.extra.USE_PUBLIC_IP true

5. 退出shell

		# exit
		$ exit
		c:/>

6. 连接手机/模拟器python-server
		
		c:/> adb -s <name> forward tcp:9999 tcp:45001
		c:/> set AP_PORT=9999

7. 开始编写python代码

		c:/> edit hello.py
		import android
		droid=android.Android()
		droid.makeToast("hello")

8. 执行python

		c:/> python hello.py

模拟器加载sdcard
==

1. 创建sdcard文件

		mksdcard -l e 512M mysdcard.img

2. 带sdcard启动avd

		emulator -avd QVGA_2.7 -sdcard mysdcard.img

3. 上传文件

		adb -s emulator-5554 push proxy.py /mnt/sdcard/sl4a/scripts

启动模拟器脚本(start avd.bat)
==
	emulator -avd QVGA_2.7 

建立adb连接脚本(start adb.bat)
==
	adb devices
	
	adb -s emulator-5554 shell am start -a com.googlecode.android_scripting.action.LAUNCH_SERVER -n com.googlecode.android_scripting/.activity.ScriptingLayerServiceLauncher --ei com.googlecode.android_scripting.extra.USE_SERVICE_PORT 45001
	
	adb -s emulator-5554 forward tcp:9999 tcp:45001
	set AP_PORT=9999
	
	
	start echo Adb connected!