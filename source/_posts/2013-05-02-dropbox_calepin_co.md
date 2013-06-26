---
layout: post
title: "利用DropBox建站"
date: 2013-05-02 14:10
comments: true
categories: DropBox 
---

Dropbox作为saas平台，也提供了第三方引用接入接口。

如类似calepin.co等网站可以利用dropbox建站，建站步骤：
<!--more-->
1. 通过dropbox帐号绑定
2. 在dropbox中建立独立的应用目录

	接入应用只能读取指定目录下的文件，无需担心其他资源暴露

3. 按照应用规则，在目录中增加自己的网页了
4. 任何文件变化dropbox自动同步
5. 有的应用自动从dropbox同步，有的由用户自己进行刷新

我的静态站：http://powerlly.calepin.co/

只能作为文章归档用，没有评论留言等功能。

PS: 为了能够置顶联系方式和归档页，通过将metainfo中Date标签时间写得久远些