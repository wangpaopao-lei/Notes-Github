## Outline

![image-20221025145617994](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221025145617994.png)

## Content

---

[toc]

# Relational Database Design 

## Functional Dependencies函数依赖

---

任意时刻的任意两个元组，在α上的取值相同，则在β上的取值相同

表达数学上的语义联系

### 平凡依赖

trivial

In general, $\alpha \to \beta$ is trivial if $\beta \subseteq \alpha$

子集依赖

### 传递依赖

transitive

a 能函数确定 b，b 能函数确定 c，***b 不是 a 的子集***，==且 b 不能函数确定 c==（若不满足这一条即为直接确定依赖，举例：学号、身份证号、姓名）

则 a 能函数确定 c

### 部分依赖

partial

$\alpha\to\beta,\gamma\subset\alpha,\gamma\to\beta$

真子集即可确定依赖，因此是部分依赖

举例：学号+姓名→性别，学号→性别，部分依赖

### 逻辑蕴含



### 函数依赖集闭包





### 阿姆斯特朗公理

自反律

增广律

传递律







## Normal Forms关系范式

---

### 1NF





### 2NF





### 3NF





### BCNF























编译原理3

软件工程3

数据仓库与数据挖掘2

计算机系统结构3

NoSQL 数据库技术2



数字图像处理

并行计算与 GPU 编程

计算机图形学

现代交换原理

信息与知识获取

网络科学

分布式计算与云计算



三大课程设计

