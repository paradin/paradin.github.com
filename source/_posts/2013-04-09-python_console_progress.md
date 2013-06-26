---
layout: post
title: "命令行中进度条展示的方法"
date: 2013-04-09 14:10
comments: true
categories: python 
---

三种命令行进度条展示的方法:
<!--more-->

1. 每次输出不换行，而是到行首重新输出

        '\r'
        sys.stdout.write('.' * i + '->\r')
        sys.stdout.flush()

2. 通过退格追加

        '\b'
        sys.stdout.write('.' + '->' + '\b\b')
        sys.stdout.flush()

3. 通过第三方库

我希望同时多行进行演示，类似比赛进度

方法：每次刷屏，重新输出每行进度

        os.system('clr' if os.name=='nt' else 'clear')