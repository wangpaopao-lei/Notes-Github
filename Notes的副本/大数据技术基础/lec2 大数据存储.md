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

1. 容错性设计
2. 副本放置策略
3. 异构存储介质
4. 集中式缓存管理

















100.125.64.250

100.125.1.250

