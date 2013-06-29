---
layout: post
title: "Python中调用父类的同名方法"
date: 2013-06-08 18:10
comments: true
categories: python 
---

面向对象设计时，无可避免的会涉及到父类和子类的关系

封装、集成、多态，大家都能娓娓道来

道理是一样的，针对不同的语言，面向对象开发也会遇到不同情况需要解决

今天学习下python中如何调用父类同名方法
<!-- more -->
PS： 如果不调用的话，子类同名方法对父类方法是直接覆盖的

python 2.2以前
--

	class FooParent:
        def bar(self, message):
            print(message)

	class FooChild(FooParent):
        def bar(self, message):
            FooParent.bar(self, message)

	>>> FooChild().bar("Hello, World.")
	Hello, World.

缺点：

- 要修改父类名称时，麻烦

- 多级、多继承时，基类方法会多次执行

2.2 以后
--
	class FooParent:
        def bar(self, message):
            print(message)

        
	class FooChild(FooParent):
        def bar(self, message):
            super(FooChild, self).bar(message)

        
	>>> FooChild().bar("Hello, World.")
	Hello, World.

优点：

- 基类名称想怎么改就怎么改了
- 多继承，不会多次调用基类方法


参考：[如何在Python中调用父类的同名方法](http://hi.baidu.com/thinkinginlamp/item/3095e2f52c642516ce9f32d5)