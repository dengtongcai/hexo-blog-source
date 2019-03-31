---
layout: '[layout]'
title: log4j同步机制导致的cpu飙升排查与解决
date: 2019/3/24 12:05:18  
categories: 后端
tags: [线上问题,log4j,jstack]
---
### 问题

组内某业务的几个相关接口均超时，上阿里云查日志一看是Dubbo调用超时，查看网络情况未发现异常，直接上Provider的机器查看占用：

```shell
# top
  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
 7020 root      20   0 2538892 164144  11856 S  90.3  8.7  61:23.54 java
11022 root      20   0 2560528 241340  11920 S  0.3 12.8 311:23.23 java
26805 root      20   0   32612   4036   2472 S  0.3  0.2  24:50.95 AliYunDunUpdate
26838 root      10 -10  134120  14524   5924 S  0.3  0.8 343:05.22 AliYunDun
    1 root      20   0   43280   3300   2108 S  0.0  0.2   2:16.82 systemd
    2 root      20   0       0      0      0 S  0.0  0.0   0:01.78 kthreadd
    3 root      20   0       0      0      0 S  0.0  0.0   1:30.68 ksoftirqd/0
    5 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 kworker/0:0H
    7 root      rt   0       0      0      0 S  0.0  0.0   0:00.00 migration/0
    8 root      20   0       0      0      0 S  0.0  0.0   0:00.00 rcu_bh                 
    9 root      20   0       0      0      0 S  0.0  0.0  65:15.85 rcu_sched             
   10 root      rt   0       0      0      0 S  0.0  0.0   2:14.65 watchdog/0             
   12 root      20   0       0      0      0 S  0.0  0.0   0:00.00 kdevtmpfs             
   13 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 netns                 
   14 root      20   0       0      0      0 S  0.0  0.0   0:00.00 khungtaskd             
   15 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 writeback             
   16 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 kintegrityd   
```

查询到7020这个进程有异常，在继续查看具体异常线程

```shell
# top -Hp 7020
 PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
 23328 root      20   0 2538892 164144  11856 S  90.0  8.7   0:00.00 java
```

找到了当前异常进程下的异常线程后使用jstack查看详细情况

```shell
# jsack -l 6377 > error.log
```

```java
# printf "%x\n" 23328
5b20
```

然后从jstack里查询该线程信息

```shell
# grep '18e9' error.log --color
"http-bio-6379-exec-200" #8869954 daemon prio=5 os_prio=0 tid=0x00007f74a81f6800 nid=0x5b20 waiting for monitor entry [0x00007f742457f000]
```

最后从jstack文件定位到堆栈信息

```shell
"http-bio-7020-exec-200" #8869954 daemon prio=5 os_prio=0 tid=0x00007f74a81f6800 nid=0x5b20 waiting for monitor entry [0x00007f742457f000]
   java.lang.Thread.State: BLOCKED (on object monitor)
	at org.apache.log4j.Category.callAppenders(Category.java:204)
	- waiting to lock <0x00000000800371d0> (a org.apache.log4j.spi.RootLogger)
	at org.apache.log4j.Category.forcedLog(Category.java:391)
	at org.apache.log4j.Category.log(Category.java:856)
	at org.slf4j.impl.Log4jLoggerAdapter.info(Log4jLoggerAdapter.java:368)
```

### 结论

在log4j 1.x版本中，logger.info等日志记录方法是同步的，大量的日志导致线程阻塞在callAppenders()这个方法

```java
public void callAppenders(LoggingEvent event) {
    int writes = 0;
    for(Category c = this; c != null; c=c.parent) {
      // Protected against simultaneous call to addAppender, removeAppender,...
      synchronized(c) {
	if(c.aai != null) {
	  writes += c.aai.appendLoopOnAppenders(event);
	}
	if(!c.additive) {
	  break;
	}
      }
    }
    if(writes == 0) {
      repository.emitNoAppenderWarning(this);
    }
}
```

临时解决办法先把日志打印级别调高至`error`，后续在考虑优化方案。

### 优化方案

1. 升级log4j 1.x至log4j 2.x版本，同时修改相应配置

   ```java
   <!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-core -->
   <dependency>
       <groupId>org.apache.logging.log4j</groupId>
       <artifactId>log4j-core</artifactId>
       <version>2.11.2</version>
   </dependency>
   ```

2. 使用log4j 桥接类

   ```java
   log4j-1.2-api-2.6.2
   ```

3. 如果是使用Spring Boot开发的话，就不会出现此问题，Spring Boot默认使用的时logback是不会有同步问题

参考：[log4j平稳升级到log4j2](https://www.cnblogs.com/hujunzheng/p/9937097.html)