##  5contents

[toc]

# Cassandra

![image-20230522145727491](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230522145727491.png)

## 基础

### 数据类型

1. 原生类型

   1. 字符串
   2. 整型
   3. 浮点型
   4. 日期型
   5. 其他

   ![image-20230522151152090](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230522151152090.png)

   ![image-20230522151203742](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230522151203742.png)

2. 集合类型

   1. 列表型：值可以重复`list<T> [value, value,....]`
   2. 集合类型：值不能重复`set<T> {value,value, ...}`
   3. 键值对类型：使用`column['key']`来访问`map<T,T> {'key1':value1,'key2':value2,...}`
   4. frozen：对前面三总类型的限定，将其所有元素序列化，形成一个整体，在此之后只能对整体进行操作

3. 用户自定义类型 User_defined_tyoe

   1. 创建类型

      ```cql
      create type address(
      	province text,
        city text,
        region text,
        HouseNumber text
      );
      ```

   2. 查询所有 UDT 类型：`describe types`

   3. 查看某个类型：`describe type address`

   4. 修改某个 UDT：

      ```cql
      alter type address add PostCode text;
      ```

   5. 删除 UDT 类型：`drop type address`

4. 元组tuple：匿名 UDT，默认是 frozen 的

## Cassandra 数据管理操作

### 键空间操作

创建键空间

```cql
// 简答策略
CREATE Keyspace ks_test
WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 3};
// 网络拓扑策略
CREATE Keyspace ks_multidctest WITH replication = {'class': 'NetworkTopoLogyStrategy', 
'DC1' : 1, 'DC2' : 3}  AND durable_writes = false;
```

查看键空间

```cql
DESCRIBE ks_test;
```

切换键空间

```cql
use ks_test
```

修改键空间属性

```cql
ALTER KEYSPACE ks_test  WITH
REPLICATION = {'class': 'SimpleStrategy', 'replication_factor' : 1} 
AND durable_writes =false;
```

删除键空间

```cql
DROP KEYSPACE ks_test;
```

### 数据表操作

创建表

```cql
// 单个主键
create table IF NOT EXISTS users (
	id bigint primary key,
	username text,
	age int,
	height double, 
	birthday date,
	isvip boolean,
	ip inet,
	hobbies list<text>, 
	skills set<text>, 
	scores map<text, int>, 
	tags tuple<text, text>, 
	createtime timestamp,
) with comment = 'user info table';
// 多个主键
CREATE TABLE CPT (
CID int,
PID int,
REMARK text, 
PRIMARY KEY (CID,PID)  );

create table if not exists Product_Info(
	id bigint,
  name text,
  type text,
  stat_month int,
  suggest_price double,
  actual_price double,
  selt_num int static,
  PRIMARY KEY((id),stat_month)
)
```

查看表

```cql
describe table name
// 查看所有表
describe tables
```

 删除表

```cql
drop table user1
```

 修改表结构信息

```cql
// 添加列
alter table user add column cql_type
// 删除列
alter table users drop (height,temp)

```

### 数据 CRUD 操作

数据插入

```cql
// 插入单挑数据
NSERT INTO Movies (movie, director, main_actor, year) 
VALUES ('movie1', 'director1', 'actor1', 2023)
USING TTL 86400;
// 用 json 插入
INSERT INTO Movies JSON '{"movie": "movie1", "director": "director1", "main_actor":"actor1","year": 2023}';
```

数据查询

```cql
SELECT * FROM users;
// 返回结果为 json 形式
SELECT JSON username, occupation FROM users WHERE id = 1;
// 用 where 查询没有创建索引的非主键字段
SELECT id, username, createtime, tags
FROM users 
WHERE id in(1, 2) and age > 18 AND tags = ('Beijing', 'Programer')
allow filtering;      //尽量少用，一般通过主键进行等值查询特定行
```

数据修改

```cql
update users using ttl 60 set username = 'hehe' where id = 1;
// 删除行
DELETE FROM Users WHERE id=1;
// 删除特定列
DELETE ip FROM Users WHERE                 53                               id IN (1, 2);
```

### 索引操作

一级索引：通过主键快速访问数据

二级索引：通过其他列快速访问数据

```cql
CREATE INDEX users_username_idx ON users(username);

DROP INDEX users_username_idx;
```

### 函数支持

聚合函数

```
SELECT AVG (players) FROM plays;
```

标量函数：简单的取多个值并用它产生输出

cast 函数，转换数据类型

```
SELECT cast(score as text) FROM studentTable;
```

token 函数，参数为列名称，计算给定分区键的 token

```
SELECT token(id) FROM users where id=1;
```

## Cassandra 集群管理

### 读写一致性管理

查看当前一致性级别

```cql
consistency;
// 修改一致性界别
consistency one;
```

### 节点状态管理

