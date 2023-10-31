##  contents

[toc]

# Neo4j

![shadow-image-20230311171212285](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230311171212285.png)

## 概述

### 数据类型

节点、边的属性是一系列 k-v 值，key 要求为字符串表示属性的名称，属性有如下基本类型

![shadow-image-20230311202805064](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230311202805064.png)

#### 复杂类型

- MAP 类型
- List 类型
- 时间、日期类型：通过函数调用获取，比如`data()`、`timestamp()`
  - 可以使用 APOC 库函数 `apoc.date.fprmat`完成格式转换
- 结构类型：可以作为 CQL 语句返回结果
  - 节点 Node：包含 Id、Labels、Map（属性）
  - 关系 Relationship：包含 Id、Type、Map 类型、起始节点标识、终止节点标识
  - 路径 Path：由节点和关系的序列组成
- ByteArray 类型：小取值范围数值属性
- Null 类型

## 数据操作基础

创建没有属性点节点

````cypher
CREATE(<node-v-name>:<label-name>)
````



创建有属性的节点

```cypher
CREATE(
	<node-v-name>:<label-name>
	{
		<property1-name>:<property1-value>
		......
	})
```

 创建标签

```cypher
CREATE(<node-v-name>:<label-name1>:<label-name2>)
```

节点查找

```cypher
MATCH(emp:Employee)
WHERE emp.name='Abc' OR emp.name='Xyz'
RETURN emp
```

更改节点属性

```cypher
MATCH (emp{id:121})
 set emp.sal=9000
   return emp
```

 删除节点

```cypher
MATCH (e:Employee)
DELETE e
```

> 当节点含关系时无法执行删除
>
> 可限定要删除节点满足的属性取值及其它需要满足的条件

删除节点属性、关系属性

```cypher
MATCH (book{id:122})
REMOVE book.price
RETURN book
```

删除节点标签

```cypher
MATCH (m:Movie)
REMOVE m:picture
```

==REMOVE 与 DELETE 比较==

- DELETE 删除节点、关系
- REMOVE 删除属性、标签
- 二者都需要配合 MATCH 使用

使用循环批量创建节点

```cypher
WITH["a","b","c"] as coll
FOREACH (value in coll| CREATE(:person{name:value}))
```

### 关系操作

创建有属性的关系

```cypher
CREATE (<node1-v-name>:<node1-label-name>{<define-properties-list})
-[<relationship-v-name>:<relationship-label-name>{<define-properties-list>}]                                                                                                                             ->(<node2-v-name>:<node2-label-name>{<define-properties-list>})
```

匹配已有节点、创建关系

```cypher
MATCH(e:Customer{custID:100}),(cc:CreditCard{cardID:300360})
CREATE (e)-[r:DO_SHOPPING_WITH]->(cc)
```

删除关系

> 需要配合匹配子句指定需要删除关系满足的条件

```cypher
MATCH (cc:CreditCard)->[r]-(e:Customer)
WHERE id(r)=273
DELETE r
```

### 排序和聚合

排序子句

```cypher
MATCH (emp:Employee)
RETURN emp.Eid,emp.name,emp.sal,emp.deptno 
ORDER BY emp.name DESC
```

使用 WITH 子句

```cypher
MATCH (a:Person)-[:ACTED_IN]->(b:Movie)
WITH a,count(b) AS k
WHERE k>0
RETURN a,k
ORDER BY k DESC
```

### 路径操作

路径查询

```cypher
MATCH (bacon:Person{name:"kevin Bacon"})-[*1..4]-(hollywood)
RETURN DISTINCT hollywood
```

### 索引操作

可为具有相同标签的节点属性、关系属性创建索引

可以在 MATCH 或 WHERE 或 IN 运算符上使用这些所有所索引来改进 CQL Command 的执行性能

创建索引

```cypher
CREATE INDEX ON :<label_name>(<property_name>)
DROP INDEX ON :<label_name>(<property_name>)
```

### 约束操作

==UNIQUE 约束==

 ```cypher
 CREATE CONSTRAINT ON(<label_name>)
 ASSERT <property_name> IS UNIQUE
 ```

### 存储过程调用

显示所有标签

```cypher
CALL db.labels()
```

显示所有关系类型

```cypher
CALL db.relationship Types()
```

显示可用的存储过程列表

```cypher
CALL dbms.procedures()
```

## Neo4j 集群技术

因果集群Casual Clustering

集群中节点分为 PRIMARY、SECONDARIES 两类节点

### 因果集群

- 核心服务器 PRIMARY：处理读写操作
- 读复制服务器 SECONDARIES：只负责分担读数据任务，数据从 PRIMARY 异步更新保持一致

==安全性==

核心服务器正常运行时，平台保持高可用

==扩展性==

读复制服务器提供了大规模横向扩展的平台

==因果一致性==

保证客户端应用程序至少能够读取自己的写入

#### 核心服务器维护

基于 RAFT 协议

M=2F+1：F 容错服务器数量，M 核心服务器总数量（如果可容忍一台故障服务器，则需要 3 台服务器）

#### 主从式架构

区别于其他主从式架构，Neo4j 聪姐带你也可以处理写入。但是为了保证数据一致性，从节点将写请求发送给主节点同步处理写入，因==更新操作是最终从主节点传播到从节点==

## Neo4j 管理与监控

### 备份与恢复

```shell
! 备份数据
neo4j-admin database dump --to-path=D:\
! 导入数据库到导出的数据 
neo4j-admin database load --from-path=D:\
```



