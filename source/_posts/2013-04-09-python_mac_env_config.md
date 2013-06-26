---
layout: post
title: "Mac上安装python环境"
date: 2013-04-09 15:10
comments: true
categories: python 
---

在Mac上从安装环境配置工具开始，一步一步安装配置python环境。
<!--more-->
1. 安装homebrew
-
	$ ruby -e "$(curl -fsSkL raw.github.com/mxcl/homebrew/go)"

2. 配置环境变量
-
~/.bashrc

	$ export PATH=/usr/local/bin:$PATH

3. 安装python
-
	$ brew install python --framework

	$ export PATH=/usr/local/share/python:$PATH

问题：
-
即使安装以上方法设置了环境变量，但是还是不起作用

解决：

	$ sudo vim /etc/paths

将路径添加到里面去， 一行一个路径  【重点】
  
注意：

即便添加成功，未必运行成功；在指定路径下的脚本必须是executable, 否则就会被在搜索时被忽略。

	$ sudo chmod +x XXX

而在Ubuntu下，则只需要修改/etc/.profile或者 ~/.profile或~/.bashrc


4. 安装pip
-
Homebrew already installed Distribute for you
安装pip替换Distribute的easy_install功能 

	$ easy_install pip

5. 安装virtualenv
-
	$ pip install virtualenv

6. 创建独立python环境
-
	$ virtualenv --distribute venv

会在执行目录生成venv

7. 激活运行环境
-
To use an environment, run 

	$ source venv/bin/activate

8. 关闭运行环境
-
Once you have finished working in the current virtual environment, run **deactivate** to restore your settings to normal.

9. 创建工作目录
-
一切准备就绪，在venv的同级目录创建自己的python工作目录就可以了
venv也带着pip，因此也可以在环境中安装需要的库