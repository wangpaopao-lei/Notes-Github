## STL队列

1. 创建队列对象：queue<elemtype>队列名;

2. 添加队列元素：队列名.push(元素名);
3. 出队：队列名.pop();
4. 访问队首元素：队列名.front();
5. 访问队尾元素：队列名.back();
6. 查看队列大小：队列名.size();
7. 查看是否为空：队列名.empty();

## 二叉树层序遍历

![image-20220125114518638](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220125114518638.png)

## BFS

与二叉树的层序遍历类似

解决后续顶点可能出现重复问题：创建标记数组

![image-20220125215112083](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220125215112083.png)

## 奇怪的电梯

可以表示为树状结构

搜索的本质是暴力枚举，前提是能把状态表示出来，且能规定状态转移的规则

![image-20220125212544401](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220125212544401.png)

![image-20220125212838747](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220125212838747.png)

## 非常可乐

隐式图

![image-20220125214446746](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220125214446746.png)

## Knight Moves

把位移的变化保存到一个数组里，一层循环即可达到八个状态

状态定义为三个变量，横坐标、纵坐标、步数