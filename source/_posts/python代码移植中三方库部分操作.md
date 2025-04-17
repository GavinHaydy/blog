---
title: "python代码移植中三方库部分操作"
date: 2022-08-27T13:18:09.000Z
tage: [python]
categories: [工具]
---
@[toc]
## 前言

​	`此篇文章环境为linux,windows环境下部分命令可能存在差异，因为作者没有windows系统，所有在此就不做windows的操作说明了，使用windows的同学请自行转换dos命令`

# pip 依赖库导出、追加与删除

==导出依赖库==

```shell
pip freeze > requirements.txt #导出所有依赖库
pip freeze |grep xxx > requirements.txt #导出某个依赖库
pip freeze |grep 'xxx0\|xxx1\|xxx2' > requirements.txt # 导出多个依赖库
```

==追加依赖库==`因为命令的最佳默认不会换行，是在最后一行接着写入，所以此处需要用到echo命令`

```shell
`追加是 >> `
echo -e "\n`pip freeze |grep xxx`" >> requirements.txt #追加某个依赖库
echo -e "\n`pip freeze |grep 'xxx0\|xxx1\|xxx2'`" >> requirements.txt # 导出多个依赖库 
```

==删除依赖库==

```shell
`删除所有依赖库，先导出依赖 再根据依赖删除`
pip freeze > uist.txt	# 导出所有依赖
pip uninstall -r uist.txt -y	# 根据uist.txt删除库
rm -f uist.txt

`删除部分依赖库`
echo -e "`pip freeze |grep 'xxx0\|xxx1\|xxx2'`" > uist.txt	# 导出指定依赖库
pip uninstall -r uist.txt -y	# 根据uist.txt删除库
rm -f uist.txt
```

==根据依赖库文件安装==

```shell
pip install -r requirements.txt
```

