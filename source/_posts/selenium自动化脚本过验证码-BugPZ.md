---
title: "selenium自动化脚本过验证码"
date: 2021-11-09T07:15:06.000Z
tags: [python,selenium]
categories: [测试,工具]
---
@[TOC](selenium自动化脚本过验证码)
需要用到的链接：
链接: [Chrome开发者工具协议](https://chromedevtools.github.io/devtools-protocol/).
借鉴原文: [chromedriver通过network日志获取response.body](https://blog.csdn.net/qq_29404831/article/details/110086552).

## 实现原理
```
	打开浏览器输入域名，调用chrome协议获取验证码接口的响应，自动填入输入框
```
### 代码
```python
import json
import re
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.desired_capabilities import DesiredCapabilities


caps = DesiredCapabilities.CHROME
caps['loggingPrefs'] = {
    'browser': 'ALL',
    'performance': 'ALL'
}
caps['perfLoggingPrefs'] = {
    'enableNetwork' : True,
    'enablePage' : False,
    'enableTimeline' : False
    }

option = webdriver.ChromeOptions()
option.add_argument('--no-sandbox')     # 彻底停用沙箱
# option.add_argument('--headless')     # 无界面模式
# option.add_argument("--disable-extensions")    # 禁用拓展
# option.add_argument("--allow-running-insecure-content")  # 放行javascript/css/plug-ins等内容
# option.add_argument("--ignore-certificate-errors")  # 忽略与证书相关的错误
# option.add_argument("--disable-single-click-autofill")  # kEnableSingleClickAutofill的“禁用”标志
# option.add_argument("--disable-autofill-keyboard-accessory-view[8]")
# option.add_argument("--disable-full-form-autofill-ios")
# option.add_argument('user-agent=Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:55.0) Gecko/20100101 Firefox/55.0')
option.add_experimental_option('w3c', False)  # 关闭w3c  必须
option.add_experimental_option('perfLoggingPrefs',{
    'enableNetwork': True,
    'enablePage': False,
})
pattern = re.compile(r'getGraphicCode')  # 自定义正则  
driver = webdriver.Chrome(options=option, desired_capabilities=caps)
driver.get(url)  # url = 需要访问的地址
driver.maximize_window()
for typelog in driver.log_types:
    perfs = driver.get_log(typelog)
    for row in perfs:
        log_data = row['message']  # str ['params']
        l = pattern.search(log_data) # 正则匹配接口 （因为项目主要请求过多，用正则匹配出我需要的接口）
        if l is not None:	# 接口匹配成功，获取requestId(获取接口响应需要此参数)
            log = json.loads(log_data)
            requestId = log['message']['params']['requestId']
            try:
                response_body = driver.execute_cdp_cmd('Network.getResponseBody', {'requestId': requestId})	# 获取接口响应
                x = json.loads(response_body['body'])
                verifyCode = x['result']['verifyCode'] # 获取验证码字段
            except:
                pass
driver.find_element_by_xpath('/html/body/div/div[1]/div[1]/div/div[2]/div/form/div[1]/div/div[2]/input').send_keys('xxxxxxxxx')
driver.find_element(By.XPATH, '/html/body/div/div[1]/div[1]/div/div[2]/div/form/div[2]/div/div[2]/input').send_keys('xxxxxxxxxx')
driver.find_element(By.XPATH, '/html/body/div/div[1]/div[1]/div/div[2]/div/form/div[3]/div/div[2]/input').send_keys(verifyCode)	# 填写上面获取到的验证码
driver.find_element(By.XPATH, '/html/body/div/div[1]/div[1]/div/div[2]/div/form/div[5]/div/button').click()
```
![获取验证码截图](https://i-blog.csdnimg.cn/blog_migrate/dd266f55f84749e0535b8b43b7f9179c.png#pic_center)
# 过验证码的方法还有OpenCV识别 有时间再写