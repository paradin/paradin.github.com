---
layout: post
title: python生成验证码图片
date: 2013-07-18 22:33
comments: true
categories: python
---

利用Captcha生成文字图片，可以用于简单验证码图片生成
<!-- more -->
	# -*- coding: utf-8 -*- 
	from Captcha.Visual import Text, Backgrounds, Distortions, ImageCaptcha 
	from Captcha import Words 
	
	myFontFactory = Text.FontFactory((50, 30), "ch") 
	
	class myCaptcha(ImageCaptcha): 
	  def getLayers(self, *args, **kwargs): 
	    if kwargs.has_key('text'): 
	      word = kwargs['text'] 
	    else: 
	      word = Words.defaultWordList.pick() 
	    self.addSolution(word) 
	    return [ 
	      Backgrounds.Grid(size=7, foreground="gray"), 
	      Backgrounds.RandomDots(dotSize=3, numDots=500), 
	      Text.TextLayer(word, textColor='red' , fontFactory=myFontFactory), 
	      Distortions.SineWarp(), 
	    ] 
	
	g = myCaptcha(text=u'測試一二三') 
	i = g.render(size=(300,100)) 
	i.save("chcaptcha.jpg")