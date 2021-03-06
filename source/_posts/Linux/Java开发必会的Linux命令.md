---
layout: '[layout]'
title: Java开发必会的Linux命令
date: 2018/03/06 13:23:53  
categories: 后端
tags: [Linux]
---
本文并不会对所有命令进行详细讲解，只给出常见用法和解释。具体用法可以使用`--help`查看帮助或者直接通过google搜索学习。

### 1.查找文件

`find / -name filename.txt` 根据名称查找/目录下的filename.txt文件。

`find . -name "*.xml"` 递归查找所有的xml文件

`find . -name "*.xml" |xargs grep "hello world"` 递归查找所有文件内容中包含hello world的xml文件

`grep -H 'spring' *.xml` 查找所以有的包含spring的xml文件

`find ./ -size 0 | xargs rm -f &` 删除文件大小为零的文件

`ls -l | grep '.jar'` 查找当前目录中的所有jar文件

`grep 'test' d*` 显示所有以d开头的文件中包含test的行。

`grep 'test' aa bb cc` 显示在aa，bb，cc文件中匹配test的行。

`grep '[a-z]\{5\}' aa` 显示所有包含每个字符串至少有5个连续小写字符的字符串的行。

### 2.查看端口占用情况***

`netstat -tln | grep 8080` 查看端口8080的使用情况

### 3.查看端口属于哪个程序***

`lsof -i :8080`

### 4.查看文件，包含隐藏文件***

`ls -al`

### 5.以树状图列出目录的内容***

`tree a`

ps:[Mac下使用tree命令](https://link.juejin.im/?target=http%3A%2F%2Fwww.hollischuang.com%2Farchives%2F546)

### 6.复制文件***

`cp source dest` 复制文件

`cp -r sourceFolder targetFolder` 递归复制整个文件夹

`scp sourecFile romoteUserName@remoteIp:remoteAddr` 远程拷贝

### 7.创建目录

`mkdir newfolder`

### 8.删除目录

`rmdir deleteEmptyFolder` 删除空目录 `rm -rf deleteFile` 递归删除目录中所有内容

### 9.移动文件

`mv /temp/movefile /targetFolder`

### 10.重命名

`mv oldNameFile newNameFile`

### 11.切换用户

`su -username`

### 12.修改文件权限***

`chmod 777 file.java` //file.java的权限-rwxrwxrwx，r表示读、w表示写、x表示可执行

### 13.压缩文件***

`tar -czf test.tar.gz /test1 /test2`

### 14.列出压缩文件列表***

`tar -tzf test.tar.gz`

### 15.解压文件

`tar -xvzf test.tar.gz`

### 16.查看文件头10行***

`head -n 10 example.txt`

### 17.查看文件尾10行

`tail -n 10 example.txt`

### 18.查看日志类型文件

`tail -f exmaple.log` //这个命令会自动显示新增内容，屏幕只显示10行内容的（可设置）。

### 19.使用超级管理员身份执行命令

`sudo rm a.txt` 使用管理员身份删除文件

### 20.查看一个程序是否运行

`ps –ef|grep tomcat` 查看所有有关tomcat的进程

`ps -ef|grep --color java` 高亮要查询的关键字

### 21.终止线程

`kill -9 19979` 终止线程号位19979的进程

### 22.查看进程

`ps aux|grep java` 查看java进程

`ps aux` 查看所有进程

### 23.当前工作目录

`pwd`

### 24. 文件下载

`wget http://file.tgz` [mac下安装wget命令](https://link.juejin.im/?target=http%3A%2F%2Fwww.hollischuang.com%2Farchives%2F548)

`curl http://file.tgz`

### 25. 网络检测

`ping www.just-ping.com`

### 26.远程登录

`ssh userName@ip`

### 27.打印信息

`echo $JAVA_HOME` 打印java home环境变量的值

### 28.java 常用命令

java javac [jps](https://link.juejin.im/?target=http%3A%2F%2Fwww.hollischuang.com%2Farchives%2F105) ,[jstat](https://link.juejin.im/?target=http%3A%2F%2Fwww.hollischuang.com%2Farchives%2F481) ,[jmap](https://link.juejin.im/?target=http%3A%2F%2Fwww.hollischuang.com%2Farchives%2F303), [jstack](https://link.juejin.im/?target=http%3A%2F%2Fwww.hollischuang.com%2Farchives%2F110)

### 29.其他命令

svn git maven

###30.查看指定端口资源消耗

`top`

- -h | -v: 显示帮助或者版本信息】
- -c: 命令行列显示程序名以及参数
- -d: 启动时设置刷新时间间隔
- -H： 设置线程模式
- -i: 只显示活跃进程
- -n: 显示指定数量的进程
- -p: 显示指定PID的进程
- -u: 显示指定用户的进程

### linux命令学习网站:

[explainshell.com/](https://link.juejin.im/?target=http%3A%2F%2Fexplainshell.com%2F)



### 参考资料：

[Linux端口被占用的解决(Error: JBoss port is in use. Please check)](https://link.juejin.im/?target=http%3A%2F%2Fwww.hollischuang.com%2Farchives%2F239)

[linux 中强大且常用命令：find、grep](https://link.juejin.im/?target=https%3A%2F%2Flinux.cn%2Farticle-1672-1.html)

[Linux命令](https://link.juejin.im/?target=http%3A%2F%2Fblog.csdn.net%2Ftianshijianbing1989%2Farticle%2Fdetails%2F40780463)


转自链接：https://juejin.im/post/5ac04f206fb9a028d1413bf2

来源：掘金