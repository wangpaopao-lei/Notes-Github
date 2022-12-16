## Outline

DBMS 内容

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221204104022194.png" alt="image-20221204104022194" style="zoom: 25%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221204133152554.png" alt="image-20221204133152554" style="zoom:25%;" />



## Content

---

[toc]

# 存储

## 存储访问

---

以块的方式进行访问

调用操作系统的IO 操作，将数据所在的数据块读到内存中

### BufferManager

提升效率——尽量减少 IO 操作（令在内存中保留的块越多越好）

因此需要 Buffer——引出 buffer manager

1. 如果块在缓存里，给出主存中的地址
2. 如果不在缓存里
   1. 在缓存里请求分配空间，替换掉其他块
   2. 扔掉的内容回写到磁盘
   3. 将新的块从磁盘读取到主存，向请求者返回地址

#### 缓存替换策略

==在 DBMS 中，数据的应用在很多情况下是可以预判的，因此会有不同的策略（相较于操作系统）==

1. LRU 最近最少使用原则
2. Pinned Block 钉住的块
   - 正在使用的块需要钉住
3. forced output 强制写出
4. Toss-immediate 立即丢弃
   - 确定不会再使用的块立即丢弃
5. MRU 最近最多使用

#### 共享和互斥

## 文件组织形式

---

主要以记录==（records）==为单位

文件使用操作系统提供的文件结构

记录被映射为磁盘上的数据块

- 定长记录
- 变长记录

### Fixed-length records 定长记录

1. 访问简单，容易找到文件起始和终止的地方
2. 记录删除方式
   1. 后面记录往前移
   2. 最后的记录拿来填
   3. 构建 free lists 记录空闲的记录位置

### Variable-length records 变长记录

1. 文件中可以存储多种记录类型
2. 记录类型允许变长域
3. 记录类型允许重复域
4. 在记录结尾添加特殊符号来进行标识
   - 会产生记录删除和维护的问题

## 记录组织形式

---

Heap：在文件中记录是随机的（堆在一起）

Sequential：文件中记录是排序的

Hashing：

B+ tree：平衡搜索树

clustering：多表聚集文件

### Heap

插入和删除比较容易

维护：空闲空间 map，用几个 bit 来表示每个块有多大空闲空间（比例）

如果块太多，可以建立二级 map

优点：便于维护

缺点：查询效率低下

![image-20221206205239030](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221206205239030.png)

### Sequential

优点：便于查询

缺点：维护不便

### Multiple clustering

把表连接的结果物理上存储在一起

优点：可以节省连接查询

缺点：查询单表时效率很低

## 数据字典存储

---

元数据

用户信息

统计信息（表）

物理模式信息（文件类型、文件位置）

## 列式存储

---

