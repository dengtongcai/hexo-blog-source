layout: '[layout]'
title: CentOS7下安装配置MySQL
date: 2018/04/12 14:03:33  
categories: Linux，MySQL
tags: [MySQL]
---
### 1.准备资源

- 下载MySQL源安装包


```
wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
```

- 安装MySQL

```
yum localinstall mysql57-community-release-el7-8.noarch.rpm
```

- 检查MySQL是否安装成功

```
yum repolist enabled | grep "mysql.*-community.*"
```

安装成功如图![1](1.png)

可以修改`vim /etc/yum.repos.d/mysql-community.repo`源，改变默认安装的mysql版本。比如要安装5.6版本，将5.7源的`enabled=1`改成`enabled=0`。然后再将5.6源的`enabled=0`改成`enabled=1`即可。

### 2.安装MySQL服务 

```
yum install mysql-community-server
```

 ### 3.启动MySQL服务

```
yum install mysql-community-server
```

 ### 4.查看服务运行状态

```
systemctl status mysqld
```

![2](/2.png)

 ### 5.配置服务开机自启

```
systemctl enable mysqld
systemctl daemon-reload
```

 

### 6.配置新密码

- 先使用默认初始密码登录，然后修改密码

```
grep 'temporary password' /var/log/mysqld.log
```

![3](/3.png)

- 登录MySQL

```
mysql -uroot -p
```

- 修改新密码

```
mysql> use mysql;
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
```

### 7.添加远程登录

```
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;
```

 注意：必须修改密码才能使用远程登录



## 扩展

### 1.密码策略

注意：mysql5.7默认安装了密码安全检查插件（validate_password），默认密码检查策略要求密码必须包含：大小写字母、数字和特殊符号，并且长度不能少于8位。否则会提示ERROR 1819 (HY000): Your password does not satisfy the current policy requirements错误，如下图所示： 

![4](/4.png)

通过MySQL环境变量可以查看密码策略的相关信息

```
show variables like '%password%';
```

![5](/5.png)

- validate_password_policy：密码策略，默认为MEDIUM策略 
- validate_password_dictionary_file：密码策略文件，策略为STRONG才需要 
- validate_password_length：密码最少长度 
- validate_password_mixed_case_count：大小写字符长度，至少1个 
- validate_password_number_count ：数字至少1个 
- validate_password_special_char_count：特殊字符至少1个 

*上述参数是默认策略MEDIUM的密码检查规则。*

| 策略        | 检查规则                                                     |
| :---------- | ------------------------------------------------------------ |
| 0 or LOW    | Length                                                       |
| 1 or MEDIUM | Length; numeric, lowercase/uppercase, and special characters |
| 2 or STRONG | Length; numeric, lowercase/uppercase, and special characters; dictionary file |

 MySQL官网密码策略详细说明：http://dev.mysql.com/doc/refman/5.7/en/validate-password-options-variables.html#sysvar_validate_password_policy 

 ### 2.修改密码策略 

在/etc/my.cnf文件添加validate_password_policy配置，指定密码策略 # 选择0（LOW），1（MEDIUM），2（STRONG）其中一种，选择2需要提供密码字典文件 

```
validate_password_policy=0
```

 如果不需要密码策略，在my.cnf文件添加如下配置

```
validate_password = off
```

 重启MySQL服务使配置生效

```
systemctl restart mysqld
```

默认配置文件路径：

- 配置文件：/etc/my.cnf 
- 日志文件：/var/log//var/log/mysqld.log 
- 服务启动脚本：/usr/lib/systemd/system/mysqld.service 
- socket文件：/var/run/mysqld/mysqld.pid

 