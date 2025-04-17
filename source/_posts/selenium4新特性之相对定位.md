---
title: "浅谈selenium4新增功能之相对定位"
date: 2022-08-31T15:39:56.000Z
tags: [selenium,python]
categories: [测试]
---
@[toc]
# 前言
	selenium4中新增了相对定位器  以下为官方介绍：
	
`这里以python做演示，用其他语言的小伙伴可以看看官方文档`
[点我跳转到官方文档哟](https://www.selenium.dev/documentation/webdriver/elements/locators/#relative-locators)
```shell
	
`Relative Locators`
Selenium 4 introduces Relative Locators (previously called as Friendly Locators). These locators are helpful when it is not easy to construct a locator for the desired element, but easy to describe spatially where the element is in relation to an element that does have an easily constructed locator.

How it works
Selenium uses the JavaScript function getBoundingClientRect() to determine the size and position of elements on the page, and can use this information to locate neighboring elements.
find the relative elements.

Relative locator methods can take as the argument for the point of origin, either a previously located element reference, or another locator. In these examples we’ll be using locators only, but you could swap the locator in the final method with an element object and it will work the same.

"顾名思义： 相对定位器就是根据某个元素去定位其他元素，这里新增了5个方法"
"分别为： 以上 above, 往下 below， 往左 to_left_of，往右 to_right_of, 附近 near"
"这些方法也可以组合使用 有时元素最容易被识别为在一个元素的上方/下方和另一个元素的右/左。"
submit_locator = locate_with(By.TAG_NAME, "button").below({By.ID: "email"}).to_right_of({By.ID: "cancel"})
```

# 源码及使用前准备
 ==源码如图==`包内包含两个方法 with_tag_name、locate_with及一个类RelativeBy`
 `因为两个方法的返回都是调用的RelativeBy类 所以直接调用两个方法即可`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/eb4378cc9bcd300212792bfb2d984ad7.png)
==所以需要先导包==

```py
from selenium.webdriver.support.relative_locator import locate_with, with_tag_name
```

# 使用方法
`这里使用鹅厂的门户网站演示 界面如下图`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/b4d2a6f7b48eec30824a702757b0c260.png)


### above 以上
```py
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.relative_locator import locate_with, with_tag_name

dri = webdriver.Chrome()
dri.get('https://qq.com')

# 首先定位qq空间
dri.find_element(By.ID, 'm-product')
q_zone = dri.find_element(By.PARTIAL_LINK_TEXT,'空间')
print(q_zone.text) #验证一下

# 定位上面的 体育APP
q_sports = locate_with(By.TAG_NAME, 'a').above(q_zone)	#  这里分两步写是为个看着直观一点
print(dri.find_element(q_sports).text) # 验证是不是定位到 体育APP


dri.quit()

```
`结果如图所示`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/23a4b34c00d024c247c9eea3b1fd2216.png)

### below 以下
```py
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.relative_locator import locate_with, with_tag_name


dri = webdriver.Chrome()
dri.get('https://qq.com')

# 首先定位qq空间
dri.find_element(By.ID, 'm-product')
q_zone = dri.find_element(By.PARTIAL_LINK_TEXT,'空间')
print(q_zone.text) #验证一下

# 定位下面的 CF
q_cf = locate_with(By.TAG_NAME, 'a').below(q_zone)  #  这里分两步写是为个看着直观一点
print(dri.find_element(q_cf).text) # 验证是不是定位到 CF


dri.quit()
```
`结果如图所示`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/f394c3bf33fda612af373a78ae66952e.png)

### to_left_of 往左
```py
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.relative_locator import locate_with, with_tag_name


dri = webdriver.Chrome()
dri.get('https://qq.com')

# 首先定位qq空间
dri.find_element(By.ID, 'm-product')
q_zone = dri.find_element(By.PARTIAL_LINK_TEXT,'空间')
print(q_zone.text) #验证一下

# 定位下面的 QQ
q_qq = locate_with(By.TAG_NAME, 'a').to_left_of(q_zone)  #  这里分两步写是为个看着直观一点
print(dri.find_element(q_qq).text) # 验证是不是定位到 QQ


dri.quit()
```
`结果如图所示`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5b98636db6685bbb727e5df5935d46c4.png)
### to_right_of 往右
```py
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.relative_locator import locate_with, with_tag_name


dri = webdriver.Chrome()
dri.get('https://qq.com')

# 首先定位qq空间
dri.find_element(By.ID, 'm-product')
q_zone = dri.find_element(By.PARTIAL_LINK_TEXT,'空间')
print(q_zone.text) #验证一下

# 定位右边的企业微信
q_work = locate_with(By.TAG_NAME, 'a').to_right_of(q_zone)  #  这里分两步写是为个看着直观一点
print(dri.find_element(q_work).text) # 验证是不是定位到 企业微信


dri.quit()

```
`结果如图所示`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/d0c193ea42606f54991be87c9009bbbc.png)

### near 附近
```py
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.relative_locator import locate_with, with_tag_name


dri = webdriver.Chrome()
dri.get('https://qq.com')

# 首先定位qq空间
dri.find_element(By.ID, 'm-product')
q_zone = dri.find_element(By.PARTIAL_LINK_TEXT,'空间')
# print(q_zone.text) #验证一下

# 定位附近标签
q_work = locate_with(By.TAG_NAME, 'a').near(q_zone)  #  这里分两步写是为个看着直观一点
els = dri.find_elements(q_work)

for i in els:
    print(i.text)

dri.quit()
```
`结果如图所示  这里我用find_elements把附近所有的元素都找出来  这里跑了两次  元素的顺序是一样的`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/8baeca7e5edd29225d88f7124f4eb41d.png)


### 组合操作 查看右下方所有元素
`原图`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/598485ceca619baef70f5e5144e94695.png)


```py
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.relative_locator import locate_with, with_tag_name


dri = webdriver.Chrome()
dri.get('https://qq.com')

# 首先定位qq空间
dri.find_element(By.ID, 'm-product')
q_zone = dri.find_element(By.PARTIAL_LINK_TEXT,'空间')
print(q_zone.text) #验证一下

# 定位CF
q_cf = dri.find_element(locate_with(By.TAG_NAME, 'a').below(q_zone))

# 定位右下方
q_work = locate_with(By.TAG_NAME, 'a').to_right_of(q_cf).below(q_zone)  #  这里分两步写是为个看着直观一点
els = dri.find_elements(q_work)

for i in els:
    print(i.text)

dri.quit()
```
`结果  这里定位到了网页最下面 且后面两行只定位了2个标签  至于为什么 如果有兴趣的同学可以自己研究一下`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/97d76bbd4180ebe325cb9e119f53ee64.png)

### with_tag_name和locate_with的区别
先看源代码
```py
def with_tag_name(tag_name: str) -> "RelativeBy":
    """
        Start searching for relative objects using a tag name.

        Note: This method may be removed in future versions, please use
        `locate_with` instead.
        :Args:
            - tag_name: the DOM tag of element to start searching.
        :Returns:
            - RelativeBy - use this object to create filters within a
                `find_elements` call.
    """
    if not tag_name:
        raise WebDriverException("tag_name can not be null")
    return RelativeBy({"css selector": tag_name})


def locate_with(by: By, using: str) -> "RelativeBy":
    """
        Start searching for relative objects your search criteria with By.

        :Args:
            - by: The value from `By` passed in.
            - using: search term to find the element with.
        :Returns:
            - RelativeBy - use this object to create filters within a
                `find_elements` call.
    """
    assert by is not None, "Please pass in a by argument"
    assert using is not None, "Please pass in a using argument"
    return RelativeBy({by: using})
```

`这里的源码还是挺简单的 显而易见`
```py
with_tag_name()  # 括号内直接写标签名
locate_with() # 和 find_element()写法一样 用到by 和 元素定位表达式
```

# 结语
`文中代码部分 有些注释里的方位没有改 写完后才发现  因为这对文章内容没啥影响 这里就不改了`
`因为appium2是依赖selenium4的  所以个人觉得相对定位对于app自动化应该很有用 当然这只是个人猜测 至于能不能在app自动化中使用，有兴趣的小伙伴可以自行测试 用法应该都是一样的，就不再发文了`