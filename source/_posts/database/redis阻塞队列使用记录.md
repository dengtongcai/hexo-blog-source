---
layout: '[layout]'
title: redis阻塞队列使用记录
date: 2018-12-23 21:40:41
categories: 数据库
tags: redis，list，blockingQuene
---

### 日常使用的redis命令

连接redis，并登录

```shell
#redis version
./redis-server -v
Redis server v=4.0.9 sha=00000000:0 malloc=jemalloc-4.0.3 bits=64 build=f9e9200ea1e870b1
#connect redis server
./redis-cli -h 127.0.0.1 -p 6379
#auth
auth 123456
```

普通操作**SET key value [EX seconds][PX milliseconds] [NX|XX]**

将字符串值 `value` 关联到 `key` 。

如果 `key` 已经持有其他值， [SET](http://redisdoc.com/string/set.html#set) 就覆写旧值，无视类型。

对于某个原本带有生存时间（TTL）的键来说， 当 [SET](http://redisdoc.com/string/set.html#set) 命令成功在这个键上执行时， 这个键原有的 TTL 将被清除。

**可选参数**

从 Redis 2.6.12 版本开始， [SET](http://redisdoc.com/string/set.html#set) 命令的行为可以通过一系列参数来修改：

- `EX second` ：设置键的过期时间为 `second` 秒。 `SET key value EX second` 效果等同于 `SETEX key second value` 。
- `PX millisecond` ：设置键的过期时间为 `millisecond` 毫秒。 `SET key value PX millisecond` 效果等同于 `PSETEX key millisecond value` 。
- `NX` ：只在键不存在时，才对键进行设置操作。 `SET key value NX` 效果等同于 `SETNX key value` 。
- `XX` ：只在键已经存在时，才对键进行设置操作。

```shell
#set/get key value
127.0.0.1:6379> set key 1
OK
127.0.0.1:6379> get key
"1"

#set key value [EX seconds]
127.0.0.1:6379> set key-expire expire ex 10
OK
127.0.0.1:6379> ttl key-expire
(integer) 2
127.0.0.1:6379> get key-expire
"expire"
127.0.0.1:6379> get key-expire
(nil)

#set key value [EX seconds] [PX milliseconds] [NX|XX]
#SETNX set if not exists
127.0.0.1:6379> SET not-exists-key "value" NX
OK
127.0.0.1:6379> SET not-exists-key "value" NX
(nil)
127.0.0.1:6379> get not-exists-key
"value"
#SETXX set if exists
127.0.0.1:6379> SET exists-key "value" XX
(nil)
127.0.0.1:6379> SET exists-key "value"
OK
127.0.0.1:6379> SET exists-key "new-value" XX
OK
```

### list

普通使用`LPOP，LPUSH，LLEN`

```shell
#LPUSH key value [value ...]
127.0.0.1:6379> LPUSH list 'value1'
(integer) 1
127.0.0.1:6379> LPUSH list value2
(integer) 2
127.0.0.1:6379> LPUSH list value3
(integer) 3
127.0.0.1:6379> LLEN list
(integer) 3
127.0.0.1:6379> LPOP list
"value3"
127.0.0.1:6379> LPOP list
"value2"
127.0.0.1:6379> LPOP list
"value1"
```

文艺使用`BLPOP`

```shell
#BLPOP key [key ...] timeout 多个list，顺序拿到就结束，都没有则超时timeout后返回nil
127.0.0.1:6379> LPUSH list 'value1'
(integer) 1
127.0.0.1:6379> LPUSH list value2
(integer) 2
127.0.0.1:6379> LPUSH list value3
(integer) 3
127.0.0.1:6379> LPUSH list1 value3
(integer) 1
127.0.0.1:6379> BLPOP list list1 10
1) "list"	#key
2) "value3"	#value

#timeout 0
127.0.0.1:6379> BLPOP list list1 10
1) "list"
2) "value2"
127.0.0.1:6379> BLPOP list list1 10
1) "list"
2) "value1"
127.0.0.1:6379> BLPOP list list1 10
1) "list1"
2) "value3"
127.0.0.1:6379> BLPOP list list1 10
^[[A
(nil)
(10.09s)
127.0.0.1:6379> 
127.0.0.1:6379> BLPOP list list1 0
|	#waiting直到拿到值
```

### 曾经使用场景

>

- [Redis 命令参考](http://redisdoc.com/index.html#)