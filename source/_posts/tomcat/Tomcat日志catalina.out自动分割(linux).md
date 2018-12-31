---
layout: '[layout]'
title: Tomcat日志catalina.out自动分割(linux)
date: 2018/5/21 17:06:53  
categories: 后端
tags: [Linux,cronolog,nginx,tomcat]
---
Tomcat服务器长时间运行，catalina.out文件越来越大，会导致服务器io性能降低。虽然tomcat有默认的日志分割功能，能每天自动生成类似catalina.2018-10-08.log的文件，但是默认catalina.out文件却一直增长，到一定大小时很占磁盘空间，影响性能，且会报错。

解决方法，使用cronolog进行日志切割,据说cronolog是个切割日志的小工具,可以切割很多种日志文件,有空得试试.具体做法如下: 
## 安装cronolog
1. 下载（最新版本） 
```
wget http://cronolog.org/download/cronolog-1.6.2.tar.gz
```
2. 解压缩 
```
tar zxvf cronolog-1.6.2.tar.gz
```
3. 进入cronolog安装文件所在目录 
```
cd cronolog-1.6.2
```
4. 运行安装 (如没有安装gcc,则先安装gcc--[`yun install gcc`])
```
./configure
```
```
make
```
```
make install
```
5. 查看cronolog安装后所在目录（验证安装是否成功） 
```
which cronolog
```
一般情况下显示为：/usr/local/sbin/cronolog 
## 使用cronlog切割tomcat/logs下面的catalina.out
1. 新建管道
```
cd /{tomcat_path}/logs/  
mkfifo  catalina.fifo.out（新建管道）
```
2. 配置cronolog
```
cd ../bin  (切换到tomcat的bin目录下）
vi startup.sh  （修改配置文件）
在下面代码前面加入
PRGDIR=`dirname "$PRG"`
EXECUTABLE=catalina.sh
加入
/usr/local/sbin/cronolog  /{tomcat_path}/logs/catalina.fifo.out.%Y-%m-%d < /{tomcat_path}/logs/catalina.fifo.out  &

（/usr/local/sbin/cronolog 为cronolog的默认安装目录（如果指定其他目录这里需要修改）  /usr/local/tomcat/logs/catalina.fifo.out.%Y-%m-%d对应的是tomcat的logs下 ） 
```
3. 指定管道
```
vi catalina.sh
将以下代码 
if [ -z "$CATALINA_OUT" ] ; then
  CATALINA_OUT="$CATALINA_BASE"/logs/catalina.out
fi

修改为 
if [ -z "$CATALINA_OUT" ] ; then
  CATALINA_OUT="$CATALINA_BASE"/logs/catalina.fifo.out
fi
```
4. 修改完配置文件以后建议重启centos 执行reboot命令（非必须）
5. 启动tomcat  在浏览器访问tomcat http://xxx.xxx.xxx:8080
6. 然后到tomcat 的安装目录的logs文件夹下查看有 catalina.fifo.out.2018-05-19这种格式的文件，就成功了 

## 使用cronlog切割ngin/logs下面的access.log
1. 新建管道
```
cd /usr/local/nginx/logs （进入nginx的安装目录下logs目录）
mkfifo access.fifo.log  （新建管道，会创建一个 access.fifo.log 文件）
cd /usr/local/nginx/conf  （进入nginx安装目录的conf文件夹下）
```
2. 配置使用管道
```
vi nginx.conf （修改配置文件）
 server{
        listen       80;
        server_name  tomcat.com;
        #charset koi8-r;

        access_log  logs/access.fifo.log;   （添加此行代码）

        location / {
             proxy_pass http://xxx.xxx.xxx.xxx:8080;
             root   html;
             index  index.html index.htm;
    }
```

3. 配置cronolog
```
reboot (重启centos)
cd /usr/local/nginx/logs
/usr/local/sbin/cronolog /usr/local/nginx/logs/access.fifo.log.%Y-%m-%d < /usr/local/nginx/logs/access.fifo.log &
/use/local/nginx/sbin/nginx       重新启动nginx
```
4. 在浏览器进入nginx首页    http://xxx.xxx.xxx.xxx
5. 然后到nginx的安装目录的logs文件夹下查看有 access.log.2018-05-19文件，就成功了

## SpringBoot日志切割
注：SpringBoot使用logback插件配置日志切割后会按配置生成日志文件并且catalina.out不会累积增长，logback.xml例子
```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>

    <!-- 记录一般信息的的appender -->
    <appender name="B"
              class="ch.qos.logback.core.rolling.RollingFileAppender">

        <File>logs/wechat-service.log</File>
        <Append>true</Append>
        <encoder>
            <pattern>%d %5p [%t] \(%F:%line\) - %msg%n</pattern>
            <charset>UTF-8</charset>
        </encoder>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- 日志输出到当前盘符的logs文件夹下 -->
            <fileNamePattern>logs/wechat-service.%d{yyyy-MM-dd}.log
            </fileNamePattern>
            <maxHistory>10</maxHistory>
        </rollingPolicy>
    </appender>

    <!-- 输出到控制台的appender -->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d %5p [%t] \(%F:%line\) - %msg%n</pattern>
        </encoder>
    </appender>

    <!-- logback为java中的包 -->
    <logger name="logback"/>

    <root level="INFO">
        <appender-ref ref="B"/>
        <appender-ref ref="STDOUT"/>
    </root>

</configuration>
```