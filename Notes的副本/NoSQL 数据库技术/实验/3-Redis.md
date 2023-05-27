# MongoDB 数据库技术实验报告







<center>
  姓名：王磊
</center>



<center>
  学号：2020211538
</center>



<center>
  提交日期：2023-4-21
</center>





## Contents

[toc]





## 实验环境

平台：MacOS ventura

数据库版本：redis-latest

其它工具：Docker v4.19, RedisInsight-v2, VS Code



## 实验内容

连接到数据库

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504210331932.png" alt="image-20230504210331932" style="zoom: 33%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504210353704.png" alt="image-20230504210353704" style="zoom:33%;" />

### K 键值操作

1. 新增字符串类型键 mykey

```
set mykey "hello world"
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504205456744.png" alt="image-20230504205456744" style="zoom: 33%;" />

2. 查询键值

```
get mykey
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504205535295.png" alt="image-20230504205535295" style="zoom: 33%;" />

3. 删除键

```
del mykey
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504205602013.png" alt="image-20230504205602013" style="zoom: 33%;" />

4. 设置有效期为120s

```
expire mykey 120
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504205843690.png" alt="image-20230504205843690" style="zoom:33%;" />

查询剩余生存时间

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504205905896.png" alt="image-20230504205905896" style="zoom:33%;" />

5. 查看键类型

```
type mykey
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504205946775.png" alt="image-20230504205946775" style="zoom:33%;" />

6. 取消有效期

```
persist mykey
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504205927481.png" alt="image-20230504205927481" style="zoom:33%;" />

7.  判断是否存在

```
exists mykey
```

 结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504210104042.png" alt="image-20230504210104042" style="zoom:33%;" />

### L 列表

列表场景：麦当劳排队列表

1. 新增 10 个元素

```redis
LPUSH mcdonalds_queue "Customer 10"
LPUSH mcdonalds_queue "Customer 9"
LPUSH mcdonalds_queue "Customer 8"
LPUSH mcdonalds_queue "Customer 7"
LPUSH mcdonalds_queue "Customer 6"
LPUSH mcdonalds_queue "Customer 5"
LPUSH mcdonalds_queue "Customer 4"
LPUSH mcdonalds_queue "Customer 3"
LPUSH mcdonalds_queue "Customer 2"
LPUSH mcdonalds_queue "Customer 1"
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504212210726.png" alt="image-20230504212210726" style="zoom:33%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504212244887.png" alt="image-20230504212244887" style="zoom:33%;" />

2. 出队三个元素

```
LPOP mcdonalds_queue
LPOP mcdonalds_queue
LPOP mcdonalds_queue
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504212402857.png" alt="image-20230504212402857" style="zoom:33%;" />

3. 获取当前元素个数

```
LLEN mcdonalds_queue
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504212455039.png" alt="image-20230504212455039" style="zoom:33%;" />

4. 显示头部元素

```
LINDEX mcdonalds_queue 0
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504212526832.png" alt="image-20230504212526832" style="zoom:33%;" />

5. 截取最后入队的六个元素

```
LTRIM mcdonalds_queue -6 -1
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504212619219.png" alt="image-20230504212619219" style="zoom:33%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504212636627.png" alt="image-20230504212636627" style="zoom:33%;" />

### S 集合



1. 添加元素

```
SADD set1 "A" "B" "C" "D" "E" "F"
SADD set2 "D" "E" "F" "G" "H" "I"
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504213137451.png" alt="image-20230504213137451" style="zoom:33%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504213202176.png" alt="image-20230504213202176" style="zoom:33%;" />

2. 差集运算

```
SDIFFSTORE set3 set1 set2
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504213218805.png" alt="image-20230504213218805" style="zoom:33%;" />

3. 返回集合 3

```
SMEMBERS set3
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504213233853.png" alt="image-20230504213233853" style="zoom:33%;" />

4. 返回集合 1 中的 2 个元素

```
SRANDMEMBER set1 2
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504213248393.png" alt="image-20230504213248393" style="zoom:33%;" />

5. 随机删除 1 中 2 个元素

```
srem set1 B D
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504213307735.png" alt="image-20230504213307735" style="zoom:33%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504213320309.png" alt="image-20230504213320309" style="zoom:33%;" />

6. 查询集合 1

```
SMEMBERS set
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230504213342241.png" alt="image-20230504213342241" style="zoom:33%;" />



### ZS 有序集合

使用场景：热搜榜单

1. 新建集合

```redis
ZADD weibo_hot_search_1 100 "Hot Search 1"
ZADD weibo_hot_search_1 90 "Hot Search 2"
ZADD weibo_hot_search_1 80 "Hot Search 3"
ZADD weibo_hot_search_1 70 "Hot Search 4"
ZADD weibo_hot_search_1 60 "Hot Search 5"
ZADD weibo_hot_search_1 50 "Hot Search 6"
ZADD weibo_hot_search_1 40 "Hot Search 7"
ZADD weibo_hot_search_1 30 "Hot Search 8"

ZADD weibo_hot_search_2 80 "Hot Search 1"
ZADD weibo_hot_search_2 70 "Hot Search 2"
ZADD weibo_hot_search_2 60 "Hot Search 3"
ZADD weibo_hot_search_2 50 "Hot Search 4"
ZADD weibo_hot_search_2 40 "Hot Search 9"
ZADD weibo_hot_search_2 30 "Hot Search 10"
ZADD weibo_hot_search_2 20 "Hot Search 11"
ZADD weibo_hot_search_2 10 "Hot Search 12"
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507093256999.png" alt="image-20230507093256999" style="zoom:25%;" />

2. 计算交集

```
ZINTERSTORE weibo_hot_search_3 2 weibo_hot_search_1 weibo_hot_search_2
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507093411270.png" alt="image-20230507093411270" style="zoom:33%;" />

3. 移除有序集合 1 最高分、最低分元素

```
# 获取最高分的元素
ZREVRANGE weibo_hot_search_1 0 0
# 获取最低分的元素
ZRANGE weibo_hot_search_1 0 0

ZREM weibo_hot_search_1 "Hot Search 8" "Hot Search 1"
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507093541755.png" alt="image-20230507093541755" style="zoom:33%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507093631909.png" alt="image-20230507093631909" style="zoom:33%;" />

4. 返回一定区间内五个元素

```
ZRANGEBYSCORE weibo_hot_search_2 10 60 WITHSCORES LIMIT 0 4
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507094758702.png" alt="image-20230507094758702" style="zoom:33%;" />

5. 查询集合 3

```
ZRANGE weibo_hot_search_3 0 -1 WITHSCORES
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507095038486.png" alt="image-20230507095038486" style="zoom:33%;" />

6. 修改有序集合 1 中某个元素的 score 值

*将热搜 2 的分数增加 20*

```
ZINCRBY weibo_hot_search_1 20 "Hot Search 2"
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507095211891.png" alt="image-20230507095211891" style="zoom:33%;" />

### SP 发布/订阅

发布、订阅

1. 客户端 1和客户端 2 订阅频道 1

```
SUBSCRIBE channel1
```

 结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507095902676.png" alt="image-20230507095902676" style="zoom:33%;" />

3. 客户端 3 发布频道 1 消息，返回结果表示接收消息数

```
publish channel1 "Hello guys"
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507095955853.png" alt="image-20230507095955853" style="zoom:33%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507100015546.png" alt="image-20230507100015546" style="zoom:33%;" />

4. 查看频道 1 的订阅数

```
PUBSUB NUMSUB channel1
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507100121779.png" alt="image-20230507100121779" style="zoom:33%;" />

### G 地理位置

 GEOADD 键名称 经度 纬度 成员名称 [经度 纬度 成员名称……

1. 新增三个地理位置

```
GEOADD newgeo 116.41338 39.91092 bupt 114.11279 30.52181  Myhome 121.53126 38.88811 dl
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507102504103.png" alt="image-20230507102504103" style="zoom:33%;" />

2. 计算两个元素的距离

```
GEODIST newgeo bupt Myhome km
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507102721118.png" alt="image-20230507102721118" style="zoom:33%;" />

3. 以某元素为中心，返回一定半径内元素

```
GEOSEARCH newgeo FROMMEMBER Myhome BYRADIUS 1100 km
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507103030586.png" alt="image-20230507103030586" style="zoom:33%;" />

4. 删除某个元素

```
ZREM newgeo bupt
```

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507103157157.png" alt="image-20230507103157157" style="zoom:33%;" />



### BE1 命令的批量指令

```
multi

set mykey "hello world"

LPUSH mcdonalds_queue "Customer 10"

SADD set1 "A" "B" "C" "D" "E" "F"

ZADD weibo_hot_search_1 100 "Hot Search 1"

exec
```

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507103516313.png" alt="image-20230507103516313" style="zoom:33%;" />

### BS,RS 数据备份与恢复



```
save

FLUSHALL
```



<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507103841491.png" alt="image-20230507103841491" style="zoom:50%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507104038201.png" alt="image-20230507104038201" style="zoom:33%;" />



查看数据库中的键

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507103812031.png" alt="image-20230507103812031" style="zoom:33%;" />

清空数据库

![image-20230507104140587](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507104140587.png)

已经完全清空

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507104208876.png" alt="image-20230507104208876" style="zoom:33%;" />

在配置文件中添加目录

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507104627312.png" alt="image-20230507104627312" style="zoom:33%;" />

重启 redis

数据已恢复

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507110907738.png" alt="image-20230507110907738" style="zoom: 33%;" />



### PH1-PH7 基于 Python 访问 Redis

![image-20230507113846107](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507113846107.png)

1. 导入 redis 包

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507143810564.png" alt="image-20230507143810564" style="zoom:33%;" />

2. 连接数据库

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507143832240.png" alt="image-20230507143832240" style="zoom:33%;" />

3. 新增新闻信息

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507145145737.png" alt="image-20230507145145737" style="zoom:33%;" />

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507145205007.png" alt="image-20230507145205007" style="zoom:33%;" />

4. 查找字段

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507145219279.png" alt="image-20230507145219279" style="zoom:33%;" />

5. 新增关注者字段

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507145331647.png" alt="image-20230507145331647" style="zoom:33%;" />

结果：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507145403941.png" alt="image-20230507145403941" style="zoom:33%;" />

6. 删除关注者字段

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507145447294.png" alt="image-20230507145447294" style="zoom:33%;" />

7. 删除该条新闻键

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230507145607909.png" alt="image-20230507145607909" style="zoom:33%;" />