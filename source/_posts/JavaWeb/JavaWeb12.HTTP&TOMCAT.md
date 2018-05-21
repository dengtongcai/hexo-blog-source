layout: '[layout]'
title: JavaWeb_12_HTTP&Tomcat
date: 2013-11-23 15:22:43
categories: JavaWeb
tags: [笔记]
---
# 1.HTTP协议
## 1.1 访问过程
## 1.2 作用及特点
- 不深入了解HTTP协议，就不能说掌握了WEB开发。
- 特点：- 基于请求/响应模型的协议，请求和协议必须成对，先有请求再有响应，三次握手
- `默认端口：80`
- 版本：HTTP/1.0，发送请求，创建一次连接，获得一个WEB资源，断开连接
- `HTTP/1.1，发送请求，创建一次连接，获得多个WEB资源，断开连接`
- 组成：请求>请求行，请求头，请求体
- `响应>响应行，响应头，响应体`
        
## 1.3 HTTP协议入门
### 请求协议
- 请求行    
	- 格式:请求方式 资源路径 协议/版本
	- 包括GET方法的数据
<!--more-->
- 请求头
- 请求体
- 两种常用响应协议<br/>
 GET请求<br/>
![get](http://i.imgur.com/uaABjMY.png)<br/>
 POST请求<br/>
![post](http://i.imgur.com/HO10m0R.png)
### 响应协议
- 响应行    

格式：协议/版本   状态码  状态码对应信息

状态码 | 对应信息 | 常见
---|---|---
1xx | 请求开始 |
2xx | 响应已结束 | 200：表示正常
3xx | 服务器通知浏览器，表示单次请求没有结束，需要继续操作 | 302：服务器通知浏览器再次发送请求，即重定向；  304：读取缓存，服务器通知浏览器，浏览器已有缓存，不需要服务器响应，读缓存显示即可。
4xx | 浏览器端有错误 | 404：页面找不到；401：没有权限
5xx | 服务器端异常 | 500：服务端程序抛异常；

- 响应头
	- location :与302状态码组合重定向
	- Content-type :用于设置服务器响应体数据类型（编码）
	    `response.setHeader("content-type","text/html;charset=utf-8");`
	- content-disposition:下载
	- set-Cookie:与Cookie组合使用
	- last-modified：最后修改，与请求协议中if-modified-since比较，如果一致就响应304
	- refresh： 秒；url=路径；路径可以忽略，表示当前路径

- 响应体
- 响应协议格式
- 
![HTTP响应协议](http://i.imgur.com/zdZ2ydT.png)
# 2 tomcat
## 资源目录
![tomcat目录](http://i.imgur.com/J4FqXiL.png)
# 3 WEB项目
## 资源目录
![WEB项目](http://i.imgur.com/3qDbDx7.jpg)