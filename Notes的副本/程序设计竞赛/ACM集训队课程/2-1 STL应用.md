## STL队列

### Operation

1. 创建队列对象：queue<elemtype>队列名;

2. 添加队列元素：队列名.push(元素名);
3. 出队：队列名.pop();
4. 访问队首元素：队列名.front();
5. 访问队尾元素：队列名.back();
6. 查看队列大小：队列名.size();
7. 查看是否为空：队列名.empty();

## 优先队列

### Feature

1. 队尾加入元素
2. 对头删除元素
3. 每次取出的是最高优先权的元素

### Operation

1. 创建队列对象：priority_queue<元素类型> 队列名；
2. 队列添加元素：队列名.push(元素名);
3. 删除第一个元素：name.pop()；
4. 判断是否为空：name.empty()；
5. 返回队列大小：name.size()；
6. 访问最优元素：name.top()；

## 栈Stack

### Feature

1. 先进先出
2. 从栈顶删除元素
3. 从栈顶添加元素 

### Operation

1. 创建栈对象：stack<元素类型> 栈名；
2. 栈顶添加元素：name.push(元素名)；
3. 删除栈顶元素：name.pop()；
4. 访问栈顶元素：name.top();
5. 判断是否为空：name.empty()；
6. 返回栈的大小：name.size()；

### 括号匹配问题

![image-20220218212306717](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220218212306717.png)

**不设置哨兵的风险**

可能会让空栈弹出元素导致错误

## 动态数组Vector

### Operation

1. 定义：vector<int>  a;
2. 初始化：
   1. vector<int>  abc;==初始化size为0的vector==
   2. vector<int>  abc(10);==初始化10个默认值为0的元素==
   3. vector<int>  cde(10,1);==初始化10个值为1的元素==

3. 取向量首元素的迭代器(地址)：name.begin();
4. 取向量尾元素的下一个地址：name.end();
5. 取首元素的值：name.front();
6. 取尾元素的值：name.back();
7. 下标形式访问：int value0 = name[0];==类似于普通的数组访问==
8. 往尾部添加一个元素：name.push_back(value);
9. 删除尾部第一个元素：name.pop_back();
10. 判断是否为空：name.empty();
11. 求向量元素个数：name.size();==实际元素个数（不是容量）==
12. 翻转向量：reverse(a.begin(),a.end());

### 经典应用-邻接表

![image-20220218214043839](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220218214043839.png)

 稀疏图

另一种表示：用下标表示起点，向量元素表示终点

## 集合Set

### 数学上集合的三个特征

1. 确定性（任一元素是否属于某个集合是确定的）
2. 互异性
3. 无序性

### Operation

1. 声明：set<int >  s;
2. 清空： s.clear();
3. 插入元素：s.insert(x);==如果集合中没有则成功插入并自动排序，否则不插入==
4. 查询是否有元素x：int hav=s.count(x);==返回0或1==
5. 查找x并返回迭代器：set<int>::iterator it = s.find(x);
6. 判断是否为空集： s.empty();
7. 求集合元素个数： s.size();
8. 删除元素x：s.erase(x);

#### 最主要用途

自动去重并按升序排序

**只能通过迭代器访问**

![image-20220218220338033](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220218220338033.png)

原因：set为树状结构存储，不是线性存储

<树状数组>

#### 降序排列set

![image-20220219163147749](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220219163147749.png)

#### 如果是结构体set

重载运算符

## string类

### Operation

1. 字符串连接：+/+=
2. 字符串比较：直接用<,<=,==
3. 求子串：substr(l,r)==l,r表示下标==
4. 插入字符串：str.insert(loc,str1)
5. 删除字串：str.erase(loc,length) / str.erase(loc)
6. 交换两个字符串：str.swap(str1)
7. 查找字符串：str.find(key) / str.find(key,loc)==loc以后的字串中查找，找到返回第一次出现的位置==
8. stl算法操作字符串

## 迭代器

遍历容器中数据的对象，能按预先定义的顺序从一个成员移向另一个成员

![image-20220219164742795](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220219164742795.png)

![image-20220219164758944](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220219164758944.png)

## map映射

### Feature

1. 键值对（key/value）容器，迭代器可以修改value，不能修改key
2. map会根据key自动排序
3. 可以理解为关联数组：可以使用key作为下标来获取一个值，关联的本质在于，元素的值与某个特定的键相关联而非通过元素在数组中的位置来获取

### Operation

![image-20220219165441304](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220219165441304.png)

![image-20220219165529252](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220219165529252.png)

iter->first=key

iter->second=value

## 火星文

![image-20220219165946743](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220219165946743.png)

## next_permutaion下一个全排列

如果当前调用排列已经达到最大字典序，则返回false

同理还有prev_permutation

