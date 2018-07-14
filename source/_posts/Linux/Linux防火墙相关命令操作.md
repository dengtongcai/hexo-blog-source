layout: '[layout]'
title: Linux防火墙相关命令操作
date: 2018/03/06 09:44:53  
categories: Linux
tags: [Linux]
---
## Linux防火墙相关命令操作
### 1.防火墙操作

- #### 编辑防火墙配置

  ```shell
  vi /etc/sysconfig/iptables
  ```

- #### 修改完保存退出，重启网卡服务

  ```shell
  service iptables restart
  ```

- #### 查看端口开放信息

  ```shell
  service iptables status
  ```

   

### 2.端口全网通

进入编辑页面，对8080端口开放全网通

```shell
-A INPUT -d 本机ip/32 -i eth1 -p tcp --dport 8080 -m state --state NEW -m comment --comment HTTPS接口全网通 -j ACCEPT
```

注意：上面配置需要放在本句后面`-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT`



### 3.添加指定IP白名单

```shell
-A INPUT -s 白名单ip/32 -d 本机ip/32 -i eth1 -p tcp -m state --state NEW -m comment --comment 注释语 -j ACCEPT
```



### 4.指定端口开放

```shell
-A INPUT -s 白名单ip/32 -d 本机ip/32 -i eth1 -p tcp --dport 开放端口 -m state --state NEW -m comment --comment 注释语 -j ACCEPT
```

