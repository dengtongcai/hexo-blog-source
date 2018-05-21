layout: '[layout]'
title: JavaWeb_22_Ajax&jQuery Ajax
date: 2014-01-13 21:47:23
categories: JavaWeb
tags: [笔记]
---
## 1.1 GET请求
- 获得Ajax引擎
```
var xmlhttp = HttpRequest();
```

- 设置回调函数
```
xmlhttp.onreadystatechange = function(){
    xmlhttp.readyState==4
    xmlhttp.status==200
    xmlhttp.responseText
};
```
- 打开连接
```
xmlhttp.open("GET",url?params);

```
- 发送
```
xmlhttp.send(null);
```
## 1.2 POST请求
- 获得Ajax引擎
```
new XMLHttpRequest();
```

- 设置回调函数
```
xmlhttp.onreadystatechange = function(){
    xmlhttp.readyState==4
    xmlhttp.status==200
    xmlhttp.responseText
};
```

- 打开连接
```
xmlhttp.open("POST",url);
```

- 请求头
```
xmlhttp.setRequestHeader("content-Type","application/x-wwww-form-urlencode");
```

- 发送
```
xmlhttp.send(params);
```

# 2 jQuery Ajax
## 2.1 $.get()
格式:
```
$.get(url, params , fn , type)
```

- 参数1：请求路径
- 参数2：请求参数, 格式：{k:v , k:v ,...}
- 参数3：回调函数
    - 回调函数的第一个参数，表示"响应数据"
    - jQuery底层使用 javascript ajax引擎XMLHttpRequest,jQuery底层使用xmlhttp.responseText获得响应数据，然后jQuery调用回调函数时将数据传递给我们设置的函数
    
## 2.3 $.post()
格式:
```
$.post(url, params , fn , type)
```

- 参数1:url ，请求路径
- 参数2:params ，请求参数，使用json格式，{k:v,....}
- 参数3:fn ，回调函数，function(data){} 通过回调函数的第一个参数获得响应内容
- 参数4: jQuery将响应内容需要转换成的数据格式。例如：json、xml等
## 2.4 json数据格式

#案例错误
- jQuery 选择器书写
- jQuery Ajax 参数写键值对
- jQuery...