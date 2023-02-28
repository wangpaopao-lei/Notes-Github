## Outline

![image-20221115133900480](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221115133900480.png)

## Content

---

[toc]

# Indexing

提高查询效率

需要物理存储（索引文件）

需要维护一致性

因此也不能建太多

Search Key：属性或属性集合，根据 key 来做索引

Pointer：指针，指向 key 对应的存储位置

![image-20221206212617064](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221206212617064.png)

类比书的目录

![image-20221206225915304](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221206225915304.png)

## 顺序索引

---



按 search key 的取值进行排序

- primary index 主索引也叫聚集索引：索引文件和数据文件顺序一致（数据文件是有序的）

  - 因此，主索引是建立在数据文件排序域上的索引（主索引最多一个）

  - 有主索引的文件成为 index-sequential file

- secondary index 辅助索引：索引文件和数据顺序文件不一致



- dense indice 稠密索引：数据文件的==所有取值==在索引域中都出现
- sparse indices 稀疏索引：可以用分块的取值来建立稀疏索引



- multilevel index 多级索引：当索引文件很大的时候，为了防止内存反复多次执行 IO 操作，使用多级索引



### 索引维护

数据删除

稠密索引：删除指针、或修改指针

稀疏索引：不做、用下一条替代、修改

## 哈希索引

---

知道就行

## SQL

---

```sql
create index <index-name> on <relation-name>
<attribute-list>

cluster
noncluster
unique


drop index<index-name>

```



