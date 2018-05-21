layout: '[layout]'
title: JavaWeb_05_jQuery
date: 2013-10-21 16:23:21
categories: JavaWeb
tags: [笔记]
---
# jQuery
## 1 引入和对象获取
### 1.1 导入jquery类库

```
<script type="text/javascript" src="../js/jquery-1.11.3.js">
</script>
```
### 1.2 获取对象
- 书写格式

```
jQuery(选择器),区分大写小写
$(选择器)
```
- 获取value值

```
$(选择器).val();
val()中没有值是获取，有值是赋值
```

<!-- more -->
### 1.3 jQuery对象和DOM对象转换
####  js的DOM对象转换成jQuery对象

```
var obj = document.getElementById("Id");
var $obj = $(obj);
```
#### jQuery对象转换成js对象
- jQuery对象内部是一个数组，用于存放一组对象

```
var obj1 = $obj[0];
var obj2 = $obj.get(0);
```

总结：jQuery对象和DOM对象API不能通用
## 2 基本操作
### 2.1页面加载
- 原始方式
```
<script type="text/javascript" >
    $(doucument).ready(
        function(){}
    );
</script>
```
- 简化方式

```
<script type="text/javascript" >
    $(function(){});
</script>
```
- js页面加载只能处理一次，写多个会覆盖；jQuery的页面加载可以写多个，顺序加载。
### 2.2基本选择器(重要)
- #id
事件绑定

```
$(function(){
$("#id").click(function(){      //添加鼠标点击事件
            $("#id1").css("backgroundColor","#ff0");     //调整样式
    }) 
});
```
- element
- .class
### 2.3 效果
- 显示/隐藏
    - show(){时间，函数};
    - hide(){};
    - toggle(){}:
- 滑动
    - slideDown(){};
    - slideUp(){};
    - slideToggle(){};
- 淡入/淡出
    - fadeIn(){};
    - fadeOut(){};
    - fadeToggle(){};
### 2.4 层级选择器
- A B--祖宗 后代
- A>B--父>所有子
- A+B--兄弟+一个兄弟
- A~ B--兄弟~ 后面所有兄弟
### 2.5 基本过滤选择器
- :first/last
- :not()
- :even /: odd -- 奇偶索引
- :eq()/:gt()/:lt() -- =/>/<
### 2.6 属性选择器
- [attribute]有属性名的元素
- [attribute=value]
- [attribute!=value]属性不为value
- [attribute^=value]以value开头
- [attribure$=value]以value结尾
- [attribute*=value]包含value
### 2.7 表单属性选择器
- :enabled
- :disabled
- :checked
- : selected
## 3.jQuery对象遍历

```
$(:selected).each(      //对数组遍历
    function(){
    	var h = $(this).html();
    	$("#selectDivId").append(h);
    }
);
```

```
$.each($(":selected"),function(){
    
    });
```
## 4.属性
属性
- prop
- removeProp
class类
- addClass
- removeClass
文本
- html()操作标签体时，会先解析
- text()操作标签体，以文本方式
## 5.文档处理
内部插入
- A.append(B)
- A.appendTo(B)
# 案例
## 1.重写弹出广告
- 使用效果函数
    - show/hide/toggle
    - slideDown/slideUp/slideToggle
    - 
## 2.重写隔行换色
- 基本过滤选择器
## 3.全选反选
- 表单属性选择器
- prop(属性);获取属性值
- prop(属性，值);属性赋值


$(this)与this的区别