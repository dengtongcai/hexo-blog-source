layout: '[layout]'
title: JavaWeb_14_Response
date: 2013-12-06 10:32:12
categories: JavaWeb
tags: [笔记]
---
# 1 HttpServletResponse对象 extends ServletResponse
- 专门用来封装HTTP响应消息
## 1.1 状态码
- `setStatus(int status);`   设置状态码
- `sendError();`   设置错误状态
## 1.2 设置消息头
- 方法
```
setHeader();
setIntHeader();
setDateHeader();    set设置一个
addHeader();        add追加多个

```
- 实例：设置定时刷新

```
setHeader("refresh","5;url=...");
```
<!--more-->
- 重定向

```
手动方式
setHeader("location","url");
API方式
sendRedirect("url");
```
- 发送响应消息

```
getOutputStream();
    应用场景：字节（图片，音频，视频）
    返回值ServletOutputStream，提供print(),write(),但print()底层也是write(字符串)
getWriter();
    最常用，常用于响应数据
    应用场景：文本
    返回值类型：PrintWriter，提供print()和write()，建议直接使用write()
注意：以上两个流不能同时使用；
    println()中的ln在源码中换行，想响应后时HTML不换行
    println("1")；print("2") 源码中时两行，html网页中时一行
    print("1<br/>")；print("2")  源码中是一行，html中时两行
```
# 2.下载
## 2.1 超链接下载

```
如果浏览器能解析的就不会触发下载，不能解析的才会触发下载
```
## 2.2 servlet下载

```
必须设置响应头，否则还是会解析
response.setHeader("content-disposition","attachment;filename");
设置响应数据类型
response.setContentType("image/jpeg");
this.getServletContext().getMimeType("mm.jpg");此API获得的是指定文件的mime类型，结果："image/jpeg"
```
## 2.3 中文文件名处理
- 方式一：取巧方式
```
HTTP协议编码为ISO-8859-1,浏览器默认编码GBK,所以设定编码：
String imageName = new String(imageName.getBytes("GBK"),"ISO-8859-1");
```

- 方式二：给不同浏览器分别处理

```
IE和Chrome使用url编码，火狐使用base64编码
>>>获得浏览器标识
request.getHeader("User-Agent");
>>>ie和chrome
if(contains){
    imageNmae = URLEncoder.encode(imageName,"项目编码");
}
>>>火狐
if(contains){
    String str = new BASE64Encoder().encode(imageName.getBytes());
    //=?UTF-8?B?str?=       标准写法
    imageName = ""; 
}
```


