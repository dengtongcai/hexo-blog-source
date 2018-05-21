layout: '[layout]'
title: JavaWeb_18_JSP模式&EL&JSTL
date: 2013-12-27 14:48:58
categories: JavaWeb
tags: [笔记]
---
# 1 EL表达式
## 1.1 基本语法
`${ 表达式 }` 不要有空格,获得数据必须在作用域中,操作运算符：`.` 或`[]`

## 1.2 内置对象
- 作用域
```
pageScope
requestScope
sessionScope
applicationScope
```
- pageContext
```
pageContext字符串对应pageContext jsp内置对象
* el：${pageContext.request.contextPath}
*jsp:<%=((HttpServletRequest)pageContext.getRequest).getContextPath()%>
```
<!--more-->
- 请求参数
```
param
paramValues
```
- 请求头
```
Header
HeaderValues
```
- cookie
```
cookie-->map
${cookie.名称.value}    获取对应的值
```
- 系统初始化参数
```
initParam
```

## 1.3 运算符
- 算数
```
+ - * / %
${"1" + 1  }  将算法两边的内容，转换成数字
/ 没有整除，底层使用double
```

- 条件
```
>   gt
<   lt
>=  ge
<=  le
==  eq

```

- 逻辑
```
&&  and
||  or
！  not
```

- 三元
```
xx ? "T" : "F"
xx ? " checked='checked' " : ""
```

- empty
```
null
""
size()==0
```

# 2 JSTL
## 2.1 导入

```
<%@taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
```
## 2.2 if语句

```
<c:if test="条件">
</c:if>
```
## 2.3 forEach语句

```
<c:forEach items="集合" var="每一项">
	${每一项}
</c:forEach>
```
```
<c:forEach begin="" end="" step="" var="每一项">
	${每一项}
</c:forEach>

类似java中
for( int var = begin ; var <= end ; var += step ){
}
```


# 3 JSP设计模式
## 3.1 MVC设计模式
软件设计模式：业务逻辑处理 与 数据显示  相分离
* Model模型
* View 视图
* Controller控制器
## 3.2 JSP设计模式
- 模式1

```
JSP + JavaBean
```

- 模式2
```
servlet + jsp + javabean
  (c)     (v)     (m)

模式2是 MVC在java中具体体现
```
## 3.3 三层体系架构
将服务器的代码逻辑认为的分成3层。
- web层：表示层，主要完成：获得数据，已经显示数据
- service层：业务逻辑层，主要完成：处理数据
- dao层：数据访问层，主要完成：持久化数据