## Content

[toc]

# Kafka ——分布式消息队列

## 四个角度的重要性

---

1. 消息中间件——解耦
2. 消息队列——缓存
3. 发布订阅系统——快速获取新增数据
4. 消息总线——由 kafka'分流进入各个消费者系统



## 特点

---

1. 高性能
2. 扩展性（分布式架构）
3. 数据持久性（存储到磁盘、多副本策略）



## 基本架构

---

producer-broker-consumer

### 分布式架构

提高数据可靠性：冗余备份（默认为 3）

### push-pull 架构

优势

1. consumer课根据自己的实际负载和需求获取西湖局，避免 consumer 收到较大压力
2. consumer自己维护已读取信息的 Offset 而不是 broker 维护，使得 broker 更加轻量级

## 组件详解

---

### Producer

将数据转换为“消息”，通过网络发送给 Broker

不需要指定所有 broker 的地址，只需给定一个或几个初始化 broker 地址即可（可以通过指定 broker 获取其他所有的地址并自动实现负载均衡）



#### “消息”

<topic, key, message>三元组

1. topic：一个 topic 可以分不到不同的 broker 上
2. key：主键，kafka 根据主键将同一个 topic 下的消息划分到不同的 partition，默认基于哈希取模算法
3. message：字节数组，消息的值

![image-20220927104157682](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220927104157682.png)



### Broker

接 producer 和 consumer 的请求，并把消息持久化到本地磁盘

以 topic 为单位将消息分成多个 partition，每个 partition 有多个副本（数据冗余）。其中一个是 leader 提供读写请求，其余为 follower，只同步数据不提供服务，当 leader 出现问题时，通过选举算法将其中一个提升为 leader

![image-20220927105620705](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220927105620705.png)

### Consumer

主动从 broker 中拉取（pull）消息进行处理

维护最后一个已读消息的 offset，大大降低了 broker 的压力提高了其吞吐率

### Zookeeper

分布式服务协调

1. 与 broker ：所有 broker 向zookeeper 注册，当一个 broker 宕掉后，其他 consumer 会通过 zookeeper 发现这一故障，自动分摊该 consumer 的负载
2. 与 consumer：zookeeper 保证各个 consumer的负载均衡，出现故障时重新分摊负载

## 关键技术点

---

### 可控的可靠性级别

producer 可以通过该同步或异步方式向 broker 发送数据

### 数据多副本

强一致性：在提供服务前保证数据备份一致，对用户来说就是在任何时刻都相同

***保证强一致性？***

A：消息首先被写入leader partition，之后由leader partition负责将收到的消息同步给其他副本

负载均衡实际上是对 leader partition 的负载均衡

### 高效持久化

基于 offset 的数据组织方式达到对磁盘的高效读写

### 数据传输优化

- 批处理：将多条消息组装到一起发送给 consumer
- zero- copy：无需进行系统调用

![image-20220927112601649](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220927112601649.png)

### 可控消息传递语义

根据接受者可能收到重复信息的次数划分为三种

1. at most once
2. at least once
3. exactly once（很难实现）



## 典型应用场景

---

1. 消息队列
2. 流式计算框架的数据源
3. 分布式日志收集系统的 source 或 sink
4. lambda architecture 中的 source

## 网络数据采集——爬虫

---

