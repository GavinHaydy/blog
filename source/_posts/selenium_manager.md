---
title: "selenium-manager驱动管理"
date: 2022-11-21 00:13:54
tags: [python]
categories: [工具]
---

# 关于selenium-manage
`众所周知， 一直以来，selenium使用都需要两个重要的东西，浏览器及其对应的驱动，最开始需要用户手动下载并配置环境。但是浏览器频繁的更新，导致驱动版本也需要跟着更新，于是出现了三方的驱动管理如：java的WebDriverManager、python的 webdriver -manager等，目前selenium官方已开发出了驱动管理工具 selenium-manager，根据官方介绍，目前好像是内置于4.6版本`
 [selenium-manager官方介绍](https://www.selenium.dev/blog/2022/introducing-selenium-manager/)
 
 # selenium4.6之前版本
 ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/d38bbbe8a154706cd466ea4b58d4828d.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/20599b476bd16c26a7ba382b1f3c21b5.png)

  `从以上图片中可以看出 4.6之前的确没有 `
  `根据官方文档介绍，可以在github仓里下载可执行文件，地址如下：`
	[selenium-manager下载地址](https://github.com/SeleniumHQ/selenium/tree/trunk/common/manager)
	`根据自己的电脑系统下载对应版本，我这里下的是linux版，下载后执行命令:`
```shell
./selenium-manager --browser chrome
# 这里需要注意，如果驱动需要更新  带上清理参数 -c
./selenium-manager -c -b chrome

```
 `执行结果如下：值得注意的是，因为版本原因，所以每次更新后文件夹名不同`
 `这里可以选择在启动浏览器前获取驱动路径(推荐)或者写个shell脚本自动设置环境变量`

```python
# 读取驱动代码
def driver_v(dir_path):
    return  dir_path + os.listdir(dir_path)[0] + '/chromedriver'

driver_path = driver_v('/home/bugpz/.cache/selenium/chromedriver/linux64/')

dri = webdriver.Chrome(executable_path=driver_path)
```
 ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/7b5e7490b686b7c1035e4c86701424b6.png)
`再次执行脚本 成功`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/0a8fca637ce4e993d9b1ab38bd771476.png)

# 4.6版本
`首先把驱动环境干掉，命令行执行chromedriver -veriosn 验证已删除环境 如图`

==这里删除命令是在另一个终端执行的 所有图里没有==

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5c9441c6e079780f5555e3dc74973894.png)

### 升级到selenium4.6
```shell
pip install --upgrade selenium
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/7cbff05a601cdf8b0ed48d9e576e8d46.png)
`执行脚本结果`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/f0376cfc9ba31f785423703785542bc5.png)
```shell
# 这里说明一下，4.6在不配置驱动的情况下会自动调用selenium-manager，selenium自带的，不用自己下载
#driver的安装目录和上面手动执行命令的目录一样
```