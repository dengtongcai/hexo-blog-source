---
title: 记一次编写shell脚本的坑与疑问
date: 2019-01-05 17:09:26
categories: 笔记
tags: [shell,疑问,笔记]
---

对于shell脚本语言，熟悉的也就那几个常用的Linux命令比如字符串处理、自动化部署脚本，并没有做过相关的项目开发，但是让我做些简单的需求，有思路后结合搜索引擎也能处理，就是会耗费更长一点的时间。今天伟哥让我帮他弄一个日志处理的脚本，记录下其中遇到的的一些坑和疑问。产生这些疑问的主要原因还是会潜移默化的把java编程习惯代入到shell编程里面，晕😵！

### 需求

一个格式如下日志文件，需要统计每一行的` 竖线|`为分隔后的最后一个数字与200大小数量

```
00000000|111111|222222222|333333333|44444444444|555555|666666|21
00000000|111111|222222222|333333333|44444444444|555555|666666|254
00000000|111111|222222222|333333333|44444444444|555555|666666|200
00000000|111111|222222222|333333333|44444444444|555555|666666|763
00000000|111111|222222222|333333333|44444444444|555555|666666|23
00000000|111111|222222222|333333333|44444444444|555555|666666|6754
00000000|111111|222222222|333333333|44444444444|555555|666666|7685
00000000|111111|222222222|333333333|44444444444|555555|666666|36
00000000|111111|222222222|333333333|44444444444|555555|666666|223
00000000|111111|222222222|333333333|44444444444|555555|666666|856
00000000|111111|222222222|333333333|44444444444|555555|666666|200
00000000|111111|222222222|333333333|44444444444|555555|666666|77
```

### 代码

```shell
#!/bin/bash
#计数器
big=0
small=0
eq=0
#读取文件按行处理，$1为需要处理的文件
cat $1 | while read line
do
	string=$line
	echo 当前行数据$line
	#arr=(${string//|/})

	#按照`|`切成数组
	OLD_IFS="$IFS"
	IFS="|"
	arr=($line)
	IFS="$OLD_IFS"

	length=${#arr[@]}
	echo 当前数组长$length
	
	#获取最后一个元素
	thisnum=${arr[length-1]}
	if test $thisnum -eq 200
	then
		let eq++
		echo $thisnum等于200
	elif test $thisnum -gt 200
	then
		let big++
		echo $thisnum大于200
	else
		let small++
		echo $thisnum小于200
	fi
	echo
done
echo --------done--------
echo 大于200的数量：$big
echo 等于200的数量：$eq
echo 小于200的数量：$small
echo ------统计完成-------
```

### 坑和疑问

1. done后面打出来的结果未被while循环里面修改

   ```shell
   --------done---------
   大于200的数量：0
   等于200的数量：0
   小于200的数量：0
   ------统计完成-------
   ```

   此处是因为使用while循环，改成for循环就能正常修改`big/eq/small`的值

   ```shell
   --------done---------
   大于200的数量：9
   等于200的数量：1
   小于200的数量：2
   ------统计完成-------
   ```

   以下2019-01-06更新：

   感谢@[Mabbs](https://github.com/Mabbs)的回答，查了一下管道相关的资料。

   使用for line in `cat $1` 和`while read line`时操作在同一个进程内。当使用`cat $1 | while read line`时，`while read line`会fork一个子进程，无法共享父进程声明的变量。

   我们可以通过命名管道实现子进程变量存储到命名管道，结束后进程再读取的方式。

   ```shell
   #!/bin/bash
   mkfifo ppp
   big=0
   small=0
   eq=0
   
   cat $1 | while read line
   #for line in `cat $1`
   do
     string=$line
     echo 当前行数据$line
     #arr=(${string//|/})
   
     OLD_IFS="$IFS"
     IFS="|"
     arr=($line)
     IFS="$OLD_IFS"
   
     length=${#arr[@]}
     echo 当前数组长$length
   
     thisnum=${arr[length-1]}
     if test $thisnum -eq 200
       then
           let eq++
           echo $thisnum等于200
       elif test $thisnum -gt 200
       then
           let big++
           echo $thisnum大于200
      else
           let small++
           echo $thisnum小于200
     fi
   echo ---------------------
   echo $big $eq $small > ppp
   done
   read big eq small < ppp
   echo --------done---------
   echo 大于200的数量：$big
   echo 等于200的数量：$eq
   echo 小于200的数量：$small
   echo ------统计完成-------
   ```

   在实际使用中，如果没有子父管道的使用需求的话，注意不要使用相关管道符即可。

2. 切数组的时候使用另一种方式，效果是只把`|`去掉，数组长度为1

   ```shell
   #切割方式
   arr=(${string//|/})
   #结果
   000000001111112222222223333333334444444444455555566666621
   ```

   这个问题不知为何，难道使用方式不对？

   以下2019-01-06更新：

   果然是坑啊，少个空格！！！！😡😡😡😡😡😡

   然后切割后的数组必须重新取值后才可获得长度，演示情况

   ```shell
   #!/bin/bash
   while read line
   do
       string=$line
       str=${string//|/ }
       echo str 长度: ${#str[@]}
   	echo $str--${str[*]}
       
       echo ------赋值给 arr-----
       arr=($str)
       echo arr 长度：${#arr[@]}
       echo $arr--${arr[*]}
   done
   ```

   结果

   ```shell
   str 长度: 1
   99 2 3 4 5 6 7--99 2 3 4 5 6 7
   ------赋值给 arr-----
   arr 长度：7
   99--99 2 3 4 5 6 7
   ```

   解析

   ```shell
   #!/bin/bash
   while read line
   do
       string=$line				#99|2|3|4|5|6|7
       str=${string//|/ }			#"99 2 3 4 5 6 7",str字符串类型
       echo str 长度: ${#str[@]}
   	echo $str--${str[*]}		
       
       echo ------赋值给 arr-----
       arr=($str)					#(99 2 3 4 5 6 7)，arr数组类型
       echo arr 长度：${#arr[@]}
       echo $arr--${arr[*]}		#$arr默认是第一个元素
   done
   ```

   终于把shell这些疑问解决了！


### 最终代码

```shell
#!/bin/bash

#读取文件按行处理
#for line in `cat $1`
while read line
do
	string=$line
	echo 当前行数据$line
	#arr=(${string//|/})

	#按照`|`切成数组
	OLD_IFS="$IFS"
	IFS="|"
	arr=($line)
	IFS="$OLD_IFS"

	length=${#arr[@]}
	echo 当前数组长$length

	thisnum=${arr[length-1]}
	if test $thisnum -eq 200
	then
		let eq++
		echo $thisnum等于200
	elif test $thisnum -gt 200
	then
		let big++
		echo $thisnum大于200
	else
		let small++
		echo $thisnum小于200
	fi
	echo ---------------------
done
echo --------done---------
echo 大于200的数量：$big
echo 等于200的数量：$eq
echo 小于200的数量：$small
echo ------统计完成--------
```

使用方法

```shell
linux 管道: cat log.txt | ./log_calc.sh
bash 标准输入导入 ./log_calc.sh < log.txt
直接在 bash 中输入一条信息测试 ./log_calc.sh <<< '000|111|2222|333|444|555555|666666|6754'
```

结果

```shell
--------done---------
大于200的数量：9
等于200的数量：1
小于200的数量：2
------统计完成-------
```



### 资料

[bash中一次性给多个变量赋值–命名管道的使用](https://xingtingyang.com/558.html)

[善于使用 bash builtin -- read](http://blog.crhan.com/2013/01/%E5%96%84%E4%BA%8E%E4%BD%BF%E7%94%A8-bash-builtin-read/)



