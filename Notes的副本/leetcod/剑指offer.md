# 字符串

## 左旋转字符串



### 思路

1. 提取后半段
2. 提取前半段

### 方法

1. 切片，在 Python 中可以直接切，java 用 substring 方法
2. 列表拼接，java 的 StringBuilder 相当于列表
3. 字符串拼接，若规定只能用 String ，可以直接写字符串

### 简化代码

用求余运算，在一个循环中完成



## 字符串转整数

### 思路

1. 跳过起始空格
2. 检查是否有符号位，如果是负号需要记录
3. 转换整数，注意越界判断和非法判断

### 简化代码

1. trim()方法删除首尾空格

### 坑

1. 考虑空字符串



# 链表

## 从尾到头

### 思路

1. 用栈
2. 两个数组：一个顺序存放，第二次逆序存放
3. 递归



### 关键

1. 链表结束判断
2. java 中栈的写法：
   - LinkedList
   - addLast
   - removeLast
3. 列表的写法：
   - 添加：add
   - 长度：size()
4. 递归：
   1. 又是列表转数组，注意列表的添加是 add
   2. 递归函数的顺序

## 反转链表

### 思路

#### 一个节点时

head->null

变为

Head->null

#### 两个节点时

n1->n2->null

变为

N2->n1->null



#### 双指针法

1. 声明三个节点，cur，pre，tmp
2. cur=head，指向当前节点
3. pre=null，没有前驱节点



1. tmp=cur.next，暂存 n2 节点
2. cur.next=pre，修改当前节点指向
3. pre=cur，前驱节点移动到当前节点
4. cur=tmp，移动到下一节点

## 复杂链表复制

### 思路

#### 普通链表

从头到尾遍历，每次申请一个新的节点，将前一个节点连接上。

```java
class solution{
  public Node copyRandomList(Node head){
    Node cur=null;
    while(head!=null){
      Node pre=new Node(head.val);
      head=head.next;
      cur.next=pre;
    }
    return cur;
  }
}
```

#### 哈希表法

1. 建立哈希表，一次遍历保存原节点到新节点的索引
2. 二次遍历建立 next 和 random 指向

#### 拼接-拆分法





![image-20230719173856188](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230719173856188.png)

![image-20230719173904475](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230719173904475.png)

![image-20230719173924258](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230719173924258.png)

![image-20230719173949451](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230719173949451.png)

![image-20230719174021729](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230719174021729.png)

![image-20230719174104695](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230719174104695.png)

![image-20230719174138294](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230719174138294.png)



![image-20230719174250274](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230719174250274.png)

![image-20230719174257504](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230719174257504.png)