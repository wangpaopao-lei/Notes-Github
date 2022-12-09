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

互斥锁X

共享锁S

锁的相容性矩阵

![image-20221129142814981](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221129142814981.png)



![image-20221129144633816](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221129144633816.png)

可能造成死锁（环形等待）

可能造成饥饿



### 2-阶段锁协议

1. growing：只能申请不能释放
2. shrinking：只能释放不能申请

