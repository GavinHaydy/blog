---
title: "Cypress安装报错连接超时"
date: "2022-10-16 16:12:03"
tags: [js, Cypress]  # 可多个
categories: [测试,工具,报错处理]    # 可多个
---
@[toc]
# 安装
```shell
# Cypress安装非常简单
npm install cypress
# 原本以为安装是最简单的事，奈何事与愿违，安装时出现了问题 当然这个情况可能不是所有人都会遇到
# 下面先看下我遇到的问题
```
# 安装遇见的问题及处理方式
==报错信息有点长，但不重要，只需看红框处即可==
`错误信息很明显，提示连接网站失败 查了下ip发现是米国的，所以这里连不上很正常`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/a4bd25fc21fb271136f9f10bfd1115b6.png)
### 解决方案及过程
- 看见url首先想到的是修改域名为国内下载地址
- 通过全局搜索找到此域名代码出现在 cypress/lib/tasks/downloads第36行 如图
	![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/e620dab8a80bdce4c19e12f086a45c7d.png)
`这里下载路径是拼接的，看了下国内仓的文件下载路径，差距略大，果断放弃`

==这时果断去官方仓的issues和官方文档看了看，问题解决方案都指向了代理==
这里比较巧合的是我刚好也有，所以试了一下设置代理
```shell
"因为我的是linux系统，所以其他系统可能会不太一样 设置方法照着官方文档操作即可"
export HTTPS_PROXY=http://127.0.0.1:8889  # 端口在网络设置-代理里面查
设置后再试试 npm install cypress
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/c9572804557d7a66a54cd61a73450899.png)
问题解决

# 测试一下
```shell
 # 先启动一次cypress
 # package配置 "cypress:open": "cypress open"
 ./node_modules/.bin/cypress open
 #启动后项目下会多出一个默认文件夹 图放下面 图一
 #创建一个用例包e2e和用例baidu.cy.js 图二
```
==图一==
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/2b429f2cf29ea53decedd4fa0805ac6f.png)
==图二==
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/3e939edd37a7c70c8c9d1c56ced886bd.png)


`打开baidu.cy.js写入如下测试代码`
```js
describe("cases title", () => {
    it('case title', () => {
        cy.visit('https://www.baidu.com')
        cy.title().should('include', '百度一下')
    })
    it('case1 title', () => {
        cy.visit('https://www.baidu.com')
        cy.title().should('include', '断言失败')
    })
    it('api case', () => {
        cy.request('http://www.baidu.com')
            .its('body')
            .should('include', '你就知道')
    })
})
// 执行结果如下图
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/198e10069d433de5754150b08c096ad5.png)
# 小结
- 关于很多人拿Cypress和selenium做对比，感觉没多大必要，毕竟存在即合理
- 关于cypress同时支持ui和接口测试这点，感觉还不错
- cypress用的js,代码方面也不难，入门应该挺快的，但是API挺多，需要花些时间熟悉
