layout: '[layout]'
title: JavaWeb_01_html
date: 2013-09-26 11:56:30
categories: JavaWeb
tags: [笔记]
---
# HTML
## 1. `<frameset>`标签，不能`<body>`标签一起出现，否则会失效。
 - rows-上下结构，多个值使用逗号分开，*代表余下所有；
 - cols-左右结构；
 - target-重定向，使用name-标记要重定向的框架。

```
<frameset rows="" 或者 cols="">
    <frame name=""/>
</frameset>
```
## 2. 网络爬虫解析标签`<title>`和`<h1>`,故一个页面一般只有一个`<h1>`标签。

- `<table>`标签固定格式：
<!-- more -->

![表格](http://i.imgur.com/fGzvKrM.jpg)
<table>
    <tr>
        <td rowspan="" 或者 colspan=""> 
        单元格内容，可嵌套表格  
        </td>
    </tr>
</table>
- cellpadding:边框与内容之间的距离
- cellspacing：单元格之间的距离。

## 3.`<td>`标签：

- rowspan-行合并
- colspan-列合并

## 4. 列表
`<ol>
    <li>有序列表</li>
</ol>`
`<ul>
    <li>无序列表</li>
</ul>`

