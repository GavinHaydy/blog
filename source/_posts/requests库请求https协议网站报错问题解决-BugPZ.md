---
title: "requests库请求https协议网站报错问题解决"
date: 2022-04-11T07:23:49.000Z
tags: [python]
categories: [测试,报错处理]
---
# 报错信息及原始代码
![报错信息](https://i-blog.csdnimg.cn/blog_migrate/616f5a74ecec9c3fe5eab8b92243a278.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/ab91760258afcc409136260945cf5ff7.png)

# 解决方法
查看 [requests官方文档/ssl](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#ssl)
发现如图所示参数
![关闭证书验证](https://i-blog.csdnimg.cn/blog_migrate/700826374d030336e60b7a1e5912e42d.png)

修改后代码

```python
r = requests.post(utl, data=data,verify=False)
```
关闭证书验证后会有如下警告，
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/e5b1e9457fef8e155bc2f048d5860bef.png)
[根据警告信息给的网址找到如下信息](https://urllib3.readthedocs.io/en/1.26.x/advanced-usage.html#ssl-warnings)

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/3df774a72baa492647ca96437ea600fd.png)
关闭提示代码： 三种

```python
# 第一 用requests
requests.packages.urllib3.disable_warnings()
# 第二 用urllib3
import urllib3
urllib3.disable_warnings()
#用logging
import logging
logging.captureWarnings(True)
```

### 没有提示了 

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/1714a53be38e6bfdf5fde6f692813cf6.png)