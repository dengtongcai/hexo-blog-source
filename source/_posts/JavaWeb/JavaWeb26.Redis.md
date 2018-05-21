layout: '[layout]'
title: JavaWeb_27_Redis
date: 2014-02-03 20:18:43
categories: JavaWeb
tags: [笔记]
---
## 1 Redis基本操作
- 存放数据`set key value`
- 获得数据`get key`
- 删除数据`del key`

## 2 Jedis的使用
### 2.1 核心对象`Jedis`
- 单实例`new Jedis(host,port);`
- 连接池`JedisPool(jedisConfigPool,host,port);`
```
static{
	//加载配置文件
	InputStream in = JedisUtils.class.getClassLoder().getResourceAsStream("配置文件.properties");
	Properties prop = new Properties();
	prop.load(in);
	//获取配置文件参数
	String host = prop.getProperty("Jedis.host");
	int port = Integer.parseInt(prop.getProperty("Jedis.port"));
	int maxIdle = Integer.parseInt(prop.getProperty("Jedis.maxIdle"));
	int minIdle = Integer.parseInt(prop.getProperty("Jedis.minIdle"));
	//池配置
	JedisConfigPool jedisConfigPool = new JedisConfigPool();
	poolConfig.setMaxIdle(maxIdle); // 最大空闲
	poolConfig.setMinIdle(minIdle); // 最小空闲
	//获取连接
	JedisPool jedisPool = new JedisPool(jedisConfigPool,host,port);
}
//对面提供获得方式
public static Jedis getJedis(){
	return jedisPool.getResource();
}
//释放资源
public static void close(Jedis jedis){
	if(jedis!=null){
		jedis.close();
	}
}
```

## 3 数据结构

### 3.1 String
- 赋值`set key value`
- 获取值`get key`
- 获得后再设置`getset key value`
- 删除`del key`
- 数值加`incr key`
- 数值减`decr key`
- 扩展
    - 数值原子性增加`incrby key num`
    - 数值原子性减少`decrby key num`
    - 追加`append key value`

### 3.2 set
- 添加`sadd key [values...]`
- 删除`srem key members[m1,m2,...]`
- 获得所有`smembers key`
- 是否有`sismember key member`
- 差`sdiff key1 key2 ...`
- 交`sinter key1 key2 ...`
- 并`sunion key1 key2 ...`
- 扩展
    - 个数`scard key`
    - 随机获得`srandmember key`
    - 差存储在destination`sdiffstore destination key1 key2`
    - 交存储在destination`sinterstore destination key1 key2 ...`
    - 并存储在destination`sunionstore destination key1 key2 ...`

### 3.3 list
- 左+`lpush key values[...]`
- 右+`rpush key values[...]`
- 范围查询`lrange key start end`从0开始计数,-1表示链尾,-2表示倒数第二...
- 左弹`lpop key`
- 右弹`rpop key`
- 长度`llen key`
- 扩展
    - 左+没有集合忽略`lpushx key value`
    - 右+没有集合忽略`rpushx key value`
    - 删除`lrem key n个 value`
    - 指定索引`lset key index value`
    - 指定元素前后`linsert key before/after pivot value`
    - 尾部弹出添加到另一个头部`rpoplpush 表1 表2`

### 3.4 hash
- 添加一个`hset key field value`
- 添加一组`hset key [field value...]`
- 获得一个`hget key field`
- 获得一组`hget key fields`
- 获得所有`hgetall key`
- 删除指定`hdel key [field...]`
- 删除所有`del key`
- 数值加`hincrby key field num`
- 扩展
    - 是否存在`hexists key field`
    - 长度`hlen key`
    - 所有键`hkeys key`
    - 所有值`hvals key`

### 3.5 sorted-set
- 添加`zadd key 分数1 成员1 ... `
- 获得指定对象分数`zscore key member`
- 获得数量`zcard key`
- 删除`zrem key member1 ...`
- 查询范围索引`zrange key start end[带分数]`
- 查询所有 大到小`zrevrange key start end [带分数]`
- 排名删除`zremrangebyrank key start end`
- 分数删除`zremrangebyscore key min max`
- 扩展
    - 分数查询`zrangebyscore key min max[withscores] [limit offset count]`从offset角标开始
    - 加分数`zincrby key increment member`
    - 统计数量`zcount key min max`
    - 排名(小到大)`zrank key member`
    - 排名(大到小)`zrevrank key member`