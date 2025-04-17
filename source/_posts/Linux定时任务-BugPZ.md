---
title: "Linux定时任务"
date: 2022-03-29T19:00:27.000Z
tags: [Linux]
categories: [运维，服务器]
---
### 前言
  测试有时写的脚本需要每天运行，一些技术稍微好点的同学可能会通过Jenkins、k8s或者xxjob这类的东西去定时任务，但是对于技能比较弱的同学可能用这些东西比较费劲，所以大概介绍下Linux的定时任务操作流程吧，我这里尽量写的详细一些，此处以python脚本为例


### 准备工作
  1、一台Linux服务器（虚拟机也行）
  2、测试脚本
  3、运行环境   python安装过程就不讲了 现版本的Linux基本自带python3了 
  

## 第一步 上传脚本到服务器  
最简单的方式sftp命令上传  （文件太大不建议  此方法传输比较慢）
常用命令  cd  pwd ls   put get    lcd lpwd lls    前面多个l为本地命令   
```shell
# 连接命令
sftp user@ip
# 上传文件 
put xxx
#上传文件夹 
put -r xxx  
```
## 第二步  编辑定时任务
  
```shell

# 最简单的  
vi /etc/crontab

# 原文件如下

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#

# 定时任务格式分为7段 以空格区分
# 分钟 小时 天 月 周几 执行用户  执行命令
# 符号包括 * , -  /
# 星号* 所有值
# 逗号, 列表隔离 比如3点和6点执行  
* 3,6 * * * root  xxx
# 中横杠 -  范围符号  比如1月到3月执行  
* * * 1-3 * root xxx
# 斜杠/  执行频率  比如每2天执行  
* * */2 * * root xxx

# 正题   假如现在有个脚本路径为  /home/test/test_script.py  要求4到6月每隔2天早上8点15分执行
# 首先确认python路径
which python3   # 返回路径为 /usr/bin/python3
# 编辑定时任务   
15 8 */2 4-6 * root /usr/bin/python3 /home/test/test_script.py
# ============================================================
# 编辑完成后 重启定时任务
service cron restart  # 有的可能是 service crond restart

# 查看执行情况
cat /var/log/cron   #或 cat /var/log/cron.log

# 我用的系统是ubuntu  需要提前设置
sudo vi /etc/rsyslog.d/50-default.conf

cron.*              /var/log/cron.log #将cron前面的注释符去掉 

#重启rsyslog

sudo  service rsyslog  restart

sudo service cron restart

#查看crontab日志

cat  /var/log/cron.log 
```
## 至此 简单的定时任务设置就算完成了
  
