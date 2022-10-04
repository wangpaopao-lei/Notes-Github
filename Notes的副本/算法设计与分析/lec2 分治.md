适用问题：

1. 问题缩小到一定规模就可以轻松解决
2. 可以分解为若干个规模较小的相同问题（最优子结构）
3. 利用该问题分解出的子问题的解可以合并为该问题的解



特征方程求解递归方程

生成函数解递归方程







Acherman 函数



大整数乘法（FFT）

矩阵乘

归并、快排

## 线性时间选择

![IMG_6609(20221003-134248)](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/IMG_6609(20221003-134248).JPG)

## 平面最近点对

![IMG_6610(20221003-134343)](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/IMG_6610(20221003-134343).JPG)



## 特征方程求解递归方程

### 常系数线性齐次

1.  解特征方程的根，得到递归方程的通解
2. 利用初始条件，确定通解中的待定叙述，得到递归方程的解

#### 两种情况

1. 无重根
   - ![image-20221003135435038](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221003135435038.png)
2. 有重根
   - ![image-20221003135451996](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221003135451996.png)

![image-20221003140029670](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221003140029670.png)

### 常系数线性非齐次

1. 求齐次通解
2. 求非齐次特解

#### 非齐次特解

没有有效方法

考虑两种情况

1. g(n)是 n 的 m 次多项式

   - ![image-20221003140548768](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221003140548768.png)

2. g(n)是 n 的指数函数

   - 求特解时用系数相同来联立方程

   1. ![image-20221003140631597](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221003140631597.png)

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221003140654709.png" alt="image-20221003140654709"  />
