layout: '[layout]'
title: JavaWeb_13_Servlet
date: 2013-12-01 10:32:12
categories: JavaWeb
tags: [笔记]
---
# 1.概念
## 1.1 作用
- 接收参数
- 处理数据
- 响应数据

## 1.2 接口

```
javax.servlet.http.HttpServlet
```
## 1.3 处理数据xml

```
<servlet>                               注册servlet
    <servlet-nanme></servlet-nanme>     serverlet名称，当前xml中唯一
    <servlet-class></servlet-class>     servlet实现类的全限定名称
</servlet>
<servlet-mapping>                       给注册的servlet添加映射路径
    <servlet-name></servlet-name>       已经注册的servlet名称，必须与注册的名称一致
    <url-pattern></url-pattern>         访问路径，必须以/开头
</servlet-mapping>
```
<!--more-->
## 1.4 获取浏览器数据

```
getParameter("name");       获取浏览器参数
PrintWriter out = response.getWriter();     获取输出流
response.setHeader("content-type","type-html;charset=utf-8");   设置响应头，使浏览器以HTML处理代码
```
## 1.5 用户登录案例
- 创建数据库和表
- 导入jar包
- 工具类jdbcUtils
- javaBean
- 完善login.html
- 编写servlet
- 编写service提供login方法
- 编写dao提供查询数据库方法

# 2.Servlet相关
## servlet规范
- JavaEE规定，servlet必须实现Servelt接口`javax.servlet.Servlet`;
- 提供两个实现类：
    - GenericServlet,通用servlet,与协议无关的实现
    - HttpServlet,与HTTP协议有关的实现类
## 2.1 Servlet生命周期(面试)
- Servlet接口中定义三个方法，作为生命周期的方法，调用者为WEB服务器（tomcat）
1,`init(ServletConfig)`初始化方法
    调用时机：默认情况下，是第一次调用service之前；
    如果修改web.xml,可以是服务器启动时，<servlet><loas-on-startup>

2,`service(ServletRequest，ServletResponse)`执行方法
    调用时机：每一次请求时都调用一次

3,`destroy()`销毁方法
    调用时机：服务器正常关闭时


## 2.2 体系结构
### Servlet接口
- init
- service
- desteoy
- getServletConfig()
### GenericServlet 实现类(抽象类，没有实现service方法，表示与协议无关)
- 提供一个成员变量，存放init(ServletConfig)实际参数，并使用getServletConfig返回
- 初始化方法调用中，没有参数初始化init()    
    - 结论：servlet实现类中，如果要完成初始化功能，建议不要复写init(ServletConfig)方法。而是复写init()
    - 原因：如果子类复写init(ServletConfig)，父类的方法就不执行，父类中的this.config没有赋值，之后再使用getServletConfig()不能获得数据，将抛空指针异常，解决办法：
    
```
public void init(ServletConfig config){
    this.config = config;
    this.init();
}
```
### HttpServlet实现类
- 先执行，在`service(ServletRequest,ServletResponse)`方法中进行强转，并调用service(Http...)
- `service(HttpServletRequest,HttpServletResponse)`方法内部，根据请求方式不同，执行不同的doXXX方法。
## 2.3 servlet执行顺序

## 2.5 ServletConfig 对象
- 定义：当前servlet配置信息封装对象
- 1，获得servlet名称
```
config.getServletName();
```
- 2，获得初始化参数
```
config.getInitParameter(name);
```
- 3，获得ServletContext对象引用
```
config.getServletContext();
```

## 2.6 配置
- servletconfig配置

```
<servlet>
	<servlet-name></servlet-name>
	<servlet-class></servlet-class>
<init-param>
	<param-name>        初始化参数
	<param-value>
	</init-param>
	<load-on-startup>       初始化方法开机执行
<servlet-mappnig>
	<servlet-name>
	<url-pattern>
```

- url-pattern配置

    - 1.完全匹配		`/a/b/hello`
    - 2.不完全匹配	`/a/b/*` ,常用`/*`
    - 3.扩展名匹配	`*.jsp`
    - 4.缺省路径		`/`
# 3.ServletContext对象
## 3.1  概念
- 定义：servlet的上下文对象（管理者），一个web项目对应一个servletContext对象，在tomcat启动时，由tomcat创建
- 目的：在整个web项目中共享数据
## 3.2 ServletContext对象获取

```
this.getServletConfig.getServletContext();
this.getServletContext();
```

## 3.3 共享数据

```
setAttribute(name,value) 设置内容，相当于map.put(key,value)
getAttribute(name) ,获得内容，相当于map.get(key)
removeAttribute(name) 移除内容，相当于map.remove(key)
```
## 3.4 获得资源

```
getRealPath(),获得tomcat下真实路径（绝对路径），获得字符串
getResourceAsStream(),获得realPath对应资源的流
```
## 3.5 初始化参数

```
servletContext.getInitParameter(name);
```
web.xml配置

```
<context-param>
    <param-name>
    <param-value>
```

# servlet执行顺序
# web.xml配置