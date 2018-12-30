---
layout: '[layout]'
title: 键盘录入Scanner类中的next()与nextLine()的问题
date: 2013-06-01 21:32:10
categories: 代码
tags: [基础,代码]
---
本来之前看视频的时候讲过这个问题，结果今天做练习的时候没有注意，导致在for循环中键盘录入String和int数据的时候，第二轮开始Scanner中的方法并没有阻塞,导致录入完第一个整数时，控制台直接显示：请输入商品名称；输入就完成了。大脑没反应过来还以为是循环的问题，之后想想找到了问题。具体代码如下:
```
System.out.println("请输入商品名称");
String s1 = sc.nextLine();
System.out.println("请输入商品数量");
int count = sc.nextInt();
System.out.println("请输入商品名称");
String s2 = sc.nextLine();
```
第一个nextLine()正常录入，输入完int之后结果就录入完成了。
这是因为在Scanner类中： 
调用nextInt()方法时，我们键盘输入10，其实录入的是 10\r\n，所以第二个nextLine()方法读取到\r\n就结束了。

总结： 
next()：从控制台获取字符串，如果字符串中包含空格，只会获取第一个作为接收的字符串。比如：输入hello I am a chinese! 接收到的只是hello 
nextLine()：从控制台获取字符串，字符串中可以包含空格，以回车符作为接收结束标志。比如：输入hello I am a chinese! 接收到的是hello I am a chinese!
所以在循环录入时候，最好还是使用next()方法，如果需要输入空格隔开的的字符串，可以通过指定结束标记来实现和nextLine()一样录入一整行的效果：
```
String s;
while(!sc.hasNext("quit"))
{
   s = sc.next();
   System.out.println(s);
}
```