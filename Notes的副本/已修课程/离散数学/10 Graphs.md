##  10 Graph

### 10.1 Introduction of Graph图的概念
**Definition**: 

1. A Graph G=(V,E) consists of V ,nonempty set of **vertices** （顶点）and E, a set of **edges**（边）. An edge is said to connect its endpoints.
   通常只考虑有限图。
2. 多重图A multigraph G=(V,E,f) consists of a set V of vertices, a set of E of edges (as primitive objects), and a function
   $f:E\rightarrow\{\{u,v\}|u,v\in V \and u\neq v\}$
3. 区别于简单图（两点之间不能有多条边）
4. 伪图：类似于多重图，但是可以包含环。

### 10.2 Graph Terminology图的术语

**Definition**:

1. 邻接Adjacency
2. 邻居Neighborhood:The **set** of all neighbors of a vertex v of G=(V,E), denoted by N(v), is called the neighborhood of v. If A is a subset of V, we denote by N(A) the set of all vertices in G that are adjacent to at least one vertex in A. So, $N(A)=$(A中所有顶点的并集）.
3. 度Degree of a Vertex:deg(v), is its number of incident edges*与其连接的边的个数*

   * A vertex with degree 0 is called **isolated**.

   * A vertex of degree 1 is called **pendant**悬挂点.
4. complete graph $K_n$完全图
5. cycles $C_n$环图
6. wheelers $W_n$轮图
7. Bipartite graph $K_{n,m}$二分图
8. Matching匹配是边集的子集
9. Maximum Matching最大匹配：含有边的数量最多的匹配
10. Complete Matching完全匹配：

**Theorem**:

1. 握手定理：所有顶点的度的和是边数的两倍（即使出现多重边和环也成立）
2. 无向图有偶数个度为奇数的顶点
3. 一个简单图是二分图，当且仅当能够对图中的每个顶点赋予两种颜色，并使得没有两个相邻的顶点被赋予相同的颜色
4. 霍尔婚姻定理：带有二部划分$(V_1,V_2)$的二分图G=(V,E)中有一个从$V_1$到$V_2$的完全匹配当且仅当对于$V_1$所有的子集A，都有$|N(A)|\geqslant|A|$。

### 10.3 Graph Representations&Isomorphism图的表示和同构

**Definition**:

1. 邻接矩阵、邻接表：行和列都为全部点集，表示在$v_i,v_j$之间边的条数。
2. 关联矩阵：行坐标为点集，列坐标为边集。若表示有向图，1表示该顶点为边的起点，-1表示终点。
3. 同构Isomorphism：对于简单图$G_1=(V_1,E_1$)和$G_2=(V_2,E_2$)，若点集存在一对一和映上的函数$f$且其具有性质：对$V_1$中所有的a和b来说，a和b在$G_1$中相邻当且仅当$f(a)和f(b)$在$G_2$中相邻，称这两个图同构。

**同构判定算法**：

1. 计算邻接矩阵
2. 计算行次和列次：矩阵每行/每列1的个数（对应于入次和出次）
3. 不考虑出现的次序，若行次和列次不同，则不同构，否则继续
4. 同时交换其中一矩阵的i行和j行，i列和j列。
5. 看能不能和另一个矩阵相同

### 10.4 Connectivity连通性

·

**Definition**:

1. 通路path：边序列，当图是简单图时，可以用顶点序列表示
2. 回路circuit：通路在相同的顶点开始和结束且长度大于1
3. 简单通路simple path：没有重复的边
4. 有向图中path must go in the direction of arrow
5. 连通性Connectedness：若无向图中的每一对不同的顶点之间都有通路，则称该图是连通的
6. 连通分支Connected component：连通子图，且是极大连通子图。
   * 不连通的图G具有2个或2个以上的不相交的连通子图，并且G是这些子图的并
7. 割点（关节点）Cut vertices(artiulation points)：删除图中的一个顶点和它关联的边，就产生比原图具有更多连通分支的子图
8. 割边（桥）Cut edge(bridge)：同理
9. 不可分割图nonseperable graph：不含割点的连通图（它通常具有比有割点的连通图更好的连通性）
10. 点割集vertex cut：G-V'是不连通的，则称V的子集V'为点割集
11. 点连通度k(G)：非完全图的点连通度为点割集中最小的顶点数

> 当G是完全图时不能这样定义，因为删除任意点集的子集后仍是完全图，要定义为n-1（删到只剩一个点）
>
> k(G)越大，图的连通性越好
>
> $k(G)\geqslant k$，我们称图是k-connected.

12. 边连通度$\lambda(G)$：把连通图变为不连通的所需要删除的最小边数，同理有边割集
13. 强连通性strongly connected：对于有向图中任意顶点a和b，都有从a到b和从b到a的通路
14. 弱连通性weakly connected：在有向图的基本无向图中，任何两个顶点之间都有通路
15. 强连通分支：有向图G的子图是强连通的，但不包含在更大的强连通子图中，即极大强连通子图，可称为G的**强连通分支**

**Theorem**:

1. 在连通无向图的每一对不同顶点之间都存在简单通路
2. $k(G)\leqslant \lambda(G)\leqslant min\ deg(v)$
3. 一个图两个顶点之间通路的数目：设G是一个图，邻接矩阵A。从$v_i$到$v_j$长度为人、的不同通路的数目等于$A^r$的第(i,j)项。

![image-20211207152454876](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20211207152454876.png)

### 10.5 Euler and Hamilton Path欧拉通路与哈密顿通路

**Definition:**

1. 欧拉回路Euler circuit：包含图中每一条边的简单回路，具有欧拉回路的图被称为欧拉图Eulerian graph
2. 欧拉通路Euler Path：包含图中每一条边的简单通路
3. 哈密顿通路Hamilton path：经过图中每个顶点恰好一次的简单通路称为哈密顿通路
4. 哈密顿回路Hamilton tour/circuit：经过图中每个顶点恰好一次的简单回路称为哈密顿回路

**Theorem:**

1. 含有至少2个顶点的连通多重图具有欧拉回路当且仅当每个顶点的度都为偶数
2. 连通多重图具有欧拉通路但没有欧拉回路当且仅当它恰有两个度为奇数的顶点
3. 狄拉克定理Dirac's theorem：如果G是有n个顶点的简单图，其中n大于等于3，并且G中每个顶点的度都至少为二分之n，则G有哈密顿回路
4. 欧尔定理Ore's corollary：如果G是有n个顶点的简单图，其中n大于等于3，并且对于G中每一对不相邻的顶点u和v来说，都有$deg(u)+deg(v)\geqslant n$，则G有哈密顿回路

![image-20211213182044717](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20211213182044717.png)

### 10.6 Shortest Path Problem最短通路问题

狄克斯特拉算法

详情见[Dijkstra 寻路算法的工作原理](https://www.pluvet.com/2020/12/07/dijkstra-%E5%AF%BB%E8%B7%AF%E7%AE%97%E6%B3%95%E7%9A%84%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86/)

### 10.7 Planar Graphs平面图

**Definition:**

1. 平面图：可以在平面中画出而没有任何边交叉
2. 初等细分elementary subdivision：删除一条边{u,v}并且添加一个新顶点和两条边{u,w}和{w,v}，获得的图也是平面图
3. 同胚Homeomorphic：经过一系列初等细分产生的图和原图

**Theorem:**

1. 欧拉公式：设G是带e条边v个顶点的连通平面简单图，r是G的平面图表示的面数，则$r=e-v+2$

2. 推论：$v\geqslant3$，则$e\leqslant3v-6$
3. 推论：若G是连通平面简单图，则G中有度数不超过5的顶点
4. 库拉图斯基定理：一个图是平面图当且仅当它不包含同胚于$K_{3,3}或K_5$的子图

![image-20211213185505378](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20211213185505378.png)

### 10.8 Graph Coloring图的着色

**Definition:**

1. 简单图的着色是对每个顶点指定一种颜色使得没有两个相邻顶点颜色相同
2. 图的着色数是着色这个图所需要的最少颜色数，记为$\chi(G)$
3. ==子图和商图==

**Theorem:**

1. 四色定理：平面图的着色数不超过4

2. 五色定理：任何平面图都可以被五种颜色着色

![image-20211213195633594](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20211213195633594.png)

### 10.9/8.4 Transport Networks传输网流量问题

