



## Outline

1. Relationl model
2. Fundametal relational-algebra-operation
3. Additional r-a-o
4. extended r-a-o
5. Null values
6. Modification of the database

## 目录

[toc]



认识一个数据模型从三个方面：数据结构、数据操作、完整性约束

# 关系模型

数据结构：基于数学上的关系，添加一些现实性的扩充

完整性约束：三类（主键、外键、自定义）

数据操作：关系代数

## 关系数据库的结构（结构）

### Relation → Relational model

数学上的relation：定义在一些域上的笛卡尔积的一个子集

迁移到数据库中：

1. “扩充”：数学上的笛卡尔积不满足交换律，因为位置也包含信息，数据库让笛卡尔积满足交换律——给每一个域加一个名字==属性==，在交换时带着属性交换
2. “限制”1：数学上的关系（笛卡尔积的子集）可以是无限的，对数据库而言无限没有意义
3. “限制”2：笛卡尔积中的任意子集都可以是关系，但是这对数据库是没有意义的，数据库只管理==有意义的数据==

因此关系数据库中的关系：有限、有意义

relation 直观地表示出来，就是 table（关系模型）

### 要求

1. 行不相关
2. 列不相关
3. 属性必须原子
4. 属性不重名
5. 不同属性可以有相同的域
6. 元组不重复

## 数据库模式schema

关系模式即为列



## Key

student 表

1. 身份证号
2. 姓名
3. 学号
4. 专业



选课表a

输出姓名为 A 的学生的信息

| ==学生姓名== | ==课程编号== | 课程教师 |
| ------------ | ------------ | -------- |
| A            | 321          |          |
| B            | 456          |          |
| C            | 789          | 表       |

```sql
select *（All）
  from r1
```

π包含去重操作

```sql
select distinct 学生姓名
	from r1
```

不会去重



课程表b

| 教师姓名 | 课程名 | ==课程编号== |
| -------- | ------ | ------------ |
| 1        |        | 456          |
| 2        |        | 789          |
| 3        |        | 321          |

选课表需要参照课程表

9*6

3 行



加一个条件 a.课程编号=b.课程编号



| a.学生姓名 | a.课程编号 | a.课程教师 | b.教师姓名 | b.课程名 | b.课程编号 |
| ---------- | ---------- | ---------- | ---------- | -------- | ---------- |
| A          | 321        | 1          |            |          | 456        |
| A          | 321        | 2          |            |          | 789        |
| A          | 321        | 3          |            |          | 321        |
| B          |            | 1          |            |          |            |
| B          |            | 2          |            |          |            |
| B          |            | 3          |            |          |            |
| C          |            | 1          |            |          |            |
| C          |            | 2          |            |          |            |
| C          |            | 3          |            |          |            |



### SuperKey超键

一个或多个属性的集合，这些属性的组合可以在一个关系中唯一标识一个元组

### Candidate Key候选键

最小的超键。

### Primary Key主键

数据库设计者选取一个候选键作为主键，是整个关系的一种性质

### Foreign Key外键

对于两个关系$R_1,R_2$，如果$ R_1$包含 $R_2$的主键，就称 $R_1$的该属性是==参照== $R_2$的外键

这是在构建数据库时向 DBMS 定义的，因此这两个属性的名称并不要求一样

外键表达的是客观世界中属性的语义联系



## 模式图schema diagram

![image-20220913151348427](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220913151348427.png)

## 关系数据库的完整性约束（约束）

1. Integrity Constraint of primary key实体完整性约束：主键的每一列的取值都不能为空

2. Referential Integrity constraint参照完整性约束：外键的取值可以为空

3. 用户自定义完整性约束



## 关系代数（操作）

### 基本操作

基本操作：代表关系代数的运算能力的

1. select
2. projection
3. union
4. product
5. difference
6. rename

#### Select

$\sigma_p(r)$

一元操作

r 代表查找范围，p 代表需要满足的条件，输出结果是满足条件的元组

#### Projection

$\prod_{A1,A2,..,An}(r)$

一元操作

An 为属性名，r 代表关系名称，输出指定的属性（指定列），还要做去重操作

#### Union并集

$r\bigcup s$

二元操作

将两个关系竖着拼接，要求

1. 属性个数相同
2. 属性定义域相容
3. 属性名称可以沿用，也可以重新定义（因为关系代数本质是对数据进行操作）

#### Difference差集

$r-s$

二元操作

在 r 中但不在 s 中的元组

要求和 union 时一样

#### Cartesian-Product笛卡尔积

表 1 有 m 行x 列，表 2 有 n 行y 列

- x+y列
- m*n 行

$r\times s$

二元操作

对关系没有要求，输出所有元组对的组合

### Additional Operations

#### Intersection

$r\bigcap s=r-(r-s)$

实际上是两个差操作的组合
#### Join连接

$r\Join_{A\theta B}s=\sigma_{p}(r\times s)$

连接操作，又称有条件的笛卡尔积，即为在做笛卡尔乘积时再进行选择操作

#### Natural-Join

$r\Join s$

条件不言而喻，是最常见的连接形式

#### Outer-Join

外连接，前面说的都是内连接

1. 左外连接：左边的 没连接上的元组也出来，没连上的属性为空值（null）$\sqsupset\Join$
2. 右外连接，$\Join\sqsubset$
3. 全外连接

用途：举个例子，外连接能找出有贷款人没金额和有金额没贷款人的数据，这些都是异常数据

#### Example

找 balance 的最大值

 $\sigma_{account.balance<d.balance}(accout\times \rho_d{(account)})$

### 增删改

#### Deletion

以元组为单位

涉及到差操作，可能不止在一张表中进行删除

#### Updating

以数据项为单位

#### Insertion

以元组为单位

涉及到并操作，也可能不止一张表





![image-20221213181105548](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221213181105548.png)
