---
layout: post
title: "colinux中实现声音播放"
date: 2013-05-04 14:10
comments: true
categories: colinux 
---

因为window占用着声卡,colinux是没法占用了,只能通过网络间接使用:

	在colinux安装虚拟声卡；
	通过网络传输声音数据到宿主机；
	在宿主机播放声音。
<!--more-->
找到了pulseaudio这个解决方案，具体安装介绍就不累赘了，这里只说明下安装过程中遇到的问题及解决方法。

window安装pulseaudio服务，运行pulseaudio后，提示错误

	W: pulsecore/random.c: failed to get proper entropy. Falling back to seeding with current time.
	W: pulsecore/core-util.c: secure directory creation not supported on Win32.
	E: pulsecore/pid.c: stale PID file, overwriting.
	W: pulsecore/core.c: failed to allocate shared memory pool. Falling back to a normal memory pool.	Unable to convert, filtering
	E: pulsecore/socket-server.c: socket(PF_INET6): Invalid argumentUnable to convert, filtering
	E: pulsecore/socket-server.c: socket(PF_INET6): Invalid argumentUnable to convert, filtering
	E: pulsecore/socket-server.c: socket(PF_INET6): Invalid argumentUnable to convert, filtering
	E: pulsecore/socket-server.c: socket(PF_INET6): Invalid argument

在网上找了一通，都没有解决，有人说ipv6应该关掉

我看了下网络连接，发现没有安装ipv6，于是尝试安装了一下，结果居然好了

成功在colinux上播放wav文件

尽管还有提示
	W: pulsecore/random.c: failed to get proper entropy. Falling back to seeding with current time.
	W: pulsecore/core-util.c: secure directory creation not supported on Win32.
	W: pulsecore/core.c: failed to allocate shared memory pool. Falling back to a normal memory pool.

因为之前更换过default.pa配置，不确定是否有关，于是切换回原配置，发现有问题

说明配置也是要更换的
	
	// default.pa

	# Load audio drivers automatically on access
	
	#add-autoload-sink output module-waveout sink_name=output source_name=input
	#add-autoload-source input module-waveout sink_name=output source_name=input
	
	# Load several protocols
	#load-module module-esound-protocol-tcp
	#load-module module-native-protocol-tcp
	#load-module module-simple-protocol-tcp
	#load-module module-cli-protocol-tcp
	
	load-module module-esound-protocol-tcp auth-anonymous=1
	load-module module-native-protocol-tcp auth-anonymous=1
	load-module module-cli-protocol-tcp
	load-module module-http-protocol-tcp
	
	### Automatically restore the volume of playback streams
	load-module module-volume-restore
	
	### Automatically move streams to the default sink if the sink they are
	### connected to dies, similar for sources
	load-module module-rescue-streams
	
	# Make some devices default
	set-default-sink output
	set-default-source input
	
	.nofail
	
	# Load something to the sample cache
	load-sample x11-bell %WINDIR%\Media\ding.wav
	load-sample-dir-lazy C:\WINDOWS\Media

现在起colinux，还要起Xming、PulseAudio，分别打开挺麻烦的，colinux早就为我们想好了

在colinux的.conf配置文件最后，可以设置启动命令，来启动所要的服务，如：

	# Run an application on colinux start (Sample Xming, a Xserver)
	exec0="D:\Program Files\Xming\Xming.exe",":0 -clipboard -multiwindow -ac"
	exec1="D:\Program Files\PulseAudio\pulseaudio.exe"

注意：

	要增加启动进程的话，exec后序号递增；
	=后的参数以逗号分隔，第一个是exe路径，后面是启动参数。包含空格的参数最好用"包含