## Content

[toc]

# 大数据存储

## 数据序列化

---

将数据对象转化为==字节流==

#### 技术演化

1. 直接转化为字符串——无法表达二进制数据，无法表达嵌套数据
2. 编程语言内置的序列化机制——难以做到跨语言
3. 应用范围广、跨语言的数据表示格式（如 JSON、XML）——解析速度慢、数据冗余大
4. 统一化的 schema

### 序列化的方案

#### Thrift





#### Protobuf



#### Avro

## 文件存储格式

---

### 行存储和列存储

![image-20221004120650096](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221004120650096.png)

#### 行式存储格式sequence file

1. sequence file：key/vallue 二进制行式存储，可存储文本格式无法存储的数据
2. 未压缩：每隔一段数目的 record 写入一个syncmark 便于数据分块和按块压缩![image-20221011100715250](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221011100715250.png)
3. 行级压缩：在 header 中加入压缩相关信息，record 中的 value 值是经过压缩的
4. 块级压缩![image-20221011101854217](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221011101854217.png)

#### 列式存储格式

1. ORC 文件：定义了三个级别的索引![image-20221011103233123](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221011103233123.png)
2. Parquet
3. Carbondate



## HDFS——分布式文件系统

---

文件级别的分布式系统

1. 难以实现负载均衡，文件大小不一
2. 难以实现并行处理





块级别的分布式系统

![image-20221011112501842](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221011112501842.png)



### 基本架构





### 关键技术

1. ==**容错性设计**==
   1. namenode：储备，防止单点故障
   2. datanode：副本
   3. 数据块：校验码
2. 副本放置策略
3. 异构存储介质
4. 集中式缓存管理



### 访问方式

shell

api



### 小结

扩展性：scale out横向扩展

容错性机制

块级别：负载均衡、并行处理

周期性心跳：防止单点故障

第三方组件：zookeeper 服务协调



## NoSQL 数据库

---

acid：atomicity，consistency，isolation，durability

非关系型数据库的统称

关系数据库的缺陷：

1. 无法满足海量数据管理需求
2. 无法满足高并发需求
3. 无法满足高扩展性和高可用性需求

新时代的需求：

1. web2.0 通常不要求严格的数据库事务
2. 不要求严格的读写实时性
3. 不包含大量复杂的 SQL 查询（以存储空间换查询性能）

### 三大基石

#### CAP 理论

==C==onsistency：==一致性==，所有节点在相同点时间都具有相同的数据

==A==valability：==可用性==，不管成功或者失败都有响应

Tolerance of ==P==artition：==分区容错性==，部分节点出问题时，系统也能继续正常运行

一个分布式系统不可能同时满足 CAP 三个性质，最多同时满足 2 个

CA：单点集群，通常可扩展性不强（传统数据库）

CP：通常性能不搞（Redis，MongoDB）

AP：通常对一致性的要求降低（大多数的选择，*满足了大数据时代思考方式的转变：效率而非准确性——牺牲了一部分的准确性换取效率提升*）

#### BASE 理论

==B==asicly ==A==valable：基本可用，一部分发生问题时，其他部分仍可以使用

==S==oft state：软状态，数据可以有一段时间不同步

==E==ventual consistency：最终一致性，允许访问操作读不到更新后的数据，但是经过一段时间后一定达到一致的状态

#### 最终一致性

有几种不同的表现形式：

1. 因果一致性
2. 读己之所写
3. 单调读一致性
4. 会话一致性
5. 单调写一致性

![image-20221018111934215](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221018111934215.png)

### 类型

key-value

key-column

key-document

graph 

















100.125.64.250

100.125.1.250

