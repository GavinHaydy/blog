---
title: "接口自动化typescript+axios2"
date: 2022-06-11T15:28:50.000Z
tags: [typescript]
categories: [测试]
---
@[toc](目录)

#  [接口自动化测试之——typescript+axios(一、基础框架搭建及断言)](https://blog.csdn.net/bugpz/article/details/125212640)
# 前言  
```shell
 `关于参数化这里就只简单讲一下使用方法，至于外部文档读取，需要自己封装方法或者找三方库，这里就不多讲，有兴趣的小伙伴可以自己研究`
```
# 环境配置
```sh
 npm i jest-each
```
# 数据准备及读取，传参
```shell
 `数据准备`：  类型1如下  [{},{},{}...] 
 			 类型2如下  [[],[],[]...]	
 			 `两个类型的区别在于接受参数的数量不同，json只需要一个变量接收参数，数组则需要多个变量接收参数  代码如下`
```

 ==类型1代码如下  一个变量 parameter接收参数==
```ts
const s = [{'password': '2d3383fa392936ad7847c50a0bb4a58e','phone':'00000000000'},
    {'password': '2d3383fa392936ad7847c50a0bb4a58e','phone':'55555555555'}]

each(s).test('api_test', (parameter) =>{
    expect(parameter.password + parameter.phone).toBe('typescript')
})

```
==执行结果==
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/4df2a6ef9256c2b09b8564cd3f6ffca6.png)


==类型2代码如下  两个变量 password，phone 接收参数==
```ts

const s1 = [['this is password a','12345678999'],['this is password b', '12345678900'],]

each(s1).test('api_test', (password,phone) =>{
    expect(password + phone).toBe('typescript')
})
```
==执行结果==
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/a70c151bf8bb70d3d9d131cf3a4d0680.png)
# 总结
```shell
`ts用来做自动化测试感觉还是挺不错的，测试框架自带生成报告，对于常规的python自动化，多了数据类型校验，
当然缺点也很明显，代码量大了一丢丢，对于初学者可能不是太好理解。
但是相较于常用的java自动化还是算比较简单了。
后期有时间会继续写ts做web自动化  别问为什么不写js的自动化，ts的学会了js自然也就会了`
```
:smirk: