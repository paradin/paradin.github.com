---
layout: post
title: "octopress+github 建站"
date: 2013-06-25 14:10
comments: true
categories: octopress github 
---

建站的过程主要参考：

1. [用octopress来写博客](http://caok1231.com/blog/2012/06/24/install-octopress-to-write-blog) - 常见说明
2. [搭Blog 学Git](http://shanewfx.github.io/blog/2012/02/16/bulid-blog-by-octopress/) - 中文Win XP下环境设置

多余的我就不说了，主要说下我遇到的问题：

1. rebenv 可以不用，查看ruby版本号命令：ruby -v

2. 不用rbenv的话，rbenv rehash 就不用执行了

3. 创建github库一定要按照参考文章创建，名称：yourname.github.com , yourname必须是你的github帐号名

4. 如果对git命令不熟悉，可以学下，快速学习可参考：https://na1.salesforce.com/help/doc/en/salesforce_git_developer_cheatsheet.pdf

5. 中文系统下 rake generate，时提示错误，通过增加两个环境变量解决：	
	
		LC_ALL=zh_CN.UTF-8
		LANG=zh_CN.UTF-8
