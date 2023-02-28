## Content

[toc]

# 分布式结构化存储

数据类型多样

存储没有固定模式

数据十分稀疏

传统存储系统无法满足需求

## Hbase——列簇式存储系统

---

1. 数据模型
2. 内部原理
3. 应用实例

### 数据模型

#### 逻辑数据模型

> 用户从数据库所看到的模型

由一系列行构成，每行数据有一个 rowkey，以及若干 column family 构成

- row key 行键：类似“主键”，唯一标识该行

- ==column family 列簇==

- column qualifier 列标识符

- cell 单元格：通过 rowkey、column family、column qualifier 定位

- ==times tamp 时间戳==

#### 物理数据模型

> 面向计算机物理表示的模型，数据在存储介质上组织结构

- 列簇式存储引擎

- 列簇内部按行存储（空字段不存储，因此能解决稀疏数据的高效存储问题）

- 历史数据包含重要信息，可以提供对比，存储中用不同行来表示不同版本（时间戳加以区别）

- 同一表中的数据按照rowkey 升序排列

- 同一cell 中的数值按照时间戳降序排列（最新的版本放在最上面）

### 内部原理

region 定位

内部关键组件

写

Memstore：随机写转换为顺序写

WAL：避免数据丢失

读

先读 BlockCache，再读 Memstore，最后读 HDFS

优化读的性能：

- 客户端可直接定位 region server 查找数据
- 采用多级缓存的 LRU 算法

优化写的性能：

- 使用 MEMstore，将数据写入内存即通知客户端写入完成，再异步刷入 HFile
- 底层采用LSM 树，将数据存储在内存中，攒到一定量后使用归并排序，顺序写入 HFile 中

### 应用实例

朋友圈：

1. 分布式弹性可伸缩
2. 多机房负载均衡
3. 容灾备份

## Kudu——列式存储系统

---

解决的问题：

1.  HDFS 适合李先分析，不支持单条记录的 update 操作，随机读写性能查
2. HBase 可以高效随机读写，不适用 SQL 数据分析，大批量获取数据的性能较差

## 云数据库

---

IaaS

PaaS

SaaS

### UMP 系统

![image-20221101110125064](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221101110125064.png)

### AWS



