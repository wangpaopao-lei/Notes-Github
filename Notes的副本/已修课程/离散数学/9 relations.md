### 9.1 Relations and their Properties关系及关系性质
Concept:Let a and B be sets.A binary relation from A to B is a subset of $A\times B$
用有序对来表达两个集合的元素之间的关系，这个集合就叫二元关系。

Properties:
1. *自反*:A relation R on A is called **reflexive** if $(a,a)\in R$ for every element $a\in R$
2. *对称和反对称*:A relation R on a set A is called **symmetric** if $(b,a)\in R$ whenever $(a,b)\in R$ ,for all $a,b\in A$ ,if $(a,b)\in R$ and $(b,a)\in R$ ,then a=b is called **antisymmetric**
3. *传递性*:A relation R is called **transitive** if whenever $(a,b)\in R$ and $(b,c)\in R$, for all $a,b,c\in A$.

Combining:*关系的合成* Let R br a relation from a set A to a set B and S a relation from a set B to a set C. The **composite** of R and S is the relation consisting of ordered pairs (a,c), where $a\in A, c\in C$, and for which there exists an element $b\in B$ such that $(a,b)\in R and (b,c)\in S$. We denote the composite of R and S by $R\circ S$.

Powers of R:*关系R的幂* :(recursively defined)Let R be a relation on the set A. The powers $R^n$, are recursively defined by $R^1=R and R^{n+1}=R^n\circ R$.

### 9.2 n-ary Relations and their Applications n元关系及应用
Concept:*n元关系和它的域和阶* Let $A_1,A_2,\dots,A_n$ be sets. An **n-ary relation** on the sets is a subset of $A_1\times A_2\times A_3\dots A_n$. The sets are called **domains** of the relation, and n is called its **degree**.

Operations:
1. *选择运算*:Let R be an n-ary relation and C a condition that elements in R may satisfy. Then the **selection operator** $S_c$ maps the n-ary relation R to the n-ary relation of all n-tuples from R that satisfy the condition C.
2. *投影*:The projection $P_{i_1,i_2,\dots ,i_m}$, maps the n-tuple $(a_1,a_2,\dots ,a_n)$ to the m-tuple $(a_{i_1},a_{i_2},\dots ,a_{i_m}$ where $m\leqslant n$.
   删除了n元组的n-m个分量

### 9.3 Representing Relations关系的表示
1. Matrix
   关系R可以用矩阵$M_R=[m_{ij}]$表示：
   $m_{ij}=1,(a_i,b_j)\in R$
   $m_{ij}=0,(a_i,b_j)\notin R$
   * 定义在一个集合上的关系的矩阵是一个方阵
   * 自反关系的矩阵主对角线上的元素全为1
   * 对称关系的矩阵与其转置矩阵相同
   * 关系合成的矩阵可以通过关系矩阵的布尔积得到，可以推广到布尔幂用来表示关系$R^n$
1. Digraphs(有向图)
   * 自反：每个顶点都有环
   * 对称：每一条边都存在一条相反的边，对称关系可用无向图表示
   * 反对称：两个顶点间不存在相反的两条边。
   
   
### 9.4 Closure of Relations关系闭包
Concept: 
* For any property X, the "X closure" of a set A is defined as ***the smallest superset*** of A that has the given property.
* The **transitive closure** or **connectively relation** of R is obtained by repeatedly adding (a,c) to R for each (a,b),(b,c) in R.
* A path of length n from node a to b in the directed graph G(or the binary relation R) is a sequence 
  {$(a,x_1),(x_2,x_3),\dots ,(x_{n-1},b)$}

Theorem 1:设R是集合A上的关系，从a到b存在一条长为n的路径当且仅当$(a,b)\in R^n$
> 用数学归纳法证明
* Connectivity Relation*连通关系*：设R是集合A上的关系，从a到b之间存在一条长度至少为1的路径（满足任意两个元素都能通过关系复合到达）
> 表示为$R^*$
> 因为$R^n$由有序对(a,b)构成，使得存在一条长度为n的路径，所以$R^*$是所有$R^n$的并集

For transitive closure: 用有向图表示路径有助于构建传递闭包
Theorem 2:关系R的传递闭包等于连通性关系$R^*$

**Warshall算法**==求传递闭包==

[Warshall算法详解](https://www.pluvet.com/2020/12/27/%E6%B1%82%E4%BC%A0%E9%80%92%E9%97%AD%E5%8C%85%E7%9A%84-warshall-%E7%AE%97%E6%B3%95%E8%AF%A6%E8%A7%A3/)

### 9.5 Equivalence Relation等价关系
同时满足自反，对称，传递的关系。
Equivalence classes *等价类* :如果R是A上的一个等价关系，A中有一个元素为a，称与a有关系的所有元素的集合R(a)为R的等价类。
商集：R关于A的所有等价类的集合。
### 9.6 Partial Orderings偏序关系
同时满足自反，反对称，传递的关系。
Poset*偏序集*：A set A together with a partial order R on A is called a poset and is denoted (A,R).
