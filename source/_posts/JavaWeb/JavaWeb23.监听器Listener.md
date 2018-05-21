layout: '[layout]'
title: JavaWeb_23_监听器Listener
date: 2014-01-16 14:57:32
categories: JavaWeb
tags: [笔记]
---
## 1.1 监听术语
- 事件源(被监听的对象)
- 监听器对象(用于监听"事件源"的对象)
- 监听器编写流程
    - 编写监听实现类,实现指定接口
    - 在web.xml中配置,绑定(注册)
```
<listener>
    <listener-class>路径</listener-class>
</listener>
```
<!--more-->
- 事件(事件源的某一行为)
- 事件对象(三个)

## 1.2 事件对象对应监听器接口
- Servlet context events
    - Lifecycle 			接口`javax.servlet.ServletContextListener `
    - Changes to attributes 接口`javax.servlet.ServletContextAttributeListener`
- HTTP session events
    - Lifecycle 			接口`javax.servlet.http.HttpSessionListener`
    - Changes to attribute  接口`javax.servlet.http.HttpSessionAttributeListener`
    - Session migration 	接口`javax.servlet.http.HttpSessionActivationListener`
    - Object binding    	接口`javax.servlet.http.HttpSessionBindingListener`
- ServletRequest
    - Lifecycle 			接口javax.servlet.ServletRequestListener
    - Changes to attribute  接口javax.servlet.ServletRequestAttributeListener
域对象属性
- 需要在web.xml文件中配置 <listener><listener-class> 
```
ServletContextAttributeListener
HttpSessionAttributeListener
ServletRequestAttributeListener
```
- 特殊JavaBean,不需要在web.xml文件配置
    `HttpSessionBindingListener`  绑定和解绑
    `HttpSessionActivationListener` 钝化或活化,需要实现接口 java.io.Serializable

## 1.3 监听servlet域对象的创建和销毁
### 监听servletContext， 接口ServletContextListener
- ServletContext对象
    - 创建：服务器启动时。创建时加载配置文件，初始化参数
	- 销毁：服务器关闭时。释放资源
监听器，用于监听ServletContext对象创建和销毁。监听器接口ServletContextListener

### 监听session，接口 HttpSessionListener
    - 创建：第一次调用 request.getSession()。如果使用jsp，底层自动调用getSession()
    - 销毁：
        - 1）30分钟，过期
        - 2）服务器非正常关闭
        - 3）手动调用方法，invalidate()
### 监听request，接口ServletRequestListener
    - 创建：每一次请求开始时
    - 销毁：请求结束时（响应结束时）

## 1.4 特殊javabean session作用域处理情况
### HttpSessionBindingListener 监听session作用域特殊javabean数据绑定和解绑。
- 此接口要求，不需要在web.xml配置，但javabean必须实现接口。

	- 绑定: javabean 添加到 属性中。setAttribute()
	- 解绑：javabean 从作用域移除。removeAttribute()
- 对比：HttpSessionAttributeListener 和 HttpSessionBindingListener
    - `HttpSessionAttributeListener` 监听session作用域对象的属性，作用域中存的数据。此处作用域中的数据没有限制。
    - `HttpSessionBindingListener` 监听session作用域 存放特殊javabean数据。此处只是部分实现接口的数据。
    - 应用实例：用户登陆成功，记录用户登陆时间、登陆ip地址等信息，不修改登陆功能代码。

- HttpSessionActivationListener接口，监听特殊javabean的钝化和活化的。
    - 钝化：将内存数据 保存 硬盘中。tomcat正常关闭时，将session 中数据保存硬盘。
    - 活化：从硬盘中 读取内容 加载内存中。
    
## 1.5 扩展
- JavaScript的事件,function(event){鼠标坐标 等}
- 设计模式:观察者设计模式
- 需要：在服务器启动时，执行定时器？如何在服务器启动时执行程序？
    - 方案1：Servlet，init初始化，web.xml <load-on-startup>
    - 方案2：监听器Listener
    - 方案3：过滤器Filter
    - 
## 2 java mail
会复制，能修改
- 1.连接：Session
- 2.消息：Message
- 3.发送：Transport
- 
## 3 定时器
JDK提供定时器，相当于一个线程

```
new Timer().schedule(1,2,3)
```

- 参数1：TimerTask 任务
- 参数2：延迟时间，单位毫秒
- 参数3：循环周期（轮回时间），单位毫秒