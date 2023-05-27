##  contents

[toc]

# Redis

![shadow-image-20230419121414888](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230419121414888.png)

## 基础

### 操作命令分类

1. Geospatial 地理空间类
2. HyperLogLog 基数统计
3. Connection 用于管理客户端链接
4. Transactions 事务
   - Redis 的所有操作都是原子的
   - 然而没有在事务上增加任何维持原子性的机制
5. Server 管理数据库服务
6. Scripting 执行缓存中的 Lua 脚本
   - Lua 脚本语言，嵌入应用程序
   - 弥补事务管理机制的不足



## 键值管理操作

### Key 操作

![shadow-image-20230424171158269](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424171158269.png)

添加一个键

```redis
set pnum 200
```

### 字符串操作

![shadow-image-20230424171442424](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424171442424.png)

![shadow-image-20230424171503508](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424171503508.png)

```
# 赋值
set ticketNum 1000
# 取值
get ticketNum
# 自增
incr ticketNum
# 减少 10
decrby ticketNum 10
# 删除键
del ticketNum
```

### 列表

按插入顺序排序的字符串双向链表

![shadow-image-20230424172045035](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424172045035.png)

![shadow-image-20230424172057469](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424172057469.png)

```
# 向 userlist 左边插入 8 个元素
lpush userlist u100 u101 u102 ...
# 从右边弹出一个元素
rpop userlist
# 返回列表元素个数
llen userlist
# 按索引返回列表元素值
lindex userlist 6
# 返回多个元素
lrange userlist 1 3
```

列表元素删除

- count 大于 0，从左往右删除 count 个value 对应元素
- count 小于 0，从右往左删除 count 个value 对应元素
- count 等于 0，删除所有 value 对应元素

```
# 列表元素删除
lrem mylist count value
```

### 集合

没有排序、不允许重复的字符串

![shadow-image-20230424172907388](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424172907388.png)

![shadow-image-20230424172919177](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424172919177.png)

```
# 添加元素
sadd myfans f1 f2 f3
# 删除指定元素
srem myfans f6
# 查询集合所有元素
smembers myfans
# 集合操作
sunion/sdiff/sinter yourfans myfans
```



### Hash

具有 String Key 和 String Value 的 map 容器

![image-20230424173758049](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424173758049.png)

![image-20230424173804930](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424173804930.png)

```
# 查询 Hash 结构键 order 所有 key
hkeys order
# 查询所有 value
hvals order
# 判断指定 key 的 field 是否存在
hexsists order count
# 返回元素个数
hlen order
# 给指定元素一个增量
hincrby order amount 3
# 删除某个 field 元素
hdel order address
```



### Zset 有序集合

![image-20230424174332791](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424174332791.png)

![image-20230424174340614](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424174340614.png)

```
# 添加元素
zadd pranklist 100 p1 20 p2 33 p3
# 查询元素个数
zcard pranklist
# 查询某个元素分数
zscore pranklist p2
# 查询索引区间元素
zrange pranklist 0 2
# 查询元素，逆序返回
zrevrange pranklist 0 2
# 删除指定元素
zrem pranklist p1
# 根据分数范围删除元素
zremrangebyscore pranklist 10 30
```

### 发布与订阅

```
# 订阅频道
SUBSCRIBE PSTest
# 重开一个客户端，模拟消息发布者
PUBLIST PSTest "Hello!"
publish PSTest "Have a good day"
```



## 集群架构



## 管理和监控



