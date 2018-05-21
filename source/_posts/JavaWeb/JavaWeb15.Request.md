layout: '[layout]'
title: JavaWeb_15_Request
date: 2013-12-13 10:32:12
categories: JavaWeb
tags: [笔记]
---
# 1.常用方法
- WEB项目名
```
requeset.getContextPath();
```
- 请求方式
```
request.getMethod();
```

- ip地址
```
request.getRemoteAddr();
```

- servlet访问路径
```
request.getServletPath();
```
<!--more-->
# 2. 请求参数
##  方法
- 获得一个值
```
String request.getParameter(name);
```
- 获得一组值
```
String[] request.getParameterValues(name);
```
- 获得所有值
```
Map<String,String[]>  request.getParmeterMap();
```

## 乱码解决
- post乱码
```
request.setContentType("text/html;charset=utf-8");
```

- get乱码
```
new String(xxx.getBytes("ISO-8859-1"),"UTF-8");
```
ISO-8859-1   , iso-8859-1   , ISO8859-1, iso8859-1  表示同一种字符集
# 3. 请求转发
## 操作数据
- 存放
```
request.setAttribute(name,value);
```

- 获得
```
request.getAttribute(name);
```

- 移除
```
request.removeAttribute(name);
```

## 请求转发

```
request.getRequestDispatcher(path).forward(HttpServletRequest request, HttpServletResponse response);
```
特点：从A请求转发B页面，响应给浏览器的时B页面的内容（即最后一个页面）。
# 4. 请求头
- 获得一个
```
request.getHeader(name);
```

- 获得一组
```
request.getHeaders(name);
```
