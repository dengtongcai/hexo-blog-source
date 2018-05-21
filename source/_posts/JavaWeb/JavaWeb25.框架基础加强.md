layout: '[layout]'
title: JavaWeb_25_框架基础
date: 2014-01-26 21:47:23
categories: JavaWeb
tags: [笔记]
---
## 1 注解
### 1.1 概述
### 1.2 JDK提供的注解
- @Override JDK5.0表示复写父类的方法,JDK6.0还可以表示实现接口方法.
- @Deprecated   表示被修饰的方法过时了,过时的方法可以正常使用但不建议使用,存在安全问题或被新的方法取代
- @SuppressWarnings 抑制警告,通过编译器被修饰的对象内部不需要进行检查.
    - rawtypes:忽略类型安全
    - unused:忽略未被使用
    - null:忽略空指针
    - serial:忽略序列化
    - deprecation:忽略过时
    - uncheck:忽略未检测
    - all:忽略所有

### 1.3 自定义注解
#### 给注解类添加属性
- 格式: 修饰符 返回值类型 属性名()[default 默认值]
    - 修饰符:只能是 public abstract,不写默认此
    - 返回值类型:基本类型,字符串,Class,注解,枚举
    - 属性名:自定义
    - default   默认值:可以省略

#### 注意事项
- 注解可以没有属性,如果有属性需要使用小括号扩住.@MyAnno1 或 @MyAnno1()
- 属性格式:属性名=属性值,多个属性用逗号分隔.@MyAnno2(username="rose",password="1234")
- 如果属性名为value,且当前只有一个属性,value可以省略.
- 如果使用多个属性,key的名称为value不能省略.
- 如果属性类型为数组,设置内容格式为:{1,2,3}.arrs={"jack","rose"}
- 如果属性类型为数组,值只有一个,{}可以省略.arrs="jack"
- 一个对象上,注解只能使用一次,不能重复使用.

#### 解析
如果给类,方法等添加注解,如果需要获得注解上设置的数据,那么我们就必须对注解进行解析,JDK提供java.lang.reflect.AnnotatedElement接口允许运行时通过反射获得注解.<br/>
常用方法
- 判断当前对象是否有注解    boolean isAnnotationPresent(Class annotationClass)
- 获得当前对象上指定的注解  T getAnnotation(Class<T> annotationClass)
- 获得当前对象及其从父类对象上继承的所有注解    Annotation[] getAnnotations()
- 获得当前对象上所有的注解  Annotation[] getDeclaredAnnotations()

#### 元注解
用于修饰注解的注解(用于修饰自定义注解的JDK提供的注解)
- @Retention    用于确定被修饰的自定义注解的生命周期
    - RetentionPolicy.SOURCE 只存在于源码中,提供给编译器使用 
    - RetentionPolicy.CLASS 只能存在于源码和字节码中,用于JVM使用
    - RetentionPolicy.RUNTIME   被修饰的注解存在于源码,字节码,内存中;取代xml配置
- @Target   用于确定修饰的目标类型
    - ElementType.TYPE  修饰类,接口
    - ElementType.CONSTRUCTOR   修饰构造
    - ElementType.METHOD    修饰方法
    - ElementType.FIELD 修饰字段
- @Documented   使用javaDoc生成api文档,是否包含此注解
- Inherited 入过父类使用被修饰的注解,子类是否继承

## 2 类加载器
### 2.1 作用
负责将class加载到内存，形成Class对象。

### 2.2 获得方式

`类名.class.getClassLoader()`

### 2.3 机制
全盘负责委托机制
- 全盘负责:A类如果要使用B类(不存在),A类加载器必须负责加载B类
- 委托机制:A类加载器如果要加载资源B,必须询问父类加载是否加载,如果加载,将直接使用;否则自己加载.
- 保证一个class文件只会被加载一次,形成一个Class对象,因为:
    - 如果一个class文件,被两个类加载器加载,将是两个对象,并且不能强转

### 2.4 JDK类加载器
- 引导类加载器：rt.jar   (java.lang , java.util 等)
- 扩展类加载器：jre/lib/ext目录所有内容
- 应用类加载器：WEB-INF/lib  和  WEB-INF/classes

## 3 动态代理
### Proxy
#### Proxy.newProxyInstance(ClassLoader, Calss[] interface, InvocationHanlder );
- 参数1:`loader`,类加载器,动态代理类,运行时创建,任何类都需要类加载器将其加载到内存:
`当前类.class.getClassLoader()`
-参数2:Class[] interfaces 代理类需要实现的所有接口
    - 方式1:`目标类实例.getClass().getInterfaces()`,只能使用自己的接口,不能获得父类接口
    - 方式2:`new Class[]{userService.class}`,例如jdbc驱动-->DriverManager 获得接口Connetion
- 参数3:InvocationHandler 处理类,接口,必须进行实现类,代理类的每一个方法执行,都将调用一次
```
invoke(Object proxy, Method method, Object[] args)
```
    - 参数3-1: Object proxy 代理对像
    - 参数3-2: Method method 代理对像当前执行的方法的描述对象(反射),method.getName()/method.invoke()
    - 参数3-3: Object[] args 方法实际参数
