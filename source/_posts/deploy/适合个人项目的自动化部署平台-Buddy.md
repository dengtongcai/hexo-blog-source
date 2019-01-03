---
title: 适合个人项目的持续集成服务-Buddy
date: 2019-01-03 21:10:00
categories: 教程
tags: [buddy,自动化部署,适合个人]
---

前几天把博客通过travis-ci自动化部署之后，配置`Build pushed branches`，每次改完push到git就不用管的感觉和之前每次手动部署相比的确是很酸爽！最近在v2ex上看到有人推荐Buddy也有持续集成功能，刚好我给大学同学做的一个简单的SpringBoot应用还有一段时间的改动期，考虑到每次手动部署确实是一个浪费时间的重复性工作，就尝试用buddy来完成部署的工作，开箱体验后感觉就一个词--简单！和老牌持续集成服务jenkins和最近很火的travis-ci相比，buddy用起来很快很轻便，只要使用过jenkins几乎不需要做很大的改动甚至更简单的就能平移到buddy持续集成服务上。

下面就是我用buddy的开箱过程。

#### 注册并绑定github账号

打开[Buddy官网](https://app.buddy.works/login)注册账号后绑定github，建议直接使用github账号登录，然后请求授予Buddy应用github权限，选好付费类型后，然后你就能在buddy个人空间看到你所有的github项目并进行配置了。

![/1.png]()



#### 配置pipeline

1. 选择好我们要集成的项目后，buddy会自动识别项目类型

   ![/2.png]()

2. 配置pipeline基本信息，包括：名称，触发模式（手动-Manual，推送代码时-On push，定时-Recurrently），需要触发pipeline的分支或者tag

   ![/3.png]()

3. 配置触发pipeline后的操作（Actions）

   ![/4.png]()

   - 从github仓库更新代码

   - 执行maven clean install

     ```shell
     mvn clean install
     ```

   - 使用ssh远程连接(使用密码或者SSH key)部署服务器，停止应用，备份jar包

     ```shell
     # kill pid
     echo '杀死应用：quick-report-0.0.1-SNAPSHOT.jar'
     pid=$(ps -ef | grep quick-report-0.0.1-SNAPSHOT.jar | grep -v grep|tail|more|less| awk '{print $2}')
     echo ----kill process ------
     echo $pid
     echo -----------------------
     if [-n $pid]
     then
     	kill -9 $pid
     else
     	echo quick-report-0.0.1-SNAPSHOT.jar already stoped
     fi
     # print pid
     echo '查看进程是否存在'
     ps -ef | grep quick-report-0.0.1-SNAPSHOT.jar | grep -v grep|tail|more|less| awk '{print $2}'
     # remove jar
     echo '备份：quick-report-0.0.1-SNAPSHOT.jar'
     file_time=$(date +%Y%m%d%k%M%S)
     mv quick-report-0.0.1-SNAPSHOT.jar ./bak/quick-report-0.0.1-SNAPSHOT.jar_$file_time
     ```

   - 上传打包好的jar

   - 启动应用

     ```shell
     export JAVA_HOME=/root/java/jdk1.8.0_161
     export JRE_HOME=/root/java/jdk1.8.0_161/jre
     # statr quick-report-0.0.1-SNAPSHOT.jar
     echo 'start quick-report-0.0.1-SNAPSHOT.jar'
     sh startup.sh
     ps -ef | grep quick-report-0.0.1-SNAPSHOT.jar | grep -v grep|tail|more|less| awk '{print $2}'
     ```

4. 然后我们就可以点击`Run pipeline`来执行部署了，每一步执行过程都有log可供查看，如果执行失败可以重新执行当前action。
