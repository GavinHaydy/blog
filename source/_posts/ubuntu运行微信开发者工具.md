---
title: "ubuntu运行微信开发者工具 | BugPZ"
date: 2021-10-15T03:11:12.000Z
tags: [ubuntu]
categories: [工具]
---
## 下载nwjs-sdk
官网下载 https://nwjs.io/
![官网下载](https://i-blog.csdnimg.cn/blog_migrate/846ed8d723092386939383d02d0a0129.png)

命令行下载
```powershell
wget -c https://dl.nwjs.io/v0.57.1/nwjs-sdk-v0.57.1-linux-x64.tar.gz
```
下载好后解压

```powershell
tar -zxvf nwjs-sdk-v0.57.1-linux-x64.tar.gz
```
## 下载微信开发者工具
```powershell
https://github.com/cytle/wechat_web_devtools
```
## 下载后把package.nw复制到刚才解压的nwjs-sdk-v0.57.1-linux-x64目录
## 进入nwjs-sdk-v0.57.1-linux-x64目录启动nw

```powershell
cd nwjs-sdk-v0.57.1-linux-x64
./nw
```
