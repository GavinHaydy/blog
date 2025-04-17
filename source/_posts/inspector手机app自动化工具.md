---
title: "appium inspector手机app自动化工具"
date: 2023-07-26 13:32:04
tags: [tools]
categories: [工具,测试]
---

@[TOC](这里写自定义目录标题)
# 一、 环境

 1. Android SDK
 2. Appium
 3. appium inspector
 5. 准备一个app(这里就用csdn的app)


# 二、安装
- `sdk和appium这里就不写了，这里就只说下inspector及app`
  - 手机安装app，并把app的安装包存放到桌面
 - `inspector`
 	- [下载地址,连不上可用备用地址](https://github.com/appium/appium-inspector/releases)
 	- [备用地址](https://kgithub.com/appium/appium-inspector/releases) 
 		- `下载对应平台的包安装即可`
 
#  三、配置
如图：
	![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/4fd0afce60c1d427fcdc0ab9f16f08ee.png)

- json解释
```json
{
  "platformName": "手机系统",
  "platformVersion": "系统版本，使用命令 adb shell getprop ro.build.version.release 查看",
  "deviceName": "使用adb devices命令查看",
  "app": "apk在电脑的完整路径，注意，路径需要用双斜杠\\,如果app已安装则不需要此行",
  "appPackage": "见下面",
  "appActivity": "见下面",
  "noReset": "true"
}
```
-  appPackage及appActivity获取
	- 打开sdk路径，找到build-tool  在路径栏打开cmd回车
		![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/3bce6b4af6226f019d3e9ba18751bccc.png)![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/91b33472dccc39abf67ad2d04882993e.png)
	- ==执行以下命令==
	```shell
	aapt dump badging  xxx.apk  #apk需要完整路径
	```
	执行后在结果里面查找package和activity 结果如下
	![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/bd0922fd45854c31e7359bff98541296.png)
	![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/91c66729397e028bb4a59306628c70df.png)
完整配置如下
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/cf48cf2ce9289984b2400598f43a823a.png)

==inspector配置完成==

# 四、服务
打开appium
- 直接startServer
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5d47043c598ea20c49970515d721ecf4.png)
- 启动inspector
	![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/6d59d969da1d7b1a6d128dd0bfee1af4.png)

# 效果图
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/ed581fe6843b7c980509646c2a358063.png)
