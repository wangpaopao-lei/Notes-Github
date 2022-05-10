# 第三章 存储系统

![image-20220403160354393](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220403160354393.png)

## 存储系统的层次结构

### 程序的局部性原理

在某一段时间内频繁访问某一局部的存储器地址空间，而对此范围之外的地址空间则很少访问

1. 时间局部性
2. 空间局部性

### 多级存储体系的组成

![image-20220403161925440](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220403161925440.png)

* 整个存储系统在速度上接近 Cache。在容量和价格上接近外存
* 主存：$N\times K$
  1. $R/\bar W$线：控制读/写
  2. K 位数据总线：字长为 K
  3. n 位地址总线：$2^n=N$，二进制表示所有存储单元

## 随机读写存储器（RAM）

### 静态随机存取存储器（SRAM）

![image-20220403171750490](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220403171750490.png)

#### 位扩展、字扩展、位字扩展

$256\times1bit->256\times8bit$

#### SRAM 时序——读周期

![image-20220403224739811](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220403224739811.png)

#### SRAM 时序——写时序

![image-20220403224900801](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220403224900801.png)

### 动态随机存取器（DRAM）

#### 管脚信号

![image-20220403224957960](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220403224957960.png)

1. nWE：写使能
2. $D_1——D_4$：数据输入/输出
3. $A_0——A_9$：地址线，分时传送低 10 位行地址和高 10 位列地址（存储容量远大于 SRAM，因此没办法表示所有地址，地址线要==分时复用==）
4. nRAS：行地址选通信号
5. nCAS：列地址选通信号
6. nOE：Output Enable

![image-20220403225839472](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220403225839472.png)

 #### DRAM 时序——读写

![image-20220403231252910](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220403231252910.png)

#### DRAM 时序——刷新周期

![image-20220403231423753](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220403231423753.png)

#### 刷新策略

1. 集中式刷新 burst refresh

   * 刷新安排在刷新周期的某一个特定时间
   * 刷新全部地址
   * 刷新按行进行
   * 优点：平均读写周期接近于存储器件的读写周期
   * 缺点：刷新过程中不允许读写，存在“死时间”
2. 分散式刷新 distributed refresh
3. 隐含式刷新

#### DRAM 存储器与 CPU的连接

DRAM 控制器（DRAM-C）：![image-20220404180516347](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220404180516347.png)

用扩展电路实现

#### 缓冲 DRAM（Cached DRAM）

Cache：保存主存中正在使用的信息的副本

CachedDRAM：在 DRAM 主存芯片与 CPU 交互接口上增加的小容量高速缓冲存储器![image-20220404181013826](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220404181013826.png)

* 保存最近访问的一行的副本
* 增加行比较器：如果下次访问的是最近访问过的行，则直接从 cache 中读出
* 最后一行放入行==地址锁存器==和==行地址比较器==
* 优点：提升特殊操作时的性价比
  1. 高速突发（burst）传送（连续访问同一行不同列的地址）
  2. cache 读出的调试进行刷新操作
  3. 读写操作可以并行（一个在 cache 中一个在主存中）

### 端模式Endian Mode

多字节数据的字节顺序

![image-20220404183910955](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220404183910955.png)

![image-20220404184041202](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220404184041202.png)

![image-20220404185718161](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220404185718161.png)

## DRAM 存储器的发展

## 闪存 FLASH 概述

FLAH：电可擦、非易失只读存储器

Intel/amd：NOR 线性闪存（CPU 产商，更关注能否在计算机中取代 EPROM）

Samsung/Toshiba：NAND（消费级产商，更关注能否被用户拿来存储资料）

特点：可在线“写入程序”，又具有 ROM 的非易失性

可以取代全部的 UVEPROM 和大部分 E2PROM

闪存主要用途：

1. 存储监控程序、引导程序等基本不变或不经常改变的程序（NOR 闪存）
2. 存储在掉电时需要保持的系统配置等不常改变的数据（NOR 闪存）
3. 固态盘（NAND 闪存）

### NOR（工艺）线型（编制）闪存

1. 能快速随机存取，读取速度高
2. XIP，CPU 能在闪存中取指令，读指令
3. 单字编程
4. 在对存储器进行重新编程之前对区或整片进行擦除
   * 一区块（sector）或芯片为单位进行擦除
5. 接口：有独立的数据总线和地址总线
6. 信息存储可靠性较高
7. 适用于擦除和编程操作较少而直接执行代码的场合，尤其是纯代码存储应用
8. 不适用于纯数据存储和文件存储
   * 擦除和编程速度较慢
   * 区块尺寸较大

### NAND 非线性闪存

1. 以页为单位进行读和编程操作（颗粒度较大 ）
2. 以块（block)为单位进行擦除
3. 快速编程和快速删除
4. 接口方式：数据、地址采用同一总线（串行）--》管脚数量较少
5. （接上）位成本低，位密度高
6. 较高的比特错误率，需要软件处理失效块
7. 适用于高容量存储设备：存储卡、固态硬盘（更适合外存）

### FLASHvsRAM

1. NOR FLASH比 SRAM 成本低，比 SRAM 集成度高，并且具有非易失性
2. NOR FLASH：非易失性在线可编程只读存储器
3. FLASH 取代 DRAM？
   * 可重复擦除次数约为$10^6$
     * DRAM 约为$10^{15}$
   * 编程速度远低于 DRAM 写入速度
   * 读出速度远低于 DRAM

## 并行存储器

### 加快存储器访问速度

1. 芯片技术——单个芯片访问速度
   1. 选用更高速半导体器件
   2. 改善芯片内部结构和对外接口方式
2. 结构技术——改进存储器与 CPU之间的连接方式
   1. 多口存储器
   2. 多体交叉存储器（多模块交叉存储器）
3. 系统结构技术——从整个存储系统的角度采用分层存储结构
   1. Cache
   2. 虚拟存储器

### 双端口存储器

![image-20220407164537325](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220407164537325.png)

* 控制逻辑还要处理双端口的冲突和仲裁

* 当两个端口存取的存储单元不相同时，读写操作不会产生冲突

* 仲裁逻辑的判断方式：
  * CE 控制的仲裁
  * 地址控制的仲裁

## 小结

1. 存储系统的基本概念
2. RAM
3. ROM
4. 并行存储器

### 本章重点

1. 存储系统的基本概念与性能指标
2. 存储系统接口
3. 提高存储系统性能的方法
