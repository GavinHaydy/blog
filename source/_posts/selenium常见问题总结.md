---
title: "selenium常见问题总结"
date: 2022-05-04T03:14:35.000Z
tags: [python,selenium]
categories: [测试,工具，问题处理]
---
@[TOC](在问答区一周左右 发现selenium的问题比较多  下面为最近见到的问题 后续有新问题会持续更新)
# 问题一 使用代理无法访问网站
[例子](https://ask.csdn.net/questions/7684748?answer=53748856&spm=1001.2014.3001.5504)

	问题出现原因：代理ip网络不稳定
	处理方法： 换代理

# 问题二 浏览器闪退 
	报错信息 ： chrome is no longer running,so ChromeDriver is assuming that chrome has crashed
	原因未知 
	处理方式:  取消沙盒
	

```python
from selenium import webdriver
ops = webdriver.ChromeOptions
ops.add_argument('--no-sandbox')
driver = webdriver.Chrome(options=ops)
# 上述方法如果解决不了，可能是应为系统权限问题引起的 需将本地账号设置为管理权限
```
# 问题三 驱动版本错误
	报错信息 类似: Message: session not created: This version of MSEdgeDriver only supports MSEdge version 98
	下载和浏览器版本对应的驱动

# 问题四  嵌套页面元素定位失败
	从要定位的元素往上找，看看是否有iframe
 [例子](https://ask.csdn.net/questions/7681234?answer=53745568&spm=1001.2014.3001.5504)

	
```python
# 进iframe有两种  推荐第二种
driver.switch_to_frame()
driver.switch_to.frame()
```

#  selenium参数表
	
[点我查看](https://blog.csdn.net/weixin_43968923/article/details/87899762)
