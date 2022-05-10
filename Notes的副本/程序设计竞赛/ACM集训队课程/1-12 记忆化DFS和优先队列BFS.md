## 记忆化DFS

### 斐波那契数列

![image-20220125215551482](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220125215551482.png)

效率接近动态规划，写法为递归

 先保存在数组里再返回避免重复计算

### 01背包

#### 常规解法

![image-20220208133038761](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220208133038761.png)

#### 记忆化DFS解法

![image-20220208133104320](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220208133104320.png)

### 老鼠和奶酪

![image-20220218151358888](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220218151358888.png)

ans[x]\[y]表示(x,y)出发的最优解

dfs表示当前位置出发的最优解，dp只能表示从头到尾的，记忆化dfs即将两者的优点结合，既 

### How Many Ways

有向无环图

![image-20220218153230790](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220218153230790.png)

## 基于优先队列的BFS

### 优先队列(priority_queue)

![image-20220218153404251](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220218153404251.png)

priority_queue<elemType,container(vector...),greater<int> > queueName;

重载运算符实现优先

![image-20220218202718908](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220218202718908.png)

### 哈夫曼编码

1. 找出权值最小的元素，构成二叉树，删掉这两元素，合并产生新的元素
2. 重复步骤1

![image-20220218203331235](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220218203331235.png)

