## 目录

---

[toc]

# The Medium Access Control SubLayer介质访问控制

## 有线局域网信道分配

---

广播式链路特有的现象：线路冲突

信道分配是用来解决这个问题的技术

![image-20220510002546144](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220510002546144.png)

### 信道分配

![image-20220510010202305](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220510010202305.png)

**随机访问**-区别于受控访问

用户和用户之间不需要中心控制点，自治访问

动态分配信道：信道和用户不是一一绑定关系

#### 动态信道分配

1. 独立传输
2. 单信道
3. 冲突可观察（有冲突时所有站点都可以检测到）
4. Time assumption
   - 连续时间
   - 分时隙
5. Carrier sense 载波监听

#### 动态随机访问技术

1. Pure ALOHA，slotted ALOHA
2. CSMA，CSMA/CD（Ethernet），CSMA/CA（WLAN）

###  Pure ALOHA

第一个介质访问协议，ALOHA（夏威夷语的 hello）

##### 基本思想（随机竞争）

1. 任何时刻只要有站点想发数据就直接发（不用协调）
2. 用 ACK 帧确认收到，ACK 超时就重传
3. 等待时间为 RTT(round-trip-time)

![image-20220512111822500](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220512111822500.png)

脆弱期：2t（==t 为 一帧的发送时延==）时间内，发的前 t 时间和发的 时间只要有其他站点产生帧就会冲突

![image-20220512105318824](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220512105318824.png)

### Slotted ALOHA

只有时隙的开始能发送

脆弱期：t，在本帧发送的 t 时间内其他站点产生的帧会到下一时隙发送

因此效率翻倍

#### 缺陷

1. 不监听载波，容易产生冲突

### CSMA 载波监听多点访问

1. 如果信道空闲，以一个==概率==发送
2. 如果信道繁忙，等到空闲了发
3. 如果两个站点同时等，冲突
4. ACK超时重传
5. ==脆弱期是$\tau$，一帧的传播时延==

#### Nonpersistent CSMA非坚持监听

如果信道忙，等一个随机的时间再监听

#### Persistent-CSMA

一直等到信道空闲

- 1 坚持：信道空闲就发
- p 坚持：信道空闲了以概率 p 发

在局域网中，$\tau$远小于 t，因此 CSMA 效率高于 ALOHA

#### CSMA Comparison

- 1-persistent CSMA：
  - 低负载时高吞吐量低时延
  - 高负载时低吞吐量
- Nonpersistent CSMA：
  - 高负载时高吞吐量

评价 MAC 协议的指标：

- 低负载情况下的时延
- 高负载情况下的吞吐量（信道利用率）

因此低负载时竞争方式更好，高负载时要提高资源利用率因而适合非竞争方式

==三种协议的共同点==：只有超时时才能检测到错误进而重传

### CSMA/CD(With Collision Detection)带冲突检测的载波监听

1. 载波检测：信道繁忙时不发送（避免）
2. 冲突检测：检测到冲突时，停止发送，发送 ==JAM 强化信号==（检测）
3. backoff 退避：发送完 jam，等待随机时间再恢复（恢复）

载波监听、冲突检测、随机退避

- 半双工
- 节省时间和带宽（因为冲突检测和退避）
- ==Ethernet以太网的基础==

#### 三种状态

1. Contention 争用期
2. transmission 传输期
3. idle闲置期

#### 冲突检测

- 时间：$2\tau$
  - 从 A 发出信号到 D，发现 D 在发，信号回到 A

![image-20220513143504515](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220513143504515.png)

- 方法：检测发出去的信号和收回来的信号是不是相同，因此有两个条件：
  1. 接收信号有一定强度
  2. 调制模式

#### 退避时间

选择一个随机时间 r

##### 指数退避

1. 选取一个随机时间 r（在 0-$2^k-1$内选择
1. k 是冲突次数，冲突产生的越多，退避时间越长
1. 十轮以后 k 不再增加
1. 16 轮后报错，由更高层处理

![CSMACD 流程图](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220513144444750.png)

## 无线局域网

---

### 问题

1. Radio 的发送范围是有限的——不是网络内的所有站点都能收到信号
2. 接收方会有很强的干扰——发信号的功率是收信号功率的 1M 倍——不能同时收发信号
3. 因此当多个站点同时收发数据时会产生两个问题
   - 隐藏终端
   - 暴露终端

因此 CSMA/CD 不能用，==强调接收方的信号干扰==

#### 隐藏终端

![image-20220513150041779](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220513150041779.png)

AC 同时和 B 通信时，B 都能收到他俩的信号，因此产生了冲突，但是他们彼此收不到彼此的信号，因此感知不到对方的存在。AC互为隐藏终端

#### 暴露终端

![image-20220513150144523](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220513150144523.png)

B 和 A 通信，C 和 D 通信时，C 是能收到 B 的信号，因此会影响自己与 D 的通信，BC 互为暴露终端。

### MACA 冲突避免多路访问

增加两个控制信号：

- RTS(Request to send)：在通信之前发送的探测包
- CTS(Clear to send)：接收方发回去的确认信号

#### 流程

1. 发送方发送 RTS
2. 接收方收到 RTS，回复 CTS
3. 发送方收到 CTS，发送数据包
4. 接收方收到数据包，发送 ACK

==使用 ACK 的原因==：无线网络容易发生错误，因此必须采取有确认的方式，而 CSMA 中有线网信道质量够高不需要考虑

#### 示例

![image-20220513151138077](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220513151138077.png)

1. A 要与 B 通信，发送 RTS
2. C 和 E 在 A 的范围内收到了 RTS，知道了 A要与 B 通信，保持沉默
3. B 收到 RTS，发送 CTS
4. D 和 E 收到了 CTS，知道了自己在 B 的范围内，==防止产生隐藏终端==，保持沉默

对于其他站点：

1. 既能收到 RTS 又能收到 CTS，保持沉默
2. 只能收到 CTS，为 ==隐藏终端== ，保持沉默
3. 只能收到 RTS，为 ==暴露终端，可以与其他站点通信

#### 产生冲突

两个站点同时给一个站点发送 RTS，两个站点都收不到 CTS

## IEEE 802 标准

![image-20220513152234193](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220513152234193.png)

补充：802.15 近距离通信（蓝牙）

![image-20220514170723825](/Users/wangpaopaopao/Library/Application%20Support/typora-user-images/image-20220514170723825.png)

高速之后，使用交换技术不再有共享介质的问题

# 以太网

## 经典以太网：物理层结构

---

### 物理拓扑结构

BUS：九十年代中期

STAR：

- hub
- Switch

Network Interface Card网卡

Hubs 集线器：集线器连接的设备在一个冲突域内

100(speed) BASE(modulation) -T(media) X(encoding)

### 10BASE5-Thick Ethernet

总线型

![image-20220514171921153](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220514171921153.png)



### 10BASET

没有共享链路（用hub实现）

#### Hub集线器

物理上星状结构，逻辑上总线结构

因此所有连接的站都在冲突域内

#### 编码方式

曼彻斯特编码

0：低→高

1：高→低

#### Repeater中继器

物理层设备

功能：按比特再生信号，作用跟 hub 类似



## 经典以太网：MAC 层结构

---

### IEEE802.3

==1-persistent CSMA/CD==

#### MAC 地址

物理编址，跟网卡绑定，全球唯一

#### 帧长度

最大长度：1518，数据长度 1500bytes

最小帧长：64bytes

- ==用于检测冲突==
- 在 10Mbps 局域网中，最远允许距离为 2500 米，2$\tau$时间为 51.2$\mu s$，这个时间可以发送 512bit 信息，因此要等到检测信号回来，帧长至少要 512bit 即为 64 字节，不足的用 pad 填充

![image-20220514174443972](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220514174443972.png)

### 提高以太网传输数据速度

1. 减少冲突
   - 使用交换式以太网
   - 控制规模：局域网之间用网桥连接
2. 线路提速
   - Fast Ethernet 
   - Gigabit Ethernet

## 交换式以太网

---

![image-20220514175856526](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220514175856526.png)

十字路口→立交桥

### Switch交换机

工作在数据链路层

有一个高速背板

#### 冲突域

每个冲突域只有一个端口，==因此不存在冲突==

#### 广播域

1. 站点发送信息，交换机整帧接收
2. 信息内附带目标站点的 MAC 地址
3. 交换机跟据 MAC 地址转发帧
4. 若地址全为 1，==则无条件向所有连接的站点转发==

## FAST Ethernet 百兆以太网

---

IEEE 802.3u

100BAST-T4、TX

AutoNegotiation自动协商机制

- 协商百兆还是十兆
- 协商全双工（Switch 模式）还是半双工（hub 模式）

### 100BASE-TX

两根 5 类双绞线

不能使用曼彻斯特编码（效率太低）

## Gigabit EthernetG 比特以太网

---

hub 连接：CSMACD 方案

允许点到点链路共享广播域

载波扩展

帧突发

### 流控技术

暂停帧

## 总结

---

![image-20220514182104869](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220514182104869.png)

