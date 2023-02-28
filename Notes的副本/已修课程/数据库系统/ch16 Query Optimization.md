## Outline

![image-20221122132323435](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221122132323435.png)

## 目录

[toc]





# 查询优化

## Heuristic Optimization——启发式优化

----

思想：

1. 选择操作尽量早执行（减少元组数量）
2. 投影操作早执行（减少属性数量）
3. 先执行限制最多的选择和连接操作



如何贯彻这些规则？

### Equivalence rules等价变换规则

1. 选择串接律![image-20221122133246725](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221122133246725.png)

2.  选择交换律![image-20221122133401974](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221122133401974.png)

3. 投影串接律![image-20221122133424116](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221122133424116.png)

4. 选择和笛卡尔积结合律![image-20221122133730453](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221122133730453.png)

5. 自然连接交换和结合律![image-20221122133747208](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221122133747208.png)

6. 选择对连接操作的分配律![image-20221122134305517](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221122134305517.png)

7. 投影对连接操作的分配律![image-20221122135224645](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221122135224645.png)

8. 并操作和交操作的结合律![image-20221122135652454](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221122135652454.png)

10. 选择操作对集合操作的分配律![image-20221122135713656](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221122135713656.png)

11. 投影对并操作的分配律



### 优化思想

![image-20221122140726372](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221122140726372.png)

### 步骤

1. 构造查询树
2. 按优化规则、变化规则进行优化

#### 查询树

1. 表达关系代数语法树形结构
   - 叶节点：关系
   - 非叶节点：各种关系代数操作
   - 由底向上执行
2. 从 SQL 构造查询树
   - FROM 对应笛卡尔乘积
   - WHERE 对应选择操作
   - SELECT 对应投影操作

### 优化算法

输入：一个关系代数的语法树

输出：计算表达式的一个优化程序

执行步骤：对查询书从顶向下进行优化

1. 利用选择串接律，将选择操作分成单个独立
2. 对每个选择操作，利用交换律、和投影的交换律、和连接/笛卡尔积的分配律、和集合的分配律，尽可能移到靠近叶节点
3. 对投影操作，尽可能移到叶节点或删除某些投影操作
4. 将串接的==多个选择和投影组合成一个==
5. 利用笛卡尔积/连接/结合操作的结合律，按照==小关系先执行==的原则，安排顺序
6. 组合笛卡尔积和相继的选择操作为==连接操作==
7. 对每个叶节点加必要的投影操作，消除对查询无用的属性
8. 分组