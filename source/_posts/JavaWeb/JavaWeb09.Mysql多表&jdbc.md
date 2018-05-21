layout: '[layout]'
title: JavaWeb_09_Mysql多表&jdbc
date: 2013-11-15 13:23:22
categories: JavaWeb
tags: [笔记]
---
# 1.多表中主键外键
## 1.1 外键：
- 从表中的一个字段
- 字段名称自定义，建议与主表名称一致
- 类型必须与住表主键类型一致
- 内容必须是主表主键的引用
- 目的：保证数据完整性
- 没有设置外间值，外键值默认为null;从表不能使用主表不存在的主键值，主表不能删除从表正在使用的外键对应主键值；
- 语法
```
//一对多
添加
alter table 从表 add [constraint] [外键] foreign key (从表外键字段名) reference 主表 (主表主键);
constraint foreign key (从表外键字段名) references 主表(主键);
删除
alter table 从表 drop foreign key 外键;
```
<!--more-->
```
//多对多
多个主表对应一个只有外键的从表
```
# 2.多表查询-一对多
## 2.1 笛卡尔积：多表乘积

```
SELECT * FROM  A,B,C...;
```
## 2.2 隐式内连接
在笛卡尔积的基础上添加连接条件
```
//主键外键不同名
SELECT * FROM  A,B,C... WHERE cid = category_id; 
//表别名
SELECT * FROM  A,B,C... WHERE c.cid = p.category_id; 
SELECT * FROM  A,B,C... WHERE category.cid = prooduct.category_id; 
```
## 2.3 内连接,"隐式内连接"的另一种语法
```
SELECT * FROM  A
    inner join B on a=b
    inner join C on b=c; 
```

```
SELECT * FROM category c
    inner join product p on c.cid = p.category_id;
```
## 2.4 外连接
### 左外连接

```
//查询左表A所有数据，右表B是否显示取决于条件是否成立,不成立则显示null
SELECT * FROM A LEFT OUTER JOIN B ON a=b;
```
### 右外连接
```
//查询右表B所有数据，左表A是否显示取决于条件是否成立，不成立则显示null
SELECT * FROM A RIGHT OUTER JOIN B ON a=b;
```
## 2.5 子查询
一条SELECT语句的查询结果，作为另一条SELECT语句语法的一部分（条件，表名，查询字段）；

# 3.多表查询-多对多
## 3.1 中间表
## 3.2 联合主键

```
now()   表示当前日期
```
```
ALTER TABLE orderitem ADD CONSTRAINT PRIMARY KEY (pid,oid);
```
## 3.3 查询
```
//不可跳过表，需要连接过去
SELECT p.pid,x.oid FROM orders oi,orderitem x,product p WHERE x.oid = oi.oid AND oi.pid = p.pid AND x.createtime = 2016-06-06;
```
# 4.jdbc
## 4.1 Junit单元测试工具
```
//注解@Test 可以添加到指定方法上，右键就可以运行测试
@Test
public voidtestxxx{
    
}
```
## 4.2 加载配置文件ResourseBandle 对象