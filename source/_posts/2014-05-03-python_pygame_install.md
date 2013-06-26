---
layout: post
title: "colinux中安装pygame"
date: 2013-05-03 14:10
comments: true
categories: python 
---

pygame是封装sdl的基于python语言的跨平台图像开发库

与pygame相对的还有pyglet，是基于opengl的跨平台python语言开发库
<!--more-->
安装pygame时需要 sdl-config

download and install SDL
==

>wget http://www.libsdl.org/release/SDL-1.2.14.tar.gz
>tar -xzvf SDL-1.2.14.tar.gz
>cd SDL-1.2.14
>./configure --prefix=/usr/local/SDL
>make
>make install

download and install PyGame
==

>wget http://pygame.seul.org/ftp/pygame-1.9.1release.tar.gz
>tar xzvf pygame-1.9.1release.tar.gz
>cd pygame-1.9.1release

\# Here you need to edit the Setup file and comment out the line that looks like
\# scrap src/scrap.c $(SDL) $(SCRAP) $(DEBUG)
\# for some reason the compilation of the camera module fails for me, so I commented it out
>vi Setup

\# once the camera module is out of the way we can proceed with the installation
\# Make sure to use the correct Python version here (e.g. 'python2.5' or 'python2.6')
>python27 setup.py install --prefix=/usr/local/pygame

That's it
==