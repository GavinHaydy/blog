---
title: "Ubuntu桌面版工作环境配置一"
date: 2022-07-31T15:41:46.000Z
tags: [Linux]
categories: [工具，环境配置]
---
@[toc](文章导航)
`注意：目前22版本因为基础库升级，部分原生应用如网易云音乐，utools等打不开，需要自己处理问题。`
`如果遇到问题可以看下我的其他文章，有问题处理方案，但是如果是新手还是推荐20,很多软件适配都是之更新到20`
`废话不多说，直接开干`
# 常用命令简介
`此处默认看文章的各位都至少了解Linux基础命令`
命令|作用|使用方法|备注
-|-|-|-
dpkg|安装deb包|sudo dpkg - i xxx.deb|deb为ubuntu安装包的后缀名
apt install|安装命令|sudo apt install xxx|
apt remove|卸载命令| sudo apt remove xxx | 
systemctl|控制面板 |ststemctl start/stop/reload/restart xxx|此命令参数较多，可使用systemctl --help查看
wget|下载命令|wget xxx|有时下载安装包会用到
curl|访问url命令|curl https://xxxx \| sh|一般用于一件安装一些软件

# 常用软件及作用介绍
软件名|作用|下载地址|备注
-|---|-|-
flameshot|截图|https://github.com/flameshot-org/flameshot/releases|火焰截图,算是snipaste的linux替代品吧
utools|工具平台|https://www.u.tools/|插件较多，几十种功能插件，如果插件不够用还可以自己开发(需要会JS)
网易云|音乐|https://music.163.com/#/download|右上角选`其他操作系统客户端`
钉钉||https://page.dingtalk.com/wow/z/dingtalk/default/dddownload-index?from=zebra:offline|
飞书||https://www.feishu.cn/download|
WPS|文档|https://linux.wps.cn/|其实ubuntu自带的LibreOffice也不错
Qv2Ray||请自行到githb搜索|官方有介绍，此处不做说明
fcitx5|小🐧输入法|使用命令 sudo apt install fcitx5|这个输入法个人比较喜欢
搜狗输入法||https://shurufa.sogou.com/linux|搜狗依赖fcitx `fcitx和fcitx5不一样哟，这点需要注意`
ubuntu-tweak|桌面优化|因为要增加源，后面再介绍|

`除上述软件外还有一些编译器，因为都是支持ubuntu的，这里就不写了`
`后面还有一些不支持linux的软件如微信、企业微信等下一篇文章再具体说一下怎么去安装使用`

# 软件安装
### 火焰截图
`因为后面文章需要截图，所以先介绍下这个吧`

==其他的软件下期再讲==
```shell
"首先下载包，然后切到包所在目录执行以下命令, 注意，版本号可能不一样，按照自己包的版本号"
sudo dpkg -i flameshot-12.1.0-1.ubuntu-22.04.amd64.deb
"安装时可能会出现如下信息"
# dpkg: 处理软件包 flameshot (--install)时出错：
#依赖关系问题 - 仍未被配置
"使用以下命令解决，后续遇到此问题大部分情况下也适用"
sudo apt install -f
"命令执行完毕后打开火焰截图看看是否正常,执行以下命令"

flameshot
"如果成功，屏幕右上角会出现图标，如下图"
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/c95d7ce98ffa1b954427b7978613c539.png)


# 特别说明
	ubuntu软件都是.deb后缀，如果下载软件让选择包类型，选deb就行
	在github上面部分软件发行版是没有deb包的，这种就下载APPImage个格式，
		这个类似于windows所谓的绿色版软件，不需要安装，给它执行权限就能直接运行
	