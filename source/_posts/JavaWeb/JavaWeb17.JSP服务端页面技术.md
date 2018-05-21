layout: '[layout]'
title: JavaWeb_17_JSP服务端技术
date: 2013-12-23 11:23:44
categories: JavaWeb
tags: [笔记]
---
# 1. JSP页面显示商品信息
## 1.1 JSP概述

```
JSP即Java Server Pages，是建立在Servlet规范之上的动态网页开发技术；
```
* 跨平台
* 业务代码分离
* 组件重用
* 预编译
* JSP执行顺序

```
工作模式：请求/响应模式
客户端请求——>JSP转换成Servlet源程序——>显示内容
```
## 1.2 JSP脚本元素
* 脚本声明
<!--more-->
```
<%! java类中任意内容 %>
```
* 代码片段
```
<% java方法体中任意内容 %>  将代码块直接添加到service()方法体内
service(){ 片段;}
```
* 表达式
```
<%= 表达式(方法实际参数) %> 将结果输出到浏览器，out.print(表达式)
```
## 1.3 JSP注释
```
<%--  任意内容  --%>    在开发中，JSP页面的注释建议都使用JSP注释
```
* 其他注释
```
<!--  html注释 -->
<%
    //java注释
    /*java 多上注释*/
%>
```
## 1.4 JSP指令
### page
* 设置编码
```
<%@page
    pageEncoding="UTF-8"                    当前页面编码
    contentType="text/html;chatset=UTF-8"   响应给浏览器的编码
%>
```

* 缓存
```
<%@page
    buffer="8kb"         JSP输出缓存大小，默认8KB
    autoflush="true"    缓存满了是否自动刷新
%>
```

* 异常处理
```
发生异常页面：<%@page errorPage="/error.jsp"%>  用于配置错误页面及JSP异常时显示页面
异常处理页面：<%@page isErrorPage="true"%>      自己是否时错误页面，用于处理错误
```
错误页面处理
```
web项目，可以统一处理错误信息，在web.xml文件进行配置
方式1：设置错误状态码 （常见）
		<error-page>
			 <error-code>500</error-code>
			 <location>/500.jsp</location>
		</error-page>
 

方式2：设置异常信息
		<error-page>
			<exception-type>java.lang.ArithmeticException</exception-type>
			<location>/error.jsp</location>
		</error-page>
```

* 其他
```
language:JSP页面允许插入脚本语言语种
import:JSP导入其他包
session:控制JSP页面session内置对象是否可以使用
```
### include
* 静态包含
```
a.jsp和b.jsp连个页面合并在一起，生成一个Servlet，再响应给浏览器
<%@include file="/"%>
```

* 动态包含
```
a.jsp和b.jsp分别生成两个Servlet，响应给浏览器，再将内容合并在一起
<jsp:include page="/">
```
* 路径问题

```
1）浏览器发送给服务器：一般情况下所有路径都需要/开头，填写web项目的名称。
/开头表示站点根（tomcat/webapps/..)
	<a href="/项目名/路径">
```

```
2）服务器发向给服务器：不需要项目名，项目内部。
/开头表示项目根（tomcat/webapps/项目名/..)
* servletContext.getRealPath("/")
* 请求转发：request.getRequestDispatcher("/")
* jsp包含
```

## 1.5 JSP内置对象
* page
* config
* application
* request
* response
* session
* out
* exception
* pageContext
```
引入其他八个
```

```
简化作用域
setAttribute(name,value) page作用域设置
getAttribute(name) page作用域获得
removeAttribute(name) 移除4个

setAttribute(name,value,scope)  指定作用域设置
getAttribute(name,scope)指定作用域获得
removeAttribute(name,scope)指定作用域移除
```

```
依次获得
findAttribute(name)  --> page/request/session/application
```
## 1.6 JSP作用域
* page：表示当前页，通常没用。jsp标签底层使用。
* request：表示一次请求。通常一次请求就一个页面，但如果使用请求转发，可以涉及多个页面。
* session：表示一次会话。可以在多次请求之间共享数据。
* application：表示 一个web应用(项目)。可以整个web项目共享，多次会话共享数据。
* 常用的常量，表示不同的作用域
	
```
PageContext.PAGE_SCOPE
PageContext.REQUEST_SCOPE
PageContext.SESSION_SCOPE	PageContext.APPLICATION_SCOPE
```
## 1.7 JSP动作标签
## 1.8 知识点
- 缓存区
- 设置属性作用域优先级
