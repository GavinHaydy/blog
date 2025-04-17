---
title: "Linux下app自动化测试脚本 开发环境搭建"
date: 2022-04-02T08:31:20.000Z
tags: [python]
categories: [Linux，测试]
---
# 注！！！（作者电脑为Ubuntu20 不同发行版可能存在些许差异）

## 需要环境如下
1. java

2. Android sdk

3. Android模拟器

4. python

5. appium

# java
java可以直接使用apt命令安装，此方法无需配置环境变量，ps：ubuntu20好像自带了jdk11。安装命令如下：

```shell
#查看存在的版本
sudo apt list |grep openjdk
#选择对应版包本名安装
sudo apt install openjdk-...
```
# sdk和模拟器 模拟器使用adt或android studio可以跳过这里
sdk [下载地址1](https://www.androiddevtools.cn/index.html)  [下载地址2_度盘](http://tools.android-studio.org/index.php/sdk)
下载后解压 ,sdk目录如下
  ```shell 
  build-tools  licenses  platforms       skins          tools
  emulator     patcher   platform-tools  system-images
  #配置adb及uiautomatorviewer软链接，方便后续使用   下面 /...  表示全路径，根据实际路径自己替换 软链接命令为小写的 LN
  sudo ln -s  /.../platform-tools/adb   /usr/bin/adb
  sudo ln -s  /.../tools/bin/uiautomatorviewer      /usr/bin/uiautomatorviewer
  #验证
  adb --version
  # 显示如下
  #Android Debug Bridge version 1.0.41
  #Version 33.0.1-8253317
  #此时SDK就算完成了
  ```
  模拟器我试过几个Bliss OS和Genymotion  感觉比较麻烦，所以还是推荐使用adt的或android studio里的，大致说一下这个的配置
 一、 adt_bundel [下载地址](http://tools.android-studio.org/index.php/adt-bundle-plugin)
    这个相对Android_studio而言还是比较麻烦，先下载adt_bundel,下载后解压，包里包含eclipse及sdk，
    运行安卓模拟器需要自己下载sdk-system-image [最下面](http://tools.android-studio.org/index.php/sdk) 并在sdk文件夹新建system-images,
把下载的image解压到此，打开eclipse创建模拟器，完成

  二、 android studio [官方下载](https://developer.android.com/studio) 下载后解压：

  ```shell
  #我的包名  android-studio-2021.1.1.22-linux.tar.gz
  
tar -xvf android-studio-2021.1.1.22-linux.tar.gz    

  cd android-studio/bin

  # 直接启动  

  ./studio.sh
```
  启动后创建一个项目，会让你选择sdk路径，（路径别带中文）
  进入项目后点击右上角的模拟器(driver Manager)
  ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/cb642b79f3a4e2af834c925b73a382df.png)

   
点击Creat drivice
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/853fc1963fc672611d3b3d5a0dd308fe.png)

  选择phone 并选择一个机型 并选择下一步

![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-IZEcMMJK-1648887689602)(/uploads/photo/2022/a9815b15-cd79-4e72-92f9-abaa7bfc20e1.png!large)\]](https://i-blog.csdnimg.cn/blog_migrate/97a3a2b131fa9268c32f014d0a4f9379.png)

  选择镜像 根据需求自行选择，可以推荐按Target选择，点download下载

![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-H3XtfYSt-1648887689603)(/uploads/photo/2022/c66204d2-dba0-48f3-99ff-772088bd7b2f.png!large)\]](https://i-blog.csdnimg.cn/blog_migrate/f8faf1f63a159ba7697f2666625a33e2.png)

  下载后选择镜像，下一步，给模拟器取名，点完成
  返回driverManager启动模拟器，此步完成

# python   ubuntu20自带python3  略过
# appium 
 [官方github](https://github.com/appium/appium-desktop/releases)
进入网站后自行选择版本，点开Assets并选择xxx.AppImage下载如图：（appimage为linux可执行文件）
![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-AcZSYLbo-1648887689603)(/uploads/photo/2022/0f966ec9-8c0e-4344-b7b7-ce979b7abdd5.png!large)\]](https://i-blog.csdnimg.cn/blog_migrate/40cf780d2dde0bf64be94886a739f854.png)

下载后 右键-属性-权限 允许文件作为程序执行 打钩，或者用命令
```shell
  chmod +x Appium-XXXX.AppImage
  # 打开方式 双击或者
./Appium-XXXX.Appimage
```
重点!!!  因为之前没有配置安卓环境变量，所以需要在appium里面配置
打开appium，点击Advanced  选择Edit Configurations配置环境 如图：
![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-mzbCOhdU-1648887689604)(/uploads/photo/2022/b864bb4f-270f-455c-8875-eba02a10ef4d.png!large)\]](https://i-blog.csdnimg.cn/blog_migrate/e56cf3fa78c382f30a81c364e190ad12.png)

在ANDROID_HOME输入你得SDK路径   如果没修改过文件夹名称就应该是 /xxx/xxx/sdk

至此，整个appium的Linux开发环境就完成了
