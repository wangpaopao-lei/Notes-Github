# 实验二实验报告

## 实验概述

### 实验任务

捕获在连接 Internet 过程中产生的网络层分组：DHCP 分组，IP 数据分组，ICMP 分组

1.  捕获和分析 DHCP 报文 描述捕获方法及过程，描述 DHCP 协议的功能和分配 IP 地址的过程。
2. 捕获和分析 ICMP 报文 描述捕获方法及过程，描述 ICMP 报头的格式及各字段的作用。 
3. IP 包的分段功能的分析 描述捕获方法及过程，描述所有分片的包长度、DF、段标识、MF、偏移量的值。
4. TCP 建立连接和释放连接的分析 描述捕获方法及过程， 画出 TCP 建立连接和释放连接的消息序列图， 表明各消息中 SYN、 ACK、FIN、发送序号、确认序号的值。

### 实验环境

Windows10专业版

wireshark-win64-3.6.5

## 实验步骤

### 准备阶段

1. 启动计算机，确保能正常上网
2. 运行 wireshark，选择活跃的网络接口 WLAN

### 捕获 DHCP 报文

1. 设置捕获过滤器 udp port 67，开始监控
2. 进入命令行窗口，使用命令 `ipconfig/release`，释放主机 IP
3. 此时捕获到一条 DHCP 消息
4. 使用命令 `ipconfig/renew`重新分配 IP 地址

5. 此时捕获到 4 条 DHCP 消息

![image-20220612221525043](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220612221525043.png)

### 捕获 ICMP报文

1. 开启 wireshark 监控，使用` ping www.bupt.edu.cn`
2. 设置显示过滤器 icmp
3. 捕获到 8 条 icmp 报文

![image-20220612220458377](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220612220458377.png)

### 长 IP 分组的分片捕获

1. 开启 wireshark 监控，使用 `ping 211.68.69.240 -l 8000 -n 1`
2. 其中 `211.68.69.240` 为 `www.bupt.edu.cn` 的 ip 地址，`-l 8000` 表示制作 8000 字节的 IP 数据报，`-n 1` 表示进行一次测试（默认为 4 次，便于分析）
3. 设置显示过滤器`ip.src eq 211.68.69.240 or ip.dst eq 211.68.69.240`，表示接受和发送到该 ip 地址的报文
4. 捕获到 5 条分片和一条 ICMP 消息

![image-20220612221108082](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220612221108082.png)

<div STYLE="page-break-after: always;">

## 协议分析

### DHCP 协议

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220611163113385.png" alt="image-20220611163113385" style="zoom:50%;" />

工作在应用层的动态主机分配协议

<div STYLE="page-break-after: always;">

### DHCP 报文分析

#### DHCP discover

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220612223403292.png" alt="image-20220612223403292" style="zoom:50%;" />

主机向服务器（广播）发送地址分配请求

- 不知道服务器在哪，广播发送
- 因为还没有自己的 IP地址，因此 src 全 0
- 附带自己的 MAC 地址

#### DHCP offer

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220612223521416.png" alt="image-20220612223521416" style="zoom:50%;" />

服务器响应，分配 IP 地址

- 未获得该 IP 使用权，根据 MAC 地址发送
- 指明分配的 IP 地址和使用期限

#### DHCP request

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220612223828170.png" alt="image-20220612223828170" style="zoom:50%;" />

确认该 offer

#### DHCP ACK

![image-20220612223945869](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220612223945869.png)

收到 ACK，正式拿到该 IP 地址

<div STYLE="page-break-after: always;">

### ICMP 协议

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220611145945151.png" alt="image-20220611145945151" style="zoom:50%;" />

工作在网络层的协议

**功能**

1. 差错报告
   1. 目的地不可达（无法传递）
   2. 超时（超出 TTL）
   3. 参数出错（无效头字段
2. 控制
   1. 源抑制（拥塞控制）
   2. 重定向（路由优化）`traceroute`命令
3. 请求应答（测试）
   - `ping `命令

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220606162301942.png" alt="image-20220606162301942" style="zoom:50%;" />

<center>
  IP 数据报格式
</center>

<div STYLE="page-break-after: always;">



### ICMP 报文分析

（仅分析第一次） Echo request

| 字段       | 报文          | 解释                                                |
| ---------- | ------------- | --------------------------------------------------- |
| 版本       | 4             | IPv4                                                |
| 首部长度   | 5             | 表示 20 字节                                        |
| 服务类型   | 00            | 未使用                                              |
| 总长度     | 60            | 首部与数据共 60 字节                                |
| 标识       | 0xa567        | 数据报的 id                                         |
| 标志       | 0x00          | MF 表示最后一片，DF 表示允许分片，Offset 偏移量为 0 |
| TTL        | 64            | 64 跳                                               |
| 协议       | ICMP          | 使用 ICMP 协议                                      |
| 头部校验和 | 0xfb0f        | 判断头部是否出现误码                                |
| 源地址     | 192.168.0.109 | 发送方 IP 地址                                      |
| 目的地址   | 211.68.69.240 | 目的地址www.bupt.edu.cn 的 IP 地址                  |

<center>
  ICMP 包头分析表
</center>



<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220612230320537.png" alt="image-20220612230320537" style="zoom:50%;" />

<div STYLE="page-break-after: always;">

### IP 包分片包分析

| 分片编号 | 协议类型 | DF 标志 | MF 标志 | Offset 偏移量 | 长度 |
| -------- | -------- | ------- | ------- | ------------- | ---- |
| 1        | IP       | 0       | 1       | 0             | 1514 |
| 2        | IP       | 0       | 1       | 1480          | 1514 |
| 3        | IP       | 0       | 1       | 2960          | 1514 |
| 4        | IP       | 0       | 1       | 4440          | 1514 |
| 5        | IP       | 0       | 1       | 5920          | 1514 |
| 6        | IP       | 0       | 0       | 7400          | 642  |

<center>
  IP 分片包头分析表
</center>



![image-20220612231637153](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220612231637153.png)

**说明：**

1. 因为最后一个分片携带了ICMP报文，所以其被Wireshark识别为ICMP协议，对照其他几个分片，验证了 ICMP 协议属于 IP 协议
2. DF(Don’t Fragment)字段=1时不允许分片，=0时允许。一般设置为0
   MF(More Fragment)字段=1，表示后面“还有分片”；=0 表示最后一个分片。路由器或主机收到分片时检查该字段，若收齐了全部分片就还原IP分组。
3. 根据 RFC894 标准的规定，以太网封装IP数据包的最大长度是 1500 字节，所以数据链路层的最大传输单元（Maximum Transmission Unit，MTU）是1500字节。而网络层中IP数据报的报头固定长度为20字节，报头的可变部分不常用，所以单个IP数据报能携带的数据的最大长度为1480字节。
4. IP数据报头的分片偏移字段为该分片的首字节在原IP数据报中的偏移量

<div STYLE="page-break-after: always;">

## 实验心得

1. 本次实验一共花费 2 个小时左右，前期的抓包过程比较简单，主要时间花费在协议的分析和实验报告的撰写上。

2. 进行 TCP 连接建立和释放实验时，由于后台网络进程过多导致抓的了很多很多无关的包，在设置显示过滤器后又由于端口问题抓不到包，迫于时间关系放弃了这个环节

3. 在 DHCP 协议分析中，我注意到 DHCP offer 的报文里，目的地址为本机的 MAC 地址，而非课上讲的广播发送![image-20220612232638473](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220612232638473.png)

   查阅资料后我找到了答案，discover 报文 bootp 的 flag 可以设置响应报文为单播还是广播

   此处引用知乎上的回答：

   >DHCP为了增强协议的健壮性(Robustness)，是这样规定的:
   >
   >如果协议栈在初始化过程中，不接收单播IP报文，请在DHCP Discovery / Request报文的Flags里明确告知服务器，通过设置"BROADCAST flag= 1"，服务器就使用广播来和客户端通信。
   >如果协议栈在初始化过程中，可以接收单播IP报文,请在DHCP Discovery | Request报文的Flags里明确告知服务器，通过设置"BROADCAST flag = 0"，服务器就使用单播来和客户端通信。

4. 这次实验，我自己动手分析了 DHCP、IP、ICMP 协议报文，不仅是对协议的理解更深了一步，更有一种从实际应用层面往回推理论学习成果的顿悟感，更是会激励我们对理论知识的学习。