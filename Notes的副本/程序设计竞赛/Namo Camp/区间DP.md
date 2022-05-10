## 石子合并

![image-20220304201104506](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220304201104506.png)

### 直观方法

递归枚举分界线，选择代价最小的

==同一个l到r区间会被重复计算很多次，怎么办==

递归过程中，用数组记录合并l到r堆石子的最小代价，再出现时直接查询

每个区间最多计算一次，共有n^2^个区间，枚举的分界线有n个，总复杂度为n^3^

这就是记忆化搜索

记忆化搜索就是动态规划的原型！

![image-20220304202323571](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220304202323571.png)

### 动态规划考虑

最优子结构：为了计算[l,r]的最小代价，首先计算所有[i,k],[k+1,r]的最小代价

状态：f[i]\[j]表示区间[l,r]的最小代价

转移：$f_{[i][j]}=min_{i\leqslant k<j}(f_{[i][k]}+f_{[k+1][j]})+\sum_{x=i}^j a_x$

==如何保证算一个解之前，已经事先计算了所有子问题的解？==

只需要把区间按照j-i排序，先算区间长度小的，再算区间长度大的

![image-20220304203252518](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220304203252518.png)

## 括号序列

![image-20220304204154843](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220304204154843.png)

状态：f[i]\[j]表示si到sj中最长合法子序列的长度

![image-20220304205402410](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220304205402410.png)

1. 左边不匹配
2. 右边不匹配
3. 左右匹配
4. 从中间连起来

==1,2其实不用考虑==

包含在第4种情况里面

![image-20220304205120290](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220304205120290.png)

