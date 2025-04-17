---
title: "缺少libcrypto.so.1.1问题解决"
date: 2022-05-10T06:13:57.000Z
tags: [Linux]
categories: [ubuntu，问题处理,utools]
---
@[toc]
#  问题出现原因 及报错信息截图 
	ubuntu22 OpenSSL版本升级3.0造成

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/b44f6a5c1c6b3019a4de370258c29f3d.png)


# 解决方案
	找到libcrypto.so.1.1文件 复制到utool安装目录   此库可以在其他低版本linux上面查找，为了方便给我，我已将此库放度盘  链接及查找文件命令放下面
```	shell
#  在其他linux查找文件 
sudo  find /  |grep libcrypto.so  

# 把下载的库移动到 utools目录   目录位置:    /opt/uTools
sudo mv /xxx/libcrypto.so.1.1  /opt/uTools/    # 完成此步就可以用utools了
# 启动utools
utools
```
# 下载链接在这里
[请点此处下载](https://pan.baidu.com/s/1XropCCKnLd_v19Pjgp6Qgw?pwd=knpb)

