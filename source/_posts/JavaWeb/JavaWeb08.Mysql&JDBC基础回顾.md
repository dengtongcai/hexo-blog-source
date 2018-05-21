layout: '[layout]'
title: JavaWeb_08_Mysql&JDBC基础回顾
date: 2013-11-01 13:34:12
categories: JavaWeb
tags: [笔记]
---
# 1.SQL分类
- 数据定义语言（DDL）：create，alter，drop
- 数据操作语言（DML）：insert，delete，update
- 数据查询语言（DQL）：select，from，where
- 数据控制语言（DCL）：grant
# 2.JDBC操作数据库过程
## 2.1 注册驱动：
1. JDBC规范规定，所有驱动必须实现`java.mysql.jdbc.Driver`接口，表示是驱动；
2. mysql提供接口实现类：`com.mysql.jdbc.Driver`
3. JDBC规范工具类，用于注册驱动：`DriverManager.registerDriver(new com.mysql.jdbc.Driver());`
4. 实际操作：`Class.forName("com.mysql.jdbc.Driver");`
<!--more-->
反射将指定的字符串表示类加载到内存生成Class对象过程，静态代码块随之加载：

```
static{
	java.sql.DriverManager.registerDriver(new Driver());
}
```

## 2.2 获得连接
通过给定url，从对应驱动获得需要的连接:
mysql中url实例： jdbc:mysql://localhost:3306/gjp
标准的格式为：  jdbc:子协议：子名称：ip地址：端口号/数据库？参数
localhost 和 3306都是默认值，可以省略："jdbc:mysql:///gjp"


## 2.3获得语句执行者获取结果集
DQL ...返回内容
DML ...返回响应行数
`boolean execute(sql)`,执行所有sql语句
查询...true，提供方法getResultSet获取查询结果
修改...false

## 2.4 处理结果集

## 2.5 释放资源

