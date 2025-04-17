---
title: "selenium处理登录及提示弹窗(此处以python举例,登录弹窗处理已更新)"
date: 2022-07-24 19:37:34
tags: [selenium,python]
categories: [测试]
---
@[toc]
# 前言
`在web项目中有些功能需要调用外部应用或者提示安装插件窗口以及打开url时需要登录`
`此篇文章简单讲解一下如何处理这两类弹窗，因为暂未找到登录弹窗的网页，所有登录弹窗等作者后期实现了再补充`

# 非登录弹窗处理
`此处拿TX会议做个例子吧 先来看看弹窗是如何出来的`

- 首先打开网站[TX会议](https://meeting.tencent.com/user-center/join) 在输入框随意输入9位数点加入，标签页会弹出如下弹窗 ====
 ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/b1130881d02ab79868ae283347ad5a91.png)
- 这个弹窗selenium好像是处理不了的(如果说错还望指出)
 `这里我们就需要用到键盘事件来处理弹窗，因为我的电脑是Linux系统， 所以选择的是pynput库`
 
 ==安装库,命令如下==
 ```shell
 pip install pynput
 ```
 ==代码如下== `提示： 此段代码效果为选择弹窗中间的单选框：如果未达到效果请把sleep里面的数字加大，要选择下面的两个按钮请自行修改代码，弹窗默认聚焦到取消按钮`
 ```python
 from time import sleep
from selenium import webdriver
from pynput import keyboard
from pynput.keyboard import Key


dri = webdriver.Chrome()
dri.get('https://meeting.tencent.com/user-center/joining?meeting_code=123456789')
key_o = keyboard.Controller()
sleep(3)

key_o.press(Key.tab)
key_o.release(Key.tab)  # 焦点切换到'打开xdg-open'按钮


key_o.press(Key.tab)
key_o.release(Key.tab)    # 焦点切换到单选框
key_o.press(Key.enter) 	# 按下回车键选择单选框
key_o.release(Key.enter) 
 ```
 `友情提示：press方法千万不要单独使用！！！  press方法千万不要单独使用！！！  press方法千万不要单独使用！！！`

# 登录弹窗处理
`登录弹窗有两种处理方式，一种和上面一样，利用pynput库实现，第二种是利用url实现，虽然利用url很简单，但是这里还是说一下第一种处理方式吧`
`首先和上面一样 需要安装三方库, 上面有命令这里就不写了`
==登陆验证框如图所示==
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/442005632ca329a3d422bd524abb3ce1.png)
==代码如下==

```py
from selenium import webdriver
from pynput import keyboard
from pynput.keyboard import Key

dri = webdriver.Firefox(executable_path='D:/data/geckodriver-v0.31.0-win64/geckodriver.exe') # 这里我没设置环境变量  有环境变量不用照着写
dri.get('http://localhost:8081/manager/html') # 这个是有弹窗的页面
btn_obj = keyboard.Controller()  # 实例化类

"""进页面光标默认聚焦在用户名输入框，所以第一步就可以直接输入用户名"""
btn_obj.type('tomcat') # type函数是输入字符串的 
"""按tab键切换到密码输入框 press和release是一对的 按下后必须释放"""
btn_obj.press(Key.tab)	# 按下按钮
btn_obj.release(Key.tab)	# 释放   当然 press和release也可以用touch函数代替
""" 切换到密码输入框后输入密码"""
btn_obj.type('123456')

"""这里演示一下touch 因为tomcat密码框有回车事件，所以可以直接点回车登录"""
btn_obj.touch(Key.enter, is_press=True)
btn_obj.touch(Key.enter, is_press=False)

```
`这里需要注意一下，用type函数需要把默认输入法改成英文，不然会失败 ，或者可以在打开网页的下一步切换为英文输入法`
==脚本成功输入截图==
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/6ffbf56a3010c34f7ceeeb1ea02aeab7.png)
==登录后截图==
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5a287ea3ab3416179bb1f476721cee2b.png)

## 最简单的处理方式
`这种类型的登录，通过开发者工具可以发现你输入的参数并没有被带入url后面或者payload里面，这是因为参数被放在的url前面 格式如下`
`协议://账号:密码@IP地址:端口/路由 如下 账号tomcat 密码123456`
`http://tomcat:123456@localhost:8081/manager/html`
此时直接访问完整的url即可
```py
from selenium import webdriver


dri = webdriver.Firefox(executable_path='D:/data/geckodriver-v0.31.0-win64/geckodriver.exe')
dri.get('http://tomcat:123456@localhost:8081/manager/html')
```

