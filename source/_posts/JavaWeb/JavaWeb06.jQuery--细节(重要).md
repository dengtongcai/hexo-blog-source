layout: '[layout]'
title: JavaWeb_06_jQuery细节
date: 2013-10-27 11:23:45
categories: JavaWeb
tags: [笔记]
---
# jQuery知识点细化
## 1.省市二级联动
### 1.1 `each();`
```
var arr1 = new Array();
$(arr1).each(function(index,ele){
    
});
```
##### javascript中没有重载的概念，只需要定义一个函数，他的参数可以任意改变。调用时与参数无关。通过arguments属性可以获得传入的所有的参数。
```
function init(){
    alert(arguments[1]);
}
$(arr1).each(function(index,ele){
    
});
```
<!--more-->
### 1.2 HTML代码/文本/值

- `html();`

	- 赋值时：会先解析标签代码
	- 获取值时：会获取带标签的代码

- `text();`

	- 赋值时：不解析，直接赋值
	- 获取值时：只获取文本，不获取标签

- `val();`


	- 赋值时：给一组参数同时设置相同值
	- 获取值时：只获取第一个值。
## 2.左右选择
### 2.1 添加元素到后面

```
A.append(B);
B.appendTo(A);
```

### 2.2 前面添加元素

```
A.prepend(B);
B.prependTo(A);
```

### 2.3 顺序添加

```
A.after(B);       //将B元素插入到A元素后面
B.before(A);      //将A元素插入到A元素前面
```
## 3 链式操作
## 4 表单校验
### 4.1 案例分析
- 导入校验包
- 手动处理，加载校验包

```
$(function(){
    $("#formId").validate();
});
```
- 导入国际化
- 选择校验方式
### 4.2 校验方式

校验类型 | 取值 | 描述
---|---|---
required | true/false| 必填字段
email | email| 邮件地址
url | /| 路径
date | 数字| 日期
dateISO | 字符串| 日期(YYYY-MM-dd)
number | /| 数字(整数，小数)
digits | /| 整数
minlength | 数字| 最小长度
rangelength | 数字| 最大长度
min | /| 最小值
max | /| 最大值
range | [min,max]| 值范围
equalTo | jQuery表达式| 两个值相同
remote | url路径| ajax校验


- class校验
    
```
<标签名 class="校验规则"><>
```

- 属性校验
```
<标签名 校验规则="校验值"><>
```

- data校验
    jQuery有专门的data()函数，用户处理data校验方式
```
$().data(规则名，多个去掉-);    //获取规则名
<标签名 data-rule-规则名-校验规则 data-rule-规则名-校验规则 ...><>
```

- static_js校验
```
$(function(){
    $("#formId").validate({
    //设定规则
        rules:{
            name:校验规则,      //简化版
            name:{校验规则：校验值,     //标准版
                校验规则：校验值,
            },
            ...
        },
     //设置提示消息   
        messages:{
            name:{
                required:"用户名必须填"
            }
        }
    });
});
```

