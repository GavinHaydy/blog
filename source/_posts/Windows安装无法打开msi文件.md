---
title: "Windows安装无法打开msi文件"
date: 2022-04-21T05:06:35.000Z
tags: [windows]
categories: [问题处理,windows]
---
@[TOC]
# 前言	
	网上搜索发现几乎都是让修改注册表的，经过测试没有用
# 解决方法
### 检查服务是否已启动
	Win+R  输入services.msc 
	或者打开任务管理器

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/cb08560c5ed76148804502d62bbb8da9.png)

### 找到windows Install
 	点启动  如果已启动状态，先禁用再启动 
 	好了，接下来就是安装了
 
 ### 安装msi文件
  打开cmd  
  输入以下命令
```shell
msiexec /package   "xxx.msi"   # xxx为你得msi文件名 此处需要写全路径  引号为半角(英文)双引号
# 例： msiexec /package "d:/a/b.msi"
```
  
  