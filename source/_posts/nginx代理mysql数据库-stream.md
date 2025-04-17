---
title: "nginx代理mysql数据库 stream | BugPZ"
date: 2022-03-03T06:22:58.000Z
tags: [nginx,mysql]
categories: [运维，数据库]
---
### 写作背景
在某云搞了个服务器，装了个数据库后一切配置OK，发现远程连接不上，排查了一天没有找到问题（安全组策略及防火墙、开放端口） 。在毫无头绪之际突然想到nginx代理的80端口可以访问，本着All Roads Lead to Rome的原则，尝试用nginx代理来解决此问题

# 准备工作 (已安装nginx跳过此步骤)
[nginx包地址](http://nginx.org/download/)
```powershell
#下载nginx  版本自选 听说需要>1.9.0版本才有strean
wget http://nginx.org/download/nginx-1.18.0.tar.gz
# 解压
tar xf nginx-1.18.0.tar.gz 
```
#  开始编译
	三方依赖库请自行百度，这里就不赘述了
```powershell
cd nginx-1.18.0
# 编译nginx时加上 ----with-stream
# 可选参数 --prefix=/usr/local/nginx   (--prefix=/usr/local/nginx指明软件安装的路径，/nginx是为安装nginx新建的目录)
./configure --with-stream
make
make install
```

# 更改配置文件
	假设nginx目录是 /usr/local/nginx 
```powershell
vi /usr/local/nginx/conf/nginx.conf
# 在文件最后添加以下配置
stream {
    server {
       listen 12345;	#外部访问端口 根据需要自行修改
       proxy_connect_timeout 10s;
       proxy_timeout 1800s;#设置客户端和代理服务之间的超时时间，如果半小时内没操作将自动断开。
       proxy_pass 127.0.0.1:3306; 本地数据库
    }
}
#配置后保存 启动nginx
/usr/local/nginx/sbin/nginx

```
# 测试是否成功
![成功后的图片](https://i-blog.csdnimg.cn/blog_migrate/17eb83e4ee6c666092def67441b149c6.png)
搞定