---
layout: '[layout]'
title: MySQL数据存在就更新，不存在就添加
date: 2018/6/21 16:04:21
categories: db
tags: [db，mysql]
---
最近新功能有个需求优化，避免库里重复出现多条同一个手机号记录，以手机号作为主键并创建唯一索引，操作是添加条目但是又可能存在此唯一项，所以想到存在即更新。
### ON DUPLICATE
`INSERT` 语句的一部分，如果指定 `ON DUPLICATE KEY UPDATE` ，并且插入行后会导致在一个UNIQUE索引或PRIMARY KEY中出现重复值，则在出现重复值的行执行UPDATE，如果不会导致唯一值列重复的问题，则插入新行。

### SQL语句原型：
`INSERT INTO table (player_id,award_type,num) VALUES (20001,0,1) ON  DUPLICATE KEY UPDATE num=num+values(num)`

### 实现思路：
  如果没有用户幸运值改变+1  ，新表中没有用户信息就添加
 1、判断用户信息是否存在 （不存在添加）
 2、存在修改用户幸运值个数
为了预防高并发下 两层sql出现问题

原SQL :  
```
 SELECT  num  FROM table WHERE  uid=20001 AND  award_type=0 LIMIT 1;
```
```
 UPDATE TABLE SET  num=num+1 where  uid=20001 AND  award_type=0；
```

#### 需求：

`uid`和`award_tyhpe`两个字段值 都相同时才更新  其中有一个不一样就添加新数据

执行语句
```
INSERT INTO table (player_id,award_type,num) VALUES (20001,0,1) ON  DUPLICATE KEY UPDATE num=num+values(num);
```

将player_id 、award_type  建立 联合unique 索引   
```
CREATE TABLE `willow_player` (
  `id` bigint(11) NOT NULL AUTO_INCREMENT,
  `player_id` bigint(16) NOT NULL DEFAULT '0',
  `num` int(11) NOT NULL DEFAULT '0',
  `award_type` tinyint(4) NOT NULL DEFAULT '0' COMMENT '类型',
  `repeat_num` smallint(11) NOT NULL DEFAULT '0' COMMENT '重复轮数',
  PRIMARY KEY (`id`),
  UNIQUE KEY `num` (`player_id`,`award_type`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=96 DEFAULT CHARSET=utf8mb4;
```
这样只有两个字段值内容全部一样时才会更新！！！   注意  id 会跳跃哦  mysql 机制原因


## REPLACE

讨人喜欢的 MySQL replace into 用法（insert into 的增强版）

在向表中插入数据的时候，经常遇到这样的情况：1. 首先判断数据是否存在； 2. 如果不存在，则插入；3.如果存在，则更新。

在 SQL Server 中可以这样处理：
```
   if not exists (select 1 from t where id = 1)
      insert into t(id, update_time) values(1, getdate())
   else
      update t set update_time = getdate() where id = 1
```
那么 MySQL 中如何实现这样的逻辑呢？别着急！mysql 中有更简单的方法： `replace into`
```
replace into t(id, update_time) values(1, now());
或
replace into t(id, update_time) select 1, now();
```
replace into 跟 insert 功能类似，不同点在于：replace into 首先尝试插入数据到表中， 1. 如果发现表中已经有此行数据（根据主键或者唯一索引判断）则先删除此行数据，然后插入新的数据。 2. 否则，直接插入新数据。

要注意的是：插入数据的表必须有主键或者是唯一索引！否则的话，replace into 会直接插入数据，这将导致表中出现重复的数据。

MySQL replace into 有三种形式：
1. `replace into tbl_name(col_name, ...) values(...)`
2. `replace into tbl_name(col_name, ...) select ...`
3. `replace into tbl_name set col_name=value, ...`
INSERT返回值为添加成功个数，REPLACE为删除与添加总数，也就是操作成功数量。
前两种形式用的多些。其中 “into” 关键字可以省略，不过最好加上 “into”，这样意思更加直观。另外，对于那些没有给予值的列，MySQL 将自动为这些列赋上默认值。

问题：这种方法没有达到真正的UPDATE，而是如他语法描述的REPLACE，而很多需求是数据完善，需要保留原有效字段，仍然有优化空间。
[转自]https://blog.csdn.net/woshihaiyong168/article/details/75082668

