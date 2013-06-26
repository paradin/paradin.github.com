---
layout: post
title: "mysql建库及用户命令实例"
date: 2013-05-15 14:10
comments: true
categories: mysql 
---

这里记录下mysql命令行下创建库及用户的命令
<!--more-->
	>mysql -u root -p
	
	>show databases;
	
	>create database new_database;
	
	>create user 'name' identified by 'pwd';
	
	>grant all privileges on new_database.* to 'name'@'localhost' identified by 'pwd' with grant option; 
	
	>quit
	
	>mysql -u name -p
	
	>show databases;