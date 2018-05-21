layout: '[layout]'
title: JavaWeb_16_Cookie&Session会话技术
date: 2013-12-16 10:32:12
categories: JavaWeb
tags: [笔记]
---
# 1.概念
## 1.1 会话技术：一次会话（多次请求）中共享数据。
- 客户端技术Cookie
- 服务端技术Session
## 1.2 什么是Cookie（细节）
- 与缓存的区别

```
浏览器缓存可以缓存任意内容，Cookie只是服务器需要浏览器缓存的数据
```
- 不能设置中文（请求头，响应头，默认编码IOS-8859-1）
- 设置数据有限，最大4KB
- 每一个站点个数限制
- 一个浏览器个数限制
<!--more-->
## 1.3 API
- 创建

```
Cookie c = new Cookie(name,value);
有限时间,如果有效时间为0，则为删除操作
cookie.setMaxAge(...);
路径
cookie.setPath(...);
```

- 响应浏览器

```
response.addCookie();
```

- 从请求中获得所有Cookie
```
Cookie[] cookies = request.getCookies();    如果没有返回null
```
## 1.4 路径使用

```
cookie.setPath("/day16/demo");  //表示day16 项目下,【demo目录】下所有的servlet，都可以访问当前cookie
cookie.setPath("/day16");       //表示【day16 项目】下的所有servlet都可以访问当前cookie
cookie.setPath("/");            //表示【tomcat下】的所有的web项目，都可以访问当前cookie
```
# 2 Session
## 2.1 工作方式

```
*当浏览器访问WEB服务器时，Servlet容器就会创建一个==Session对象==和==ID属性==
*当客户端后续访问服务器时，只要将ID传递给服务器，服务器就能判断出是哪个客户端发送的请求
*从而选择对应的Session对象为其服务
```

```
*Cookie由大小和个数的限制，Session没有大小和个数的限制
*Cookie相对于Session是不安全的
```
## 2.2 获得流程

```
第一次访问，创建/之后每一次访问，获取
request.getSession();
request.getSession(boolean create);
    getSession() 相当于 getSession(true)
	getSession(false) 如果有就使用，如果没有返回null。
```
## 2.3 生命周期
- 创建，第一次请求
- 销毁
```
*过期（默认30分钟）：web.xml中可以配置<session-config><session-timeout>
*手动销毁：invalidate();
*tomcat非正常关闭（正常关闭使Session被序列化）
```
## 2.4 作用域

```
Servlet 3 个作用域对象：ServletRequest 、 HttpSession  、 ServletContext 
Servlet 3个作用域：request、 session、 servletContext
```


```
1. 作用域：在一定的范围内共享数据。
2. 作用范围：
* request：一次请求中共享数据。默认情况一次请求就一个页面。如果使用“请求转发”可以在多个页面之间共享数据（服务器内部一个项目中）。
* session：一次会话中共享数据。一次会话可以有多次请求。（每一个浏览器内部）
* servletContext：一个项目中共享数据。一个项目可以有多次会话。（所有浏览器）

3. 开发中如何选择作用域？
* 能小不大原则。
* 需求优先原则。
```


问题：需要在两次请求之间共享数据，选择（session）