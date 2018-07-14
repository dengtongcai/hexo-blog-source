layout: '[layout]'
title: Java服务程序CPU飙升排查，找出死循环代码
date: 2018/04/30 10:29:33  
categories: Linux，MySQL
tags: [MySQL]
---
## Java服务程序CPU飙升排查，找出死循环代码
### windows环境下CPU飙升问题
线上某台runtime机器（windows Server）cpu报警，这种情况初步就是代码里面死循环了，先把机器下线了保证不再有新的任务分配进来，然而cpu使用依然不降这是正常的因为程序未结束死循环一直在运行。



#### 1.找到Java进程对应的pid 

找pid的方法是:打开任务管理器，然后点击 “查看” 菜单，然后点击 “选择列”，把pid勾上，然后就可以在任务管理器里面看到所有进程的pid值了。(也可以用第三步中提到的工具直接查看)，windows10系统可以在详细信息直接查看 。![11](C:\Users\Administrator\Desktop\11.jpg)



#### 2.导出该Java进程快照

```
jstack -l pid > c:/31372.stack
```



#### 3.在windows下只能查看进程的cpu占用率，要查看线程的cpu占用率要借助其他的工具，可以使用微软提供的 [Process Explorer](https://docs.microsoft.com/zh-cn/sysinternals/downloads/process-explorer)（点击前往下载页面） 

#### 4.点击需要查看的进程右键properties![12](C:\Users\Administrator\Desktop\12.jpg)  



#### 5.选择Threads选项卡，找到占用CPU的线程id![13](C:\Users\Administrator\Desktop\13.jpg)



#### 6.把pid转换成16进制，我这里直接用系统自带的计算器转换，置于为什么要转换，是因为先前用jstack导出的信息里面线程对应的tid是16进制的。

  ![14](C:\Users\Administrator\Desktop\14.jpg)



#### 7.然后在导出的进程文件里面搜7C84即可



### Linux环境下CPU飙升问题

#### 1.使用如下命令找到最耗CPU的进程，然后再按一下 1，就会显示你服务器逻辑CPU的数量以及现在服务器CPU各个参数。占用最高的排在前面，我们可以看到占用高的ava进程 

```
top -c
```



 #### 2.找出占用高的Java线程，显示一个进程的线程运行信息列表，按一下P查看最高占用

```
top -Hp pid
```



#### 3.使用JVM自带的jstack命令导出当前所有线程的运行情况和线程当前状态，jstack工具可以用来获得core文件的java stack和native stack的信息，从而可以轻松地知道java程序是如何崩溃和在程序何处发生问题  

```
jstack pid > error.log
```



#### 4.目标线程pid转16进制

```
pringf "%x\n" pid
```



#### 5.查找error.log中pid16进制转化后的位置 

```
grep pif error.log --color
```

