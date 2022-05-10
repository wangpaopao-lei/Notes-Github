无负权边最短路

dist(u)，表示当前求出的从 S到 u 的最短路径的长度

维护顶点集 C，满足 C 中所有的顶点 x，都已经找到了 S 到 x 的最短路，此时 dist（x）就是最终最短路径的长度

1. C 设置为空，S 设为 0，其与顶点为无穷大
2. 每一轮中，将离起点最近的还不在 C 中的顶点加入 C，用这个点连出去的边通过松弛操作尝试更新其他点的 list
3. 当 T 或者没有新的点加入 C 时结束

![image-20220422194712898](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220422194712898.png)

优化

花费大量时间在找到最小 dist 上

**用堆（优先队列）来维护最小值**

![image-20220422200211137](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220422200211137.png)