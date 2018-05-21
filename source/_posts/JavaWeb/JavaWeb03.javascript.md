layout: '[layout]'
title: JavaWeb_03_Javascript
date: 2013-10-13 14:22:15
categories: JavaWeb
tags: [笔记]
---
# Javascript入门
## 1.javascript基本概述 ##
### 引入方法
- 内嵌式：通过`<script>`标签引入

```
<script type="text/javascript">
    javascript代码...
</script>
```

- 外联式：通过`<script src="">`标签引入.js文件

```
<script src="1.js" type="text/javascript" charset="UTF-8">
</script>
```
## 2.基本语法
### 2.1 变量
- javascript严格区分大小写
### 2.2 数据类型
<!-- more -->
- 基本数据类型
    - undefined (未初始化时默认值是undefined,null==undefined,但是含义不同)
    - boolean
    - number
    - string
    - object
- 引用类型
    - 默认值是null
- 运算符
    - ==(等于)只比较数值，不判断类型
    - ===(全等)比较数值和类型
### 2.3 基本操作
- 通过标签id获得对象,注意执行顺序。

```
var obj = document.getElementById();
```

- 通过innerHTML修改标签体的内容

```
obj.innerHTML="<a href='#'>你好</a>";
```
## 3. 注册页面简单js校验
### 3.1 表单提交事件

`<form onsubmit="boolean">
</form>`

### 3.2 轮播图
### 3.3 页面加载成功后，触发指定函数
 onload在body元素加载完成后加载

```
<body onload="函数">
</body>
```

### 3.4间隔时间触发函数

```
window.setInterval(函数名,间隔时间);
```
### 3.5 定时广告
- 显示/隐藏
使用display属性block/none
- 定时器

```
window.setTimeout(showAd,5000);
```

- 事件
	- click
	- submit
	- load
	- focus
- 事件绑定

方式1：等效
	
```
<body onload=""></body>等效
window.onload = init();
function init(){
        代码...
}
```

方式2：匿名函数

```
window.onload = function(){
document.forms[0].onsubmit = function(){
        代码...
    }
}
```

## 4.总结：BOM
### 分类
- BOM Window 窗口
- BOM History 历史纪录
- BOM Location 地址栏
- BOM Navigator 浏览器信息
- BOM Scaree 屏幕
### BOM Window
- 提示框：`window.alert();`
- 确认框：`window.confirm();返回true/false`
- 提示输入框：`window.prompt();取消=null` 
### BOM Location
- location.href="";
### BOM History
`back()==go(-1);
forward()==go(1);`
