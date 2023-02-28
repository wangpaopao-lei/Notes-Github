## Outline

1. ![image-20221129142252432](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221129142252432.png)

## 目录

[toc]





# 并发控制

## 并发控制协议

---

基于锁的协议

基于时间戳的协议

### Locks

互斥锁X：加锁后就排他，别人申请不到

共享锁S：加锁后别人还是能申请

锁的相容性矩阵：

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221129142814981.png" alt="image-20221129142814981" style="zoom:25%;" />

### 基本锁协议

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221129144633816.png" alt="image-20221129144633816" style="zoom:25%;" />

1. 读操作之前加共享锁
2. 写操作之前加互斥锁

==思想==：执行操作之前加锁，完成操作后释放

**问题**：

不能保证调度可串行

可能造成死锁（环形等待）

可能造成饥饿



### 两阶段锁协议

把==**事务**==分成两个阶段 

1. growing：只能申请不能释放
2. shrinking：只能释放不能申请

==一定是冲突可串行的：等价于按锁点（事务得到最后一把锁的点）的顺序串行的调度==

不能避免死锁

不能保证无级联回滚

***严格两阶段锁协议***

所有事务的互斥锁要保持到提交之前释放

***强两阶段锁协议***

所有锁都要保持到提交之前释放
