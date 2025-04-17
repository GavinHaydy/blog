---
title: "mysql常见问题记录"
date: 2022-04-24T04:01:52.000Z
tags: [mysql]
categories: [数据库,问题处理]
---
@[TOC]
# 查看mysql版本号
```sql
select @@version;
```

# 事务自动提交
```sql
select @@autocommit;
# 0是关闭  1是开启
#关闭自动提交
set @@autocommit;
#Jebt全家桶需要重新打开会话才生效  其他工具请自行测试
```
# mysql版本 ≥5.7 group by 报错

```sql
 # 报错信息 为 sql_mode="ONLY_FULL_GROUP_BY"
select @@GLOBAL.sql_mode;

set @@GLOBAL.sql.mode='把上面查询出来的结果复制到这里，把ONLY_FULL_GROUP_BY 删掉再执行';
```

# [1235] This version of MySQL doesn't yet support 'LIMIT & IN/ALL/ANY/SOME subquery'
==原sql如下==
```sql
select * from cj where score in (select distinct score from cj group by name order by score desc limit 5) order by score desc ;

```
==修改后如下==
```sql
select * from cj where score in (select t.score from (select distinct score from cj group by name order by score desc limit 5) as t)order by score desc ;
```
`这个问题的原因是limit不能在in的子语句中，至于错误信息中提到的当前版本不支持，我的版本是8.0.28，不知道后面版本后期版本是否至此原sql的写法请各位自行测试`