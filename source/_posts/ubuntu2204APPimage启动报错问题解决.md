---
title: "ubuntu2204APPimage启动报错问题解决"
date: 2022-05-07T14:08:22.000Z
tags: [Linux]
categories: [ubuntu,问题处理]
---
@[toc]
# 前言：
 因为个人比较喜欢提前使用一些较新的软件、系统之类的东西，所有在ubuntu22刚发行就选择了升级， ubuntu22.04目前好像还不是稳定版本，几乎每天都会有更新，如果不是特别喜欢处理问题的同学还是不太建议更新，国内的软件如：网易云、utool等都会出现依赖问题（废话有些许的多了） 下面进入正题
# 报错信息
	直接运行APPimage会有如下图的提示 因为APPimage依赖libfuse2，原本打算直接安装libfuse2，结果发现在官方wiki提示ubuntu22不建议安装，可能会引起系统问题，so：只能慢慢看文档
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/c82903d8ce57d70ea751814455f55a19.png)
# 解决方法
 在docker说明处发现如下文档
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/94b237ed0df72341fbd26af49f8341f7.png)
```shell
 Instead, just use the --appimage-extract-and-run parameter to the AppImage in your build script:
在平时运行APPimage的命令后面加上 参数--appimage-extract-and-run
例：
./test.Appimage --appimage-extract-and-run
```

# 结果展示

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5815c08fde36f9979107e508fe27b64c.png)

# 你以为就完了？ 天真！
	 本着严谨发文的原则，又试了一下Appium-Server.APPimage
	 结果还真就报错了  如图
	
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/11b9366ea86ab24551e6889d68a0cd5e.png)
 ```shell 
FATAL:gpu_data_manager_impl_private.cc(986)

通过报错信息可知这和libfuse没有关系，
本着面向浏览器解决问题的态度，去栈溢出（https://stackoverflow.com/）找了一下，没有发现此问题
及其不甘心的我又Google了一下，还真就找到了解决方案
在之前参数的基础上再加一个取消沙盒的参数  --no-sandbox
完整命令如下：
./Appium-Server.Appimage  --appimage-extract-and-run  --no-sandbox

 ```
 
 # 结果展示2

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/e97ca2e1ce8b3bb2f04fcd06c2c82ca0.png)

# 结语
 目前ubuntu22.04虽然相比20.04变化确实有点大，但是存在的问题还是有点多的，比如python自带版本升级到了3.10  OpenSSL升到了3.0  有些依赖老版本库的软件会出现问题
 所以 再次建议各位想要升级的再观望一段时间