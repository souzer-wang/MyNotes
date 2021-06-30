# RedisTemplate 常用方法总结

## 前情提要

1. **Redis**

一款开源的Key-Value数据库，运行在内存中，由C语言编写，通常被用来实现缓存。

2. **Jedis**

由 Redis 官方推出的面向 Java 的客户端

3. **Spring  Data Redis**

**Spring-data-redis** 是spring中的一部分，作用是在 Spring 应用中实现通过简单的配置访问 redis 服务



对 redis 底层开发包进行了高度封装，RedisTemplate 提供了 redis **各种操作、异常处理和序列化**

>  spring-data-redis 对 jedis 提供了如下功能：
>
> 1. 连接池自动管理，提供了一个高度封装的 “RedisTemplate” 类
> 2. 针对jedis客户端大量的 api 进行归类封装，将同一类型操作封装为operation接口，具体有：
>       1. ValueOperation：简单的 K-V 操作
>       2. SetOperation：set 类型数据操作
>       3. ZSetOperation：zset 类型数据操作
>       4. HashOperation：针对 map 类型的数据操作
>       5. ListOperation：针对list类型的数据操作
> 3. 提供了对 key 的bound便捷化操作 API，通过bound封装指定的key，对应上面有：
>       1. BoundValueOperation
>       2. BoundSetOperation
>       3. BoundListOperation
>       4. BoundZSetOperation
>       5. BoundHashOperation
> 4. 将事务操作封装化，有容器控制
> 5. 对于数据的 序列化和反序列化 提供了多种可选择策略(RedisSerializer)



Redis常用的数据类型有：

- String
- Hash
- List
- Set
- zSet
- Sorted set

**注：** 原文中的操作指令太多，这边只记录少量，其余可参考原文[RedisTemplate常用方法总结](https://blog.csdn.net/sinat_22797429/article/details/89196933)

## String 类型

### 概念

- string 是 redis 最基本的类型，一个 key 对应一个 value
- string 类型是二进制安全的，即， 它可以包含任何数据，如jpg图片，或序列化的对象
- string 类型是 Redis 最基本的数据类型，一个键最大存储 512MB



### 操作

```java
//判断是否有key所对应的值，有的话返回true，没有就false
redisTemplate.hasKey(key);

//有则取出key所对应的值
redisTemplate.opsForValue().get(key);

//删除单个key值
redisTemplate.delete(key);

//批量删除key
redisTemplate.delete(keys); //keys:Collection<K> keys

//将当前传入的key值序列化为byte[]类型
redisTemplate.dump(key);

//设置过期时间
public Boolean expire(String key, long timeout, TimeUnit unit) {
    return redisTemplate.expire(key, timeout, unit);
}

public Boolean expireAt(String key, Date date) {
    return redisTemplate.expireAt(key, date)
}

//查找匹配的key值，返回一个Set集合类型
public Set<String> getPatternKey(String pattern) {
    return redisTemplate.keys(pattern);
}

//修改redis中key的名称
public void renameKey(String oldKey, String newKey) {
    redisTemplate.rename(oldKey, newKey);
}

//返回传入key所存储的值的类型
public DataType getKeyType(String key) {
    return redisTemplate.type(key);
}

//如果旧值存在，将其改为新值
public Boolean renameOldKeyIfAbsent(String oldKey, String newKey) {
    return redisTemplate.renameIfAbsent(oldKey, newKey);
}

//从redis中随机取出一个值
redisTemplate.randomKey();

//得到当前key对应的剩余过期时间
public Long getExpire(String key) {
    return redisTemplate.getExpire(key);
}

//将key持久化保存
public Boolen persistKey(String key) {
    return redisTemplate.persist(key);
}

//将当前数据库的key移动到指定redis中数据库
public Boolean moveToDbIndex(String key, int dbIndex) {
    return redisTemplate.move(key, dbIndex);
}

//设置当前的key和value
redisTemplate.opsForValue().set(key, value);

//设置当前 key、value、过期时间
redisTemplate.opsForValue().set(key, value, timeout, unit);

//返回key中字符串的子字符
public String getCharacterRange(String key, long start, long end) {
    return redisTemplate.opsForValue().get(key, start, end);
}

//在原有的值基础上新增字符串到末尾
redisTemplate.opsForValue().append(key, value);

//获取字符串的长度
redisTemplate.opsForValue().size(key);
```

## Hash 类型

### 概念

- redis 的哈希值是字符串字段和字符串之间的映射
- 哈希中的字段数量没有限制

### 操作

```java
//获取变量中的指定map键是否有值,如果存在该map键则获取值，没有则返回null
redisTemplate.opsForHash().get(key, field);

//获取变量中的键值对
public Map<Object, Object> hGetAll(String key) {
    return redisTemplate.opsForHash().entries(key);
}

//新增hashMap值
redisTemplate.opsForHash().put(key, hashKey, value);

//以map集合的形式添加键值对
public void hPutAll(String key, Map<String, String> maps) {
    redisTemplate.opsForHash().putAll(key, maps);
}

//删除一个或者多个hash表字段
public Long hashDelete(String key, Object... fields) {
    return redisTemplate.opsForHash().delete(key, fields);
}

//查看hash表中指定字段是否存在
public boolean hashExists(String key, String field) {
    return redisTemplate.opsForHash().hasKey(key, field);
}

//获取所有hash表中字段
redisTemplate.opsForHash().keys(key);

//获取hash表中字段的数量
redisTemplate.opsForHash().size(key);

//获取hash表中存在的所有的值
public List<Object> hValues(String key) {
    return redisTemplate.opsForHash().values(key);
}
```



## List 类型

### 概念

- redis列表是简单的字符串列表，排序为插入顺序。列表的最大长度为2^32-1。
- redis的列表使用链表实现的，增加元素到头部或尾部的操作都在常量时间完成。

### 操作

```java
//通过索引获取列表中的元素
redisTemplate.opsForList().index(key, index);

//获取列表指定范围内的元素(end 为-1返回所有)
redisTemplate.opsForList().range(key, start, end);

//存到头部/尾部
redisTemplate.opsForList().leftPush(key, value);
redisTemplate.opsForList().rightPush(key, value)

//把多个值存入List中(value可以是多个值，也可以是一个Collection value)
redisTemplate.opsForList().leftPushAll(key, value);
redisTemplate.opsForList().rightPushAll(key, value);

//List存在的时候再加入
redisTemplate.opsForList().leftPushIfPresent(key, value);

//如果pivot处值存在则在pivot前面/后面添加
redisTemplate.opsForList().leftPush(key, pivot, value);
redisTemplate.opsForList().rightPush(key, pivot, value);

//设置指定索引处元素的值
redisTemplate.opsForList().set(key, index, value);

//移除并获取列表中第一个(最后一个略)元素(如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止)
redisTemplate.opsForList().leftPop(key);
redisTemplate.opsForList().leftPop(key, timeout, unit);

//从一个队列的右边弹出一个元素并将这个元素放入另一个指定队列的最左边
redisTemplate.opsForList().rightPopAndLeftPush(sourceKey, destinationKey);
Template.opsForList().rightPopAndLeftPush(sourceKey, destinationKey, timeout, unit);

//删除集合中值等于value的元素
//(index=0, 删除所有值等于value的元素; index>0, 从头部开始删除第一个值等于value的元素; index<0, 从尾部开始删除第一个值等于value的元素)
redisTemplate.opsForList().remove(key, index, value);

//将List列表进行剪裁
redisTemplate.opsForList().trim(key, start, end);

//获取当前key的List列表长度
redisTemplate.opsForList().size(key);
```

## Set 类型

### 概念

- redis集合是无序的字符串集合，集合中的值是唯一的，无序的。
- 可以对集合执行很多操作，如测试元素是否存在，对多个集合执行交集、并集和差集等等。

### 操作

```java
//添加元素
redisTemplate.opsForSet().add(key, values);

//移除元素(单个值、多个值)
redisTemplate.opsForSet().remove(key, values);

//删除并且返回一个随机的元素
redisTemplate.opsForSet().pop(key);

//获取集合大小
redisTemplate.opsForSet().size(key);

//判断集合是否包含value
redisTemplate.opsForSet().isMember(key, value);

//获取两个集合的交集(key对应的无序集合与otherKey对应的无序集合求交集)
redisTemplate.opsForSet().intersect(key, otherKey);

//随机获取集合中的一/多个元素
redisTemplate.opsForSet().randomMember(key);
redisTemplate.opsForSet().randomMembers(key, count);

//获取集合中的所有元素
redisTemplate.opsForSet().members(key);
```



## ZSet 类型

### 概念

- 有序集合由唯一的，不重复的字符串元素组成
- 有序集合中的元素是按序存储的，不是请求时才排序的
- 每个元素都关联了一个浮点值，称为分数（score）

### 操作

```java
//添加元素(有序集合是按照元素的score值由小到大进行排列)
redisTemplate.opsForZSet().add(key, value, score);

//删除对应的 value，value可以是多个值
redisTemplate.opsForZSet().remove(key, values);

//增加元素的score值，并返回增加后的值
redisTemplate.opsForZSet().incrementScore(key, value, delta);

//返回元素在集合的排名,有序集合是按照元素的score值由小到大/大到小排列
redisTemplate.opsForZSet().rank(key, value);
redisTemplate.opsForZSet().reverseRank(key, value);

//根据score范围获取集合元素数量
redisTemplate.opsForZSet().count(key, min, max);

//获取集合的大小
redisTemplate.opsForZSet().size(key)
redisTemplate.opsForZSet().zCard(key)
```

