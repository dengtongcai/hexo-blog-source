layout: '[layout]'
title: JavaWeb_02_html&css
date: 2013-10-08 16:12:34
categories: JavaWeb
tags: [笔记]
---
# HTML表单&css
## 表单标签`<form>`
### 1. 表单标签在浏览器中没有任何显示；
### 2. 如果需要提交数据，表单必须存在`<form>`标签内；
### 3. action-表单提交到服务器端程序的路径
### 4. method-提交常用方式：
- get-将请求参数追加在请求路劲上，注意其格式，请求路劲长度有限
- post-请求参数不可见（安全），请求参数个数不限制
### 5. 子标签`<input>`
<!-- more -->
- type属性
 	
	- hidden 隐藏域，浏览器不显示，但可以提交(程序员需要传递一些不希望用户看到的数据)
    - text 文本框(默认)
    - password 密码框
    - radio 单选框(给多个单选设置相同name值)
    - checkbox 复选框
    - submit 提交按钮
    - reset 重置，将表单成员恢复默认值(value)
    - date HTML5标签，必须保留html注释<！DOCTYPE html>才有效
    - file 文件上传
    - image 图形提交按钮
    - button 普通按钮
- name属性
	- 要提交到服务器的数据，都要有name属性，提交按钮则不需要
- value属性
	- 提交到服务器的数据
- checked属性
	- 单选或复选中表示选中，标准写法checked="checked"
- size属性
	- 文本框长度
- maxlength属性
	- 可以输入的最大长度
- readonly属性
	- 只读，浏览器不能修改，回提交到服务器
- disabled属性
	- 不可用
### 6. 子标签"select"
	- 多选 `<select multiple="multiple">`
	- 默认长度 `<select size="4">`
	- 默认选中 `<option selected="selected">`
```
<select name=""(key) multiple="" size="">
        <option value=""(value)>...</option>
        ...
</select>
```
### 7. 子标签"textarea"
	    <textarea name="" rows="" cols="">

## div+css重写首页
## div
- 一个普通标签，进行区域划分。
- 特性：独自占一行，必须结合css进行渲染
- <div id="" class="">使用id和class标记是div更加有效
## css
### 1.样式规则：
- 选择器{属性1：属性值1;属性2：属性值2...}
### 2.特点：
- css样式选择器严格区分大小写，属性和属性值不区分大小写
- 属性值有多个单词单词和空格组成，需要""
- 为提高可读性，一般要为css添加注释 /* 注释内容*/
- css不解析空格。
- 属性值与单位之间不可出现空格 font-size:20 px为错误写法

### 3.css引入方式(优先级：就近原则)
- 行内样式  标签带style属性

```
<div style="color:red;size:4>
    ...
</div>
```

- 内部样式  在"head"标签内，使用`<style type="text/css">`标签

```
<head>
    <style type="text/css">
    div{...}
    </style>
</head>
```

- 外部样式  将css样式引入到html文件，固定写法`<link rel="stylesheet" type="text/css" href=""/>`

```
<head>
    <link rel="stylesheet" type="text/css" href=""/>
    ...
</head>
```
### 4.选择器
- 元素选择器

```
<style type="text/css">
    div{...}
</style>
```

- id选择器  `每个标签都有id属性，整个页面唯一；通常提供css和javascript使用`

```
<head>
    <style>
        #id名字{...}
    </style>
</head>
<body>
    
</body>
```
- 类选择器  `.类名{....}`

```
<head>
    <style>
        .class名字{...}
    </style>
</head>
```
- 属性选择器   `标签名[标签属性="标签属性值"]{...}`

```
<head>
    <style>
        input[type="text"]{...}
    </style>
</head>
```
- 包含选择器    `父标签  后代标签{...}`

```
<head>
    <style>
        #d1 div{...}
    </style>
</head>
```
### 5.布局
- 边框和尺寸：`width height  border：1px solid #ff0`
- 浮动：`float clear：left right both`
- 元素转换：`display：inline(默认)`
    - inline    `行内元素，高和宽无效<span><a>`
    - block     `块元素，<h1><div><ul>`
    - inline-block  `行内块级元素，高和宽有效`
    - none      `元素被隐藏，不显示，不占用页面空间`
    
### 6.盒子模型
- margin    外边距，边框之间(上下左右；上=下，左=右)
- padding   内边距，内容与边框