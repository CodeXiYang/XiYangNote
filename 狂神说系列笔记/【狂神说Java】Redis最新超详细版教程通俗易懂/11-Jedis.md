# Jedis

jedis是Redis官方推荐的java连接redis开发工具,使用java操作redis中间件就需要使用jedis



## 1. 连接测试

1.1 创建maven工程,导入对应的依赖

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>3.2.0</version>
</dependency>
```



1.2 连接数据库

```java
//1. 创建jedis对象
Jedis jedis = new Jedis(TestPing.HOST, TestPing.PORT);
```

1.3 操作命令

```java
//3. jedis所有的方法就是redis中所有的命令
String ping = jedis.ping();
```

1.4 断开连接

```java
//5. 断开连接
jedis.close();
```

[连接测试代码](../redis代码/01-redis-jedis/src/main/java/com/xiyang/test/TestPing.java)

## 2. key的常用API

> jedis操作redis的5大基本数据类型代码

[key](../redis代码/01-redis-jedis/src/main/java/com/xiyang/base/JedisKey.java)

[String](../redis代码/01-redis-jedis/src/main/java/com/xiyang/base/JedisString.java)

[list](../redis代码/01-redis-jedis/src/main/java/com/xiyang/base/JedisList.java)

[Set](../redis代码/01-redis-jedis/src/main/java/com/xiyang/base/JedisSet.java)

[Hash](../redis代码/01-redis-jedis/src/main/java/com/xiyang/base/JedisHash.java)

Zset

## 3. 
