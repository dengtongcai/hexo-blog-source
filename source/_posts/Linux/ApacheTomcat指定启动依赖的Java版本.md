---
layout: '[layout]'
title: ApacheTomcat指定启动依赖的Java版本
date: 2018/04/18 14:50:23 
categories: 后端
tags: [tomcat]
---
项目原来使用的jdk版本位1.7，导致机器上很多tomcat都是依赖jdk1.7版本。现在部分项目需要升级jdk1.8，则单独指定启动jdk路径

### 1. windows环境
   找到bin下的setclasspath.bat/catalina.bat文件，在文件的开始处添加如下代码：

   ```
   set JAVA_HOME=/app/appuser/yys/jdk1.8.0_161
   set JRE_HOME=/app/appuser/yys/jdk1.8.0_161/jre
   ```
   上面的意思是设定JAVA_HOME和JRE_HOME的路径；
   通过这里我们可以看出可以不设置JDK的环境变量；

### 2. linux环境
   找到bin下的setclasspath.sh/catalina.sh文件，在文件的开始处添加如下代码：

   ```
   export JAVA_HOME=/app/appuser/yys/jdk1.8.0_161
   export JRE_HOME=/app/appuser/yys/jdk1.8.0_161/jre
   ```

 