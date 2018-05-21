layout: '[layout]'
title: JavaWeb_11_XML&反射
date: 2013-11-15 13:22:12
categories: JavaWeb
tags: [笔记]
---
# 1.模拟servlet
## 1.1 XML
- 可扩展标记语言，与HTML相似，但是XML元素可以由用户自己定义。
- 配置文件
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xmlns="http://java.sun.com/xml/ns/javaee" 
xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" 
id="WebApp_ID" version="2.5">
<servlet>
	<description></description>
    <display-name>ProductFindAllServlet</display-name>
    <servlet-name>ProductFindAllServlet</servlet-name>
    <servlet-class>com.heima.web.servlet.ProductFindAllServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>ProductFindAllServlet</servlet-name>
    <url-pattern>/ProductFindAllServlet</url-pattern>
</servlet-mapping>
```
## 1.2 语法
- 文档声明
```
0行0列，开头不能有空格
<?xml version="1.0" encoding="UTF-8"?>
```
<!--more-->
- 元素
格式化良好的XML，必须只能有一个根元素
```
元素命名：
区分大小写
不能使用空格，冒号
不建议以xml开头
```
- 属性
不能使用空格冒号，必须是字母开头
<web-app version="2.5">
<!--more-->
- 注释
```
与HTML注释相同
<!-- 注释-->以--结束
```
- 转义字符

符号 | 转义代码
---|---
> | \&gt;
< | \&lt;
& | \&amp;

- CDATA区
```
<![CDATA[
    文本，特殊不转义  
]]>
```

## 1.2 DTD 约束
### 概述
- 常见XML约束：DTD/Schema
- 约束XML，规定XML中元素名，子元素及元素顺序
### 语法
- DTD文档声明，直接复制
- 元素写法
```
遵循DTD文件即可 
```
### 语法(了解)
- 文档声明
```
//内部DTD
<!DOCTYPE >
//外部DTD
<!DOCTYPE 根元素名 SYSTEM "本地地址">
//公共DTD
<!DOCTYPE 根元素名 PUBLIC "名字""网络地址">
```
- 元素声明

```
<!ELEMENT 元素名 元素描述>
元素描述包括：符号，数据类型
常见类型：#PCDATA 只能是文本，不能是标签

<!ELEMENT web-app (servlet*,servlet-mapping* , welcome-file-list?) >
<!ELEMENT servlet (description?,servlet-name,(servlet-class|jsp-file))>
<!ELEMENT servlet-mapping (servlet-name,url-pattern+) >
<!ELEMENT servlet-name (#PCDATA)>
<!ELEMENT servlet-class (#PCDATA)>
```
符号| 描述
---|---
？ | 出现0或者1次
* | 不限制
+ | 最少出现一次
() | 给元素分组
丨 | 在()列出的对象中选择一个
, | 表示对象必须按指定的的顺序出现

- 属性声明

```
元素名：属性必须是给元素添加
属性名：自定义
类型：ID,CDATA,枚举...
    ID：标识属性唯一性
    CDATA：文本类型
    枚举：（e1|e2|e3...）多选一
约束：
    #REQUIRED   必须
    #IMPLIED    可选
<!ATTLIST 元素名
属性名 属性类型 约束
属性名 属性类型 约束
>
<!ATTLIST web-app 
	version CDATA #IMPLIED 
	wid ID #REQUIRED>
```

## 1.3 Schema约束
### 概述
- 新得XML文档约束，本身就是XML，但是扩展名为xsd
- 比DTD强大很多，是DTD的替代者
- 支持命名空间
### 重点要求
- 文档声明
```
<xxx xmlns="..."
     xmlns:xsi="http://wwww.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="名称 位置 名称2 位置2..."
>
```
- 命名空间

```
使用一个名称编辑xsd文档，从而在xml中对不同的xsd进行划分
A.xsd <table> 表格
B.xsd <table> 桌子

格式：  xmlns (xml namespace)
1,默认：  xmlns="名称A"
    使用：<table>
2,显示：  xmlns：别名="名称"
    xmlns:a="...A"
    xmlns:b="...B"
    使用：<a:table> <b：table>
    
目的：需要使用约束文档(demo.xsd)来约束xml文档(demo.xml)
1，xsd文档确定"名称" ##>命名空间
    targetNamespace="http://www.example.org/demo"
    名称自定义，要求全球唯一，开发中采用URL
2，在xml中使用 xsd的"名称"
    xmlns=""
3，固定值
    xmlns:xsi="http://wwww.w3.org/2001/XMLSchema-instance"
4，确定schema文档的位置
    xsi:schemaLocation="名称 位置 名称2 位置2..."
```
- 语法


## 1.4 dom4j解析
### DOM
- 常规方式
```
核心类 SAXReader
获得 document >> read(is)
获得根元素 getRootElement()
获得所有 elements()
获得一个 element()
获得文本 elementText()
元素的属性值 atrributeValue("属性")
```

- XPATH
```
语法
/完整路径
//
@
//title[@id='leng']
```

### SAX
```
速度快，只读，读少占多少内存
```

### PULL(Android内置XML解析方式，类似SAX)
### 解析开发包
- dom4j

```
selectNodes()获得所有节点
selectSingleNode() 获得一个节点
```

## 1.5 反射（java.lang.reflect）
### 概念

```
在运行状态中，对于任意一个类，任意一个对象，都能调用他的任意一个方法和属性：类，构造，方法

```
### 获取对象（Class对象，Method对象，Constructor对象）
- Class.forName("类路径");
- 类名.class
- obj.getClass()
# 2.案例实现