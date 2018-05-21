layout: '[layout]'
title: JavaWeb_10_JDBC连接池&DBUtils
date: 2013-11-09 10:32:12
categories: JavaWeb
tags: [笔记]
---
# 1.使用连接池重写工具类
## 1.1规范
Java为数据库连接池提供公共接口`javax.sql.DataResjavax.sql.DataSource`，重写`getConnection()`方法
## 1.2 重写连接池工具类
- 提供容器
- 注册驱动
- 获取连接并添加到容器
- 返回连接池
<!--more-->
## 1.3 增强close()方法
### 装饰设计模式
- 变量 接口A，实现类B，其他实现类C
- 明确并实现A
- 提供成员变量，提供有参构造存放实际参数
- 重写需要增强的方法
- 调用不需要重写的方法。
- 如果要关闭原Connection，需要在装饰类中定义一个获取Connection的方法
# 2.D3P0
## 2.1 导入jar包
## 2.2 配置文件
- 名称 `c3p0-config.xml（固定）`
- 位置 src
- 常见配置项
# 3.DBCP
## 3.1 导入jar包
## 3.2 配置文件
- 名称 `*.properties`
- 位置 任意
- 注意：`properties` 不能编写中文，不支持在STS中修改，必修使用记事本修改内容，否则中文注释就乱码了。
- 常见配置项
# 4.DBUtils
## 4.1 JavaBean组件
其实就是一个根据数据库列名建立与之对应的类,==必须提供无参构造==
## 4.2 DBUtils完成CRDU
### QueryRunner核心类
- `Query(DataSource ds)`
- `update(String sql, Object...params)`
- `query(String sql, ResultSetHandler<T>rsh, Object...params)`
### ResultSetHandler结果集处理
- `BeanHandler`   结果集中的第一条记录封装到一个指定的javaBean中
- `BeanListHandler`   结果集中的每一条记录封装到一个指定的javaBean中，并将这些javaBean封装到一个List集合中
- `ScalarHandler` 他是用于单数据，一列。
### DBUtils工具类
- `closeQuietly(Connection con)` 关闭连接，有异常try后不抛
- `commitAndCloseQuietly(Connection con)` 提交并关闭连接
- `rollbackAndCloseQuietly(Connection con)` 回滚并关闭连接