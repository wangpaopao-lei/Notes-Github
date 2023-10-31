##  contents

[toc]

# MongoDB

![shadow-image-20230402124253426](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230402124253426.png)

## 概述



## 文档操作基础

### CRUD 操作

#### 新增文档

```JSON
db.ct.insertOne({"Name":"Wang","Age":28})
```

插入多个文档

```JSON
db.ct.insertMany([{}])
```

#### 文档查询

- 第一个花括号设定满足的条件
- 第二个花括号设定返回的字段

```
db.ct.find({},{})
```

结果按年龄升序排列，-1 表示降序

```
db.ct.find().sort({age:1})
```

跳过前三个文档，显示后续 top2 文档

```
db.ct.find().limit(2).skip(3)
```

in 判断条件，还有 nin

```
db.ct.find({age:{$in:[23,26,32]}})
```

or 运算

```
db.ct.find({$or:[{age:22},{age:28}]})
```

age<=23 or age>=33

```
db.ct.find({$or:[{age:{$lte:23}},{age:{$gte:33}}]})
```

count 计数查询

```
db.ct.find({age:{$gt:30}}).count()
```

#### 文档删除

删除单个文档

```
db.ct.findOne({Name:"Wang"})._id
db.ct.deleteOne({_id:ObjectId("xxxx")})
```

删除多个文档

```
db.ct.deleteMany({"age":{$gt:25}})
```

删除所有

```
db.ct.drop()
db.ct.remove()
```

#### 文档更新

```
db.ct.updateMany(filter,update,options)
```

- filter：过滤条件
- update：更新操作表达式
- options：更新参数，如 upsert 参数（如目标文档不存在，是否插入新文档）

### 文档连接查询

相当于 RDB 中的左外连接运算

```JSON
db.ct.aggregate([
	{
		$lookup:
			{
				from:"inventory",
				localField:"item",
				foreignField:"sku",
				as:"inventory_docs"
			}
	}
])
```

### 文档聚合和管道操作

#### 单一目的聚合方法

```
db.ct.distinct("item")
db.ct.find({status:"A"}).count()
```

#### 聚合管道

```
db.ct.aggregate([
	{$match:{status:"A"}},
	{$group:{_id:"$cust_id",total:{$sum:"$amount"}}}
])
```



### 索引机制

可用 explain（）方法来了解 find 查询都利用了哪些索引

- 每个集合的_id 字段默认创建索引
- 支持全文索引、地理位置索引



创建索引

```
db.ct.createIndex(keys,options)
```

- keys 为要创建索引的字段，1、-1 分别按升降序创建索引
- options 课设置是否后台创建、是否创建唯一索引、是否创建稀疏索引、指定索引名称

查询索引

```
db.ct.getIndexes()
```

删除索引

```
db.ct.dropIndex("Name_1")
```

全文索引



地理位置索引



## MongoDB 集群架构

![shadow-image-20230402181515116](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230402181515116.png)

- shard：用于存储实际的数据块，实际环境中一个 shard可由多台机器组成的复制集承担
- router mongos：前端路由
- config server：配置服务器存储整个 cluster 的元数据，包括分快（chunk）信息

### 分片机制

- 以集合为基本单位，集合中的数据通过shard key 被分成多个部分
- 片键对应集合中一个或多个字段，作为数据拆分的依据，选取原则
  - 片键基数大（取值多）
  - 片键分布情况（尽可能均匀）
- 片键必须有索引

### 数据冗余复制集

- 主从式架构，至少需要两个节点

- 可以有一个 Arbiter 类型节点：只参与投票，不能被选为 primary，本身不存储数据，是非常轻量级的服务



### Journaling 日志

- 数据恢复的重要功能
- 本质是一种变更操作持久化预写（write ahead）机制
- 磁盘上的 journaling 文件是实现写操作持久化保存的第一个地方
- 满足一定条件时将日志从内存缓冲区写入磁盘
- wiredTiger 会自动删除旧的 Journal 文件，仅维护从上一个 checkpoint 恢复数据所需要的文件

![shadow-image-20230402182312445](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230402182312445.png)

# 未完成

