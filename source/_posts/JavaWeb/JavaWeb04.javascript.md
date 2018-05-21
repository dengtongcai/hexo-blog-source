layout: '[layout]'
title: JavaWeb_04_Javascript进阶
date: 2013-10-16 14:32:34
categories: JavaWeb
tags: [笔记]
---
# javacript进阶
## 1.表格隔行换色
### 1.1 获取所有行并隔行换色（标签）
- 代码
```
document.getElementsByTagName(标签名);
```
- 设置所需样式
### 1.2 鼠标进入变色，移出恢复
- 事件
    
```
obj.onmouseover=""
obj.onmouseout=""
```
- this:在函数内部代表当前操作的对象
- 为什么不能使用 allTr[i]?
- 变色之后鼠标移出要恢复原来颜色，需要先记录原颜色；也说明标签属性可以自定义,方法是：
```
obj.setAttribute(key,value);
```
<!-- more -->
## 2.全选
- 函数参数只需要写参数名
- 通过class属性获取所有对象 

```
var allCheckbox = document.getElementsByClassName(className);
```
- 在HTML代码中,checked被选中状态值为"checked"；js代码中，checked被选中状态为true/false。
## 3.省市二级联动
### 3.1 数组  Array
- 数组中数据类型没有任何限制，可以存放任何数据
- 数组的长度可以自动变化，没有越界一说，和java中的ArrayList一样

```
Array（size）
Array（元素）
```
### 3.2 API
- document.creatElement(标签名);
- Element.appendChild(子元素);
- document.creatTextNode();
### 3.3 简化写法

```
for(var i=0; i<allCity[index].length; i++){
    cityObj.innerHTML+=<option>allCity[index].[i]</option>
}
```
## 4.全局函数
- parseInt 
- parseFloat  转换
- eval()    处理字符串，生成js对象
