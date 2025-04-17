---
title: "Ubuntu桌面版工作环境配置二"
date: 2022-08-06T07:42:04.000Z
tags: [Linux]
categories: [工具，环境]
---
@[toc](目录导航)
# GNOME Tweaks(界面优化)

```shell
"使用以下命令即可安装"
sudo apt install gnome-tweaks
"安装完成后使用命令打开"
gnome-tweaks
"使用方法请自行研究"
```
#  flameshot(火焰截图)
[传送门](https://github.com/flameshot-org/flameshot/releases)
`如图，下载系统对应的包，我用的22.04，所以下载框住的包`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/4e04616a704a981b652856f6b0348086.png)
```shell
"下载好后切换到包目录，使用以下命令安装"
sudo dpkg -i flameshot-12.1.0-1.ubuntu-22.04.amd64.deb 

"启动程序"
flameshot
"执行命令后屏幕右上角出现如下如所示图标则说明成功"
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/14dc15b71f530cfb2f4b72db099f6fad.png)
# java
`jdk8因为官网需要登陆，比较麻烦，在这里就选择华为镜像下载，命令如下`
`我一般喜欢把包放/usr/local/内，这个根据个人习惯而定`
```shell
#创建java文件夹
sudo mkdir -p /usr/local/java
cd /usr/local/java/
# 下载jdk包 需要其他版本请自行更换
wget https://repo.huaweicloud.com/java/jdk/8u181-b13/jdk-8u181-linux-x64.tar.gz
# 解压
sudo tar -zxf jdk-8u181-linux-x64.tar.gz
# 配置环境变量  这里我选择配置全局  用户环境变量文件为 ~/.bashrc
sudo su root
# 如果没有vim可以使用 apt install vim 进行安装 
vim /etc/profile
# 在最后面加上以下两行  第一行最后改为自己解压的文件夹名
export JAVA_HOME=/usr/local/java/jdk1.8.0_181
export PATH=$JAVA_HOME/bin:$JAVA_HOME/jre/bin:$PATH

#保存后使用以下命令刷新
source /etc/profile

#使用以下命令验证 
java -version
javac -version

```
 `其他语言方法类似，有时间再补充吧`
 ==这里说明一下：==
 		1. 如果不想配置环境变量也可以用软链接 
 		2.  如果使用软链接，像nodejs需要配置node,npm,npx等，需要注意下
 # JetBrains全家桶安装
 `他家的软件安装方式都一样，此处就以Idea举例.首先到官网下载包，因为20版本之后需要登陆，此处以20版本为例`
 `进入官网选择Idea下载页` [传送门](https://www.jetbrains.com.cn/idea/download/other.html)`选择2020.3.4版本，下载.gz包,如图`
 ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/24906223c0841c467d18423a7f369c9c.png)
```shell
#因个人习惯把工具放在同一个地方，所有会创一个Tools目录，各位请根据自己习惯来
mkdir Tools
cd Tools
#然后把包剪切过来 我的包在下载目录里 这里也根据个人情况修改
mv ../下载/ideaIU-2020.3.4.tar.gz  ../Tools/
#解压
tar -zxf ideaIU-2020.3.4.tar.gz
#解压完成后切换到解压后的bin目录
cd idea-IU-203.8084.24/bin/
#运行idea.sh
./idea.sh 
#执行后会出现如下界面
```
==勾选协议，提交==
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5fbc5b784bdb7827266aa8fe5795ec94.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/4ebb4e73f590513d01a53099bfee68c2.png)

==先选择体验30天，至于后续是以什么方式激活这里就不管了，建议有钱还是支持下正版吧==
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/064b4461d1082be28d5885f9134e69a8.png)

==配置启动器，先新建或者打开一个项目，打开Idea界面后操作如下==
==打开工具栏的Tools，如图==
`方框中第一个为创建命令行启动，在命令行用idea命令就能启动`
`第二行为图标，按WIN键搜索idea可启动`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/96f7bf8c2a88b47d75261ae8e1256ded.png)
`创建图标会有个弹框，因为我的电脑只有自己使用，所有不需要勾选，这个视个人情况而定`
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/8a167407efcbb54e7cc2ec39822452d2.png)
`至此，安装和配置完成`

#  其他软件说明
	国内办公软件部分已支持linux，，这里就不再说了
	如微信，企业微信，内网通等软件可以使用wine安装使用
	企业微信wine跑会有较多的问题需要自己处理，可以使用docker跑，问题较少