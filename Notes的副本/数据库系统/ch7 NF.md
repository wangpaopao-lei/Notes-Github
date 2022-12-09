## Outline

![image-20221025145617994](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221025145617994.png)

## Content

---

[toc]

# Relational Database Design 

## 函数依赖

---

==任意时刻的任意两个元组，在α上的取值相同，则在β上的取值相同==

表达数学上的语义联系（只能表达数值上的相等，实际上起的一种约束的作用）

### 键的新定义

如果 K 是 R 的超键，那么 K 能函数确定 R 上的所有属性。

如果 K 是候选键，在超键的基础上加上，任何 K 的真子集，都不能函数确定 R

### 平凡依赖

trivial

In general, $\alpha \to \beta$ is trivial if $\beta \subseteq \alpha$

子集依赖

### 传递依赖

transitive

a 能函数确定 b，b 能函数确定 c，***b 不是 a 的子集***，==且 b 不能函数确定 a==（若不满足这一条即为直接确定依赖，举例：学号、身份证号、姓名）

则 a 能函数确定 c

### 部分依赖

partial

$\alpha\to\beta,\gamma\subset\alpha,\gamma\to\beta$

真子集即可确定依赖，因此是部分依赖

举例：学号+姓名→性别，学号→性别，部分依赖

### 逻辑蕴含

基于关系模式 R 的函数依赖集 F，f 属于 F，被 F 逻辑蕴含

### 函数依赖集闭包

F*，被 F 逻辑蕴含的所有函数依赖的集合

### 阿姆斯特朗公理

$>$ reflexivity (自反律) : if $\beta \subseteq \alpha$, then $\alpha \rightarrow \beta$

$>$ Augmentation (增广律) : if $\alpha \rightarrow \beta$, then $\gamma \alpha \rightarrow \gamma \beta$

$>$ transitivity (传递律) : if $\alpha \rightarrow \beta$, and $\beta \rightarrow \gamma$, then $\alpha \rightarrow \gamma$

1. 正确的：推出的依赖关系都是正确的
2. 完备的：可以推出所有成立的依赖关系

#### 根据公理系统求 F 闭包

1. 针对所有函数依赖关系，应用自反律和增广律
2. 应用传递律
3. 重复，直到 F*不再变化

#### 公理系统的推理

 union :合并律

If $\alpha \rightarrow \beta$ holds and $\alpha \rightarrow \gamma$ holds, then $\alpha \rightarrow \beta \gamma$ holds $(\alpha \rightarrow \beta, \alpha \rightarrow \alpha \beta ; \alpha \rightarrow \gamma, \alpha \beta \rightarrow \beta \gamma ; \alpha \rightarrow \beta \gamma)$

Decomposition:分解律
If $\alpha \rightarrow \beta \gamma$ holds, then $\alpha \rightarrow \beta$ holds and $\alpha \rightarrow \gamma$ holds $(\alpha \rightarrow \beta \gamma, \beta \gamma \rightarrow \beta, \alpha \rightarrow \beta)$

pseudotransitivity：伪传递律
If $\alpha \rightarrow \beta$ holds and $\gamma \beta \rightarrow \delta$ holds, then $\alpha \gamma \rightarrow \delta$ holds

### 属性集闭包

$\alpha \rightarrow \beta$ is in $F^{+} \Leftrightarrow \beta \subseteq \alpha^{+}$

$\alpha$能函数确定的所有属性的集合

***求解***：

1. 对所有函数依赖关系 a 确定 b，如果 a 是 result 的子集，那么将 b 加入 result 中
2. 当 result 不再变化，停止，输出 result

***作用***：

1. 验证某属性集是否为超键：==求该属性集闭包是否包含所有属性==
2. 检查函数依赖是否成立：==求左边的闭包是否包含右边==
3. 求 F 的闭包：
   - 对任意属性 r，找到其闭包 r*
   - 对任意 S 是 r* 的子集，输出函数依赖 $r\rightarrow S$

### Canonical Cover正则覆盖

又称最小覆盖、最小函数依赖集

#### Extraneous Attribute无关属性

去除函数依赖中的属性不改变其闭包

对$\alpha \rightarrow \beta$ in $F$，如下定义

- 如果 A 属于$\alpha$，F 逻辑蕴含 将这条函数依赖去掉 A 后得到的 F‘

  - 分析

  - 如果无关，说明取掉 A 前和取掉 A 后的 F ==等价==

    - 等价：==1== F 是 F‘*的子集、==2== F’是 F\*的子集

      1. **1 是一定成立的，因此只用证明 2**![image-20221203161142675](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221203161142675.png)

      2. 证明2，将证明子集转换为求$\alpha-A$的闭包是否包含β

         ![image-20221203161409563](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221203161409563.png)

- 如果 A 属于$\beta$，F’ 逻辑蕴含 F

## Normal Forms关系范式

---

### 1NF





### 2NF





### 3NF





### BCNF





## 范式分解

---





















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

