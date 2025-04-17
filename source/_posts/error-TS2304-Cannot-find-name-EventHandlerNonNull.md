---
title: "error TS2304 Cannot find name ‘EventHandlerNonNull‘"
date: 2021-09-13T04:44:54.000Z
tags: [typescript]
categories: [前端]
---
@[TOC](TS+ Ant Design Vue打包报错（error TS2304: Cannot find name 'EventHandlerNonNull'）解决方案)
## 报错详情
报错如图所示: 
![报错](https://i-blog.csdnimg.cn/blog_migrate/8911dfc93b1df253fb8992b17bffbf23.png)
查看了一下ts源码，发现4.2版本没有EventHandlerNonNull  所以TS≥4.2时打包应该都会出现此问题 附源地址：
[TS4.1源](https://github.com/microsoft/TypeScript/blob/release-4.1/lib/lib.dom.d.ts)
[TS4.2源](https://github.com/microsoft/TypeScript/blob/release-4.2/lib/lib.dom.d.ts)
<font color=#f41e43>访问不了可将github.com改为github1s.com</font>


## 解决方案（刚开始想改Ant Design Vue 发现用EventHandlerNonNull的地方有点多，放弃）
 `在node_modules\typescript\lib\lib.dom.d.ts增加如下代码`
```javascript
// An highlighted block
interface EventHandlerNonNull {
    (event: Event): any;
}
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/d824ef1cbcd1654c07c3e7d80f61572e.png)

##  再次执行编译  
![成功](https://i-blog.csdnimg.cn/blog_migrate/6fc4e67fd30accc48021095ced2ae4b4.png)

 如上图所示，编译成功,搞定(第一次接触TS，也不知是不是最优解)

