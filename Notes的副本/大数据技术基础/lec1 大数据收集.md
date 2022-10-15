## Content

[toc]

# 大数据收集



## Scoop





## Flume——流式数据收集



### 特点

---

1. ==插拔式==软件架构——高度定制
2. 本质上是一个==中间件==——屏蔽了数据源和存储系统
3. 去中心化——可扩展性
4. 语意路由
5. 内置事务支持——可靠性



### 基本架构

---

由一系列 Agent 组件组成（完全分布式）

![image-20221010151017784](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221010151017784.png)

流水线中传递的数据叫 Event

**“Event”**

由头部和字节数据两部分构成

1. 头部：一系列键值对，用于数据路由
2. 数据：封装了要传递的数据内容

**“Agent”**

source-channel-sink

#### Source

接受 Event 的组件，从 client 或上一个 Agent 接收数据，写入一个或多个 channel

主要实现有：

1. Avro、Thrift（内置）
2. Exec：执行指定的 shell，从该命令的输出中获取数据
3. Spooling Directory：监控指定目录池下文件的变化，有新文件时写入 channel
4. Kafka：内置 Kafka consumer
5. Syslog：接受 TCP 和 UDP 协议发送的数据
6. HTTP：接受 HTTP 协议发送的数据

#### Channel

缓冲区，暂存 source 写入的 Event，直到被 sink 发送出去

主要实现有：

1. Memory：内存队列中缓存
2. File：磁盘文件
3. JDBC：数据库中
4. Kafka

#### Sink

从 channel 中读取 Event，发送给下一个 Agent

主要实现：

1. HDFS：将 channel 中的数据写入 HDFS
2. HBase：写入 HBase
3. Avro/trift：发送给指定的 Avro/Trift server
4. MorphlineSolar/ElasticSearch：写入搜索引擎
5. Kafka

![image-20221010154144461](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221010154144461.png)

### 数据流拓扑

---

#### 构建数据流水线

1. 确定流式数据获取方式
2. 根据需求规划 Agent
3. 设置每个 Agent
4. 测试、部署

#### 流式数据获取方式

1. 远程过程调用（PRC），包括 Avro 和 Trift
2. TCP 或 UDP
3. 执行命令：通过 exec source，允许执行 shell 产生流式数据

#### 多路合并

![image-20221010175713000](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221010175713000.png)

#### 多路复用

![image-20221010175733364](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221010175733364.png)



## Kafka ——分布式消息队列

### 四个角度的重要性

---

1. 消息中间件——解耦
2. 消息队列——缓存
3. 发布订阅系统——快速获取新增数据
4. 消息总线——由 kafka 分流进入各个消费者系统



### 特点

---

1. 高性能
2. 扩展性（分布式架构）
3. 数据持久性（存储到磁盘、多副本策略）



### 基本架构

---

producer-broker-consumer

#### 分布式架构

提高数据可靠性：冗余备份（默认为 3）

#### push-pull 架构

优势

1. consumer课根据自己的实际负载和需求获取西湖局，避免 consumer 收到较大压力
2. consumer自己维护已读取信息的 Offset 而不是 broker 维护，使得 broker 更加轻量级

### 组件详解

---

#### Producer

将数据转换为“消息”，通过网络发送给 Broker

不需要指定所有 broker 的地址，只需给定一个或几个初始化 broker 地址即可（可以通过指定 broker 获取其他所有的地址并自动实现负载均衡）



##### “消息”

<topic, key, message>三元组

1. topic：一个 topic 可以分不到不同的 broker 上
2. key：主键，kafka 根据主键将同一个 topic 下的消息划分到不同的 partition，默认基于哈希取模算法
3. message：字节数组，消息的值

![image-20220927104157682](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220927104157682.png)



#### Broker

接 producer 和 consumer 的请求，并把消息持久化到本地磁盘

以 topic 为单位将消息分成多个 partition，每个 partition 有多个副本（数据冗余）。其中一个是 leader 提供读写请求，其余为 follower，只同步数据不提供服务，当 leader 出现问题时，通过选举算法将其中一个提升为 leader

![image-20220927105620705](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220927105620705.png)

#### Consumer

主动从 broker 中拉取（pull）消息进行处理

维护最后一个已读消息的 offset，大大降低了 broker 的压力提高了其吞吐率

#### Zookeeper

分布式服务协调

1. 与 broker ：所有 broker 向zookeeper 注册，当一个 broker 宕掉后，其他 consumer 会通过 zookeeper 发现这一故障，自动分摊该 consumer 的负载
2. 与 consumer：zookeeper 保证各个 consumer的负载均衡，出现故障时重新分摊负载

### 关键技术点

---

#### 可控的可靠性级别

producer 可以通过该同步或异步方式向 broker 发送数据

#### 数据多副本

强一致性：在提供服务前保证数据备份一致，对用户来说就是在任何时刻都相同

***保证强一致性？***

A：消息首先被写入leader partition，之后由leader partition负责将收到的消息同步给其他副本

负载均衡实际上是对 leader partition 的负载均衡

#### 高效持久化

基于 offset 的数据组织方式达到对磁盘的高效读写

#### 数据传输优化

- 批处理：将多条消息组装到一起发送给 consumer
- zero- copy：无需进行系统调用

![image-20220927112601649](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220927112601649.png)

#### 可控消息传递语义

根据接受者可能收到重复信息的次数划分为三种

1. at most once
2. at least once
3. exactly once（很难实现）



### 典型应用场景

---

1. 消息队列
2. 流式计算框架的数据源
3. 分布式日志收集系统的 source 或 sink
4. lambda architecture 中的 source

## 网络数据采集——爬虫

---

### 原理

1. url 抓取
2. 网页遍历
3. 构造 url 队列
   - ![image-20221004104722477](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221004104722477.png)

### 抓取策略

1. 宽度优先策略
2. 反向链接数策略（类比成被引用数），作为评价重要程度的指标
3. PartialPageRank，计算每个页面的 PageRank 的值，按照大小排列

### 更新策略

1. 历史参考策略
2. 用户体验策略
3. 聚类抽样策略，根据不同类别确定更新周期

### 网页爬虫的分布式系统架构

1. Master-Slave：由主机维护抓取队列并分发给各个服务器
2. Peer-to-peer：待抓取 url 通过 hash 得到负责该网页的主机编号

### Scrapy 框架

