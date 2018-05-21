layout: '[layout]'
title: JavaWeb_24_过滤器Filter
date: 2014-01-22 16:34:23
categories: JavaWeb
tags: [笔记]
---
## 1.1 什么是过滤器?
- 过滤器是一个运行在服务端的程序,先于与之相关Servlet或JSP页面之前运行,实现对请求资源的过滤功能.
- 过滤器可附加到一个或多个Servlet获JSP页面上,可以检查请求信息,也可以处理响应信息.
- Filter的基本功能是对Servlet容器调用Servlet的过程进行拦截,从而在Servlet执行前后实现一些特殊的功能.

## 1.2 过滤器常用实例
自动登录,解决全站乱码,屏蔽非法文字,进行响应数据压缩
<!--more-->
## 1.3 过滤器编写流程
- 实现类,需要实现接口 javax.servlet.Filter
```
放行:chain.doFilter(request,response);
```

- 配置,在web.xml中注册
```
<filter>    		用于注册过滤器
    <filter-name>   给注册的过滤器进行命名，名称自定义（常用类名）
    <filter-class>  注册过滤器实现类（选限定类名）
</filter>
<filter-mapping>    给已经注册的过滤器添加过滤路径映射
    <filter-name>   已经注册的过滤器名称
    <url-pattern>   需要被过滤，资源路径
</filter-mapping>
```

## 1.4 生命周期
- init(FilterConfig) , 初始化方法，服务器启动自动执行。
- doFilter(ServletRequest,ServletResponse , FilterChain) 过滤方法，对每一个符号匹配路径都进行过滤
- destroy()  服务器关闭时销毁

## 1.5 FilterConfig对象
- web.xml 文件中进行初始化参数配置
- 
```
<filter>
	<filter-name>
	<filter-class>
	<init-param>
		<param-name>
		<param-value>
	</init-param>
</filter>
<filter-mapping>  
	<filter-name>encodingfilter</filter-name>  
	<url-pattern>/*</url-pattern>  
</filter-mapping>
```

- api
	`filterConfig.getFilterName()`
	`filterConfig.getInitParameter()`
	`filterConfig.getServletContext()`

## 1.6 FilterChain过滤器链
- 放行方法：
	`chain.doFilter(request ,response);`
- 过滤器，多个过滤器执行顺序：web.xml `<filter-mapping>`的顺序

## 1.7 dispatcher配置
-  默认值，表示请求开始时连接
- forward 表示请求转发开始时拦截
- include 表示请求包含开始时拦截
- error 表示显示错误处理页（友好页面）开始时拦截

## 1.8 request和response装饰者设计模式基类
- request --> HttpServletRequestWrapper ,采用装饰者设计模式，编写实现类，类中所有的方法都不增强。

- response --> HttpServletResponseWrapper 

## 总结
服务器启动时，加载配置文件
- 1.servlet ，在init(..)编写代码，然后web.xml 配置 <load-on-startup>
- 2.listener , 实现 ServletContextListener接口，在服务器启动时调用 初始化方法
- 3.filter，在init(...)编写代码，然后web.xml只要注册过滤器，tomcat启动时，自动加载。