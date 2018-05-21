layout: '[layout]'
title: JavaWeb_19_JDBC事务
date: 2014-01-09 16:21:42
categories: JavaWeb
tags: [笔记]
---
# 1 概念

一个业务存在一组操作，要么全部成功，要么全部失败

# 2 操作
## 2.1 mysql操作
- 开始事务`start transaction`
- 提交事务`commit`
- 回滚事务`rollback`<br/>
mysql事务默认自动提交： `autocommit = on`,在之前执行一条sql语句，相当于一个事务 
## 2.2 jdbc操作
<!--more-->
```
Connection conn = null;
try{
    //获得连接
    conn = JdbcUtils.getConnection();
    //开启事务
    conn.setAutoCommit(false);
    //提交事务
    conn.commit();
}catch(e){
    //回滚事务
    conn.rollback();
}finally{
    //释放资源
    conn.close();
}
```

## 2.3 DBUtils操作
```
Connection conn = null;
try{
	//获得连接
	
	//1 开启事务
	conn.setAutoCommit(false);

	//ABCD

	//2提交并关闭
	DbUtils.commitAndClose(conn);
} catch(e){
	//3回滚并关闭
	DbUtils.rollbackAndClose(conn);
}
```
## 2.4 ThreadLocal
在一个线程中共享数据。
- 操作：set(value) / get() / remove()

- 进程：java虚拟机，一个进程中可以有多个线程。例如：QQ软件、tomcat
- 线程：进程中一个执行单元。例如：QQ窗口

# 3 事务总结
## 3.1 特性ACID
- 原子性（Atomicity）
- 一致性（Consistency）
- 隔离性（Isolation）
- 持久性（Durability）
## 3.2 隔离问题
- 脏读
- 不可重复读
- 虚读/幻读
## 隔离级别
- read uncommitted
- read committed
- repeatable read
- aerializable