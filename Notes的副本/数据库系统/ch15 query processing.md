## Outline

![image-20221209135852903](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221209135852903.png)

## Content

---

[toc]

# Query Processing

![image-20221209140212975](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221209140212975.png)

## 基本步骤

---

（提交）

语法分析与翻译

查询优化

执行

（拿到结果）

## 代价衡量

---

代价衡量：从提交到拿到结果的时间

==磁盘访问==（作为主要衡量标准），CPU 处理（数量级小于磁盘访问时间），网络通信（不在考虑范围内）

磁盘访问：

- 寻址
- 写入、读出



$t_T$——块传输时间

$t_S$——寻址时间



以选择操作为例

1. 线性扫描 $cost=t_T\times b_r /2+t_S$
2. 二分查找$log_2 b_r\times(t_T+t_S)$
3. 按索引查询
   - **候选键上建立主索引、针对候选键进行等值查询**：$(h_i+1)\times(t_T+t_S)$，其中，$h_i$（索引层数）系数代表对索引的操作时间，1 代表读取数据块的时间
   - **非键属性上建立主索引、针对该属性进行等值查询**：==区别：可能返回过个结果==$h_i\times(t_T+t_S)+t_S+t_T\times b$
   - **建立辅助索引、在 search key 上做等值查询：**
     - 如果 search key 是候选键$(h_i+1)\times(t_T+t_S)$
     - 如果 search key 不是候选键$(h_i+n)\times(t_T+t_S)$
4. 范围查询

