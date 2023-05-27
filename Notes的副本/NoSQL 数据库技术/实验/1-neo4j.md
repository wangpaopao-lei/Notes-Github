

# Neo4j 数据库技术实验报告







<center>
  姓名：王磊
</center>

<center>
  学号：2020211538
</center>

<center>
  提交日期：2023-3-27
</center>



## Content

[toc]

## 实验环境说明

- 实验平台：windows11 专业版
- 数据库版本：neo4j 5.5 community
- 工具：web 控制台

## 属性图模型

![shadow-image-20230323181808600](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230323181808600.png)



## NDB1

![shadow-image-20230323171247410](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230323171247410.png)

尝试过更改数据库，失败告终



## NDB2 节点操作

### NDB2-1

```cypher
CREATE(P1:Person:User{name:"P1",age:21,Uid:241423}),(P2:Person:User:Uploader{name:"P2",age:24,Uid:264656,fans:12365}),(P3:Person:User{name:"P3",age:35,Uid:265578}),(P4:Person:User{name:"P4",age:36,Uid:299014}),(P5:Person:Uid:Singer{name:"P5",age:28,Uid:100485}),(V1:Video{tittle:"V1",type:"music",vid:7796322}),(M1:Song{tittle:"M1",genre:"pop",album:"MA1"});
```

![shadow-image-20230323184046582](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230323184046582.png)

对照属性图将所有节点一次性创建。

### NDB2-2

![shadow-image-20230323184733385](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230323184733385.png)

可以看到起个节点创建成功。

### NDB2-3

```cypher
match (n) return count(distinct n);
```



![shadow-image-20230323184952865](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230323184952865.png)



### NDB2-4

```cypher
match(n) where n.name="P1" delete n;
```



![shadow-image-20230323185229852](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230323185229852.png)



## NDB3关系操作



### NDB3-1

```cypher
match (p1{name:"P1"}),(p2:Uploader{name:"P2"}),(p3{name:"P3"}),(p4{name:"P4"}),(p5{name:"P5"}), (v1{tittle:"V1"}),(m1{tittle:"M1"})create (p1)-[r1:follow{time:20230201,desc:"newhere"}]->(p2),(p1)-[v2:like{time:20230203,star:5}]->(v1),(p2)-[v3:upload{time:20230202,state:1}]->(v1),(v1)-[r4:singing{time:20230202,type:6}]->(m1),(p3)-[r5:lyricsof{time:20130618,desc:"a poem"}]->(m1),(p4)-[r6:composeof{time:20130910,desc:"..."}]->(m1),(m1)-[r7:masterpiece{time:20200309,desc:"make famous"}]->(p5);
```

![shadow-image-20230324001745819](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230324001745819.png)

一次性创建所有关系，由于是基于已有节点创建关系，所以要使用 MATCH 语句。

这里 nei4j 报了警告，不影响结果。

### NDB3-2

![shadow-image-20230324001833792](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230324001833792.png)

### NDB3-3

![shadow-image-20230324001914262](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230324001914262.png)



### NDB3-4

```cypher
match (m:Person)-[r:upload]->(n:Video) where m.name="P2"and n.tittle="V1" delete r;
```

![shadow-image-20230324002152042](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230324002152042.png)





## NDB4 属性操作



### NDB4-1

```cypher
match (m:Person{name:"P2"})-[r:upload]->(n:Video{tittle:"V1"}) set r.author="P2" return r;
```

![shadow-image-20230324004826462](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230324004826462.png)



### NDB4-2

```cypher
match (m:Person{name:"P3"})-[r:lyricsof]->(n:Song{tittle:"M1"}) set r.time=20180723 return r;
```

![shadow-image-20230324005105326](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230324005105326.png)



### NDB4-3

```cypher
MATCH (m:Person{name:"P4"})-[r:composeof]->(n:Song) REMOVE r.desc return r;
```

![shadow-image-20230324005420054](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230324005420054.png)





## NDB5 标签操作



### NDB 5-1

```cypher
match (m{name:"P3"}),(n{name:"P4"}) set m:Teacher,n:Teacher return m;
```

![image-20230324005835775](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230324005835775.png)



### NDB5-2

```cypher
MATCH (m:Person:Teacher) return m;
```

![image-20230324010004585](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230324010004585.png)



### NDB5-3

```cypher
match (m:Teacher{name:"P3"}) remove m:Teacher return m;
```

![image-20230324010146009](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230324010146009.png)



## NDB6 数据导入

users.csv

![image-20230324120802360](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230324120802360.png)

```cypher
load csv with headers from "file:///users.csv" as row create(n:Person:User) set n=row,n.name=row.name,n.age=toInteger(row.age),n.Uid=toInteger(row.Uid);
```

创建一个 users.csv，里面记载了三个待插入的节点信息。（忘记截图了）

## NDB7 图遍历操作

![image-20230324235910357](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230324235910357.png)

### NDB7-1

```cypher
match p=(m:Person{name:"Tom Cruise"})-[*..3]->(n) return distinct n;
```

![image-20230325000044039](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325000044039.png)



### NDB7-2

```cypher
match (m:Person{name:"P2"}),(n:Person{name:"P3"}),p=shortestpath((m)-[*..4]->(n)) return p
```

![image-20230325000356157](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325000356157.png)

出现了意料之外的结果，查询显示无结果，反复检查代码后想到可能它们之间没有路径，回去查看属性图发现果然如此

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325102027918.png" alt="shadow-image-20230325102027918" style="zoom:50%;" />



## NDB8 APOC 库

### NDB8-1

![image-20230325002236207](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325002236207.png)

### NDB8-2

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325003032184.png" alt="image-20230325003032184" style="zoom:50%;" />

![image-20230325003046683](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325003046683.png)



## NDB9 Python 访问

为了与前面创建的节点作区分，该实验中的 P2_2 代指 P2，以此类推



```python
from py2neo import Graph,Node,Relationship
graph = Graph("http://localhost:7474", auth=("neo4j", "2323765581"),name="neo4j")

# 定义节点对象
p2_2=Node('Person','User','Upload',name="P2_2",age=24,Uid=264656,fans=12365)
m1_2=Node('Song',tittle="M1_2",genre="pop",album="MA1")
v1_2=Node('Video',tittle="V1_2",type="music",vid=7796322)
# 定义关系对象并添加属性值
p2_upload_v1=Relationship(p2_2,'upload',v1_2)
p2_upload_v1["time"]=20230202
p2_upload_v1["state"]=1
v1_singing_m1=Relationship(v1_2,'singing',m1_2)
v1_singing_m1["time"]=20230202
v1_singing_m1["type"]=6
# 创建节点和关系
graph.create(p2_upload_v1)
graph.create(v1_singing_m1)

```

![image-20230325014823797](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325014823797.png)

![image-20230325014922790](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230325014922790.png)