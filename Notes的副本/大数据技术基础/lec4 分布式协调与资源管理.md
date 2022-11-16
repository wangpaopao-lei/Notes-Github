## Content

[toc]

# ZooKeeper——分布式协调服务

## 特性

---

顺序一致性

原子性

单一系统视图

可靠性

及时性

## 存在意义

---

分布式环境的特点

1. 分布性
2. 并发性：多个节点同时访问
3. 无序性：进程间的直接消息通信

 因此

1. leader 选举
2. 负载均衡

![image-20221108101616716](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221108101616716.png)

## 数据模型

---

文件系统的目录节点数

==维护和监控存储的数据的状态变化==

每个节点被称为 znode

包含以下属性

1. data 数据域
2. type 
   1. persistent 持久化
   2. ephemeral 临时性节点
   3. sequential 记录文件创建顺序
3. version 版本号
4. children 子节点，但是 ephemeral 类型的节点因为存在生命周期，因此不存在子节点
5. ACL 访问控制列表，表示节点可访问的用户

- Watcher：发布/订阅机制，可以在某个节点上注册 watcher 以监听，以事件形式将变化内容发送给监听者，一旦触发就会删除
- session：客户端与 zookeeper 服务端的通信通道，同一个 session 中消息有序

## 基本架构

---





## 程序设计

----

### API

### 配置管理模块



### 服务发现模块设计与实现





## 应用案例

---

### leader 选举



### 分布式队列



### 负载均衡



# YARN——资源管理与调度系统

