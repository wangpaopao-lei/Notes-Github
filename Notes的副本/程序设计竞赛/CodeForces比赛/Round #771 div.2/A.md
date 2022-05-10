```c++
/*

     YOU WON'T FAIL UNLESS
     AND UNTIL YOU STOP TRYING
            -->Sarthak Gokhale
*/
```

![image-20220215114010908](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220215114010908.png)

### 解读

对序列进行一次reverse操作，使操作后序列字典序最小

#### 我的代码（未完成）

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
int a[502],b[502],c[502];
bool cmp(int*x,int *y)
{
    for(int i=1;i<501;i++)
    {
        if(x[i]>y[i])
        return 0;
    }
    return 1;
}
void reverse(int*x,int l,int r)
{
    for(int i=l;i<(l+r)/2;i++)
    {
        swap(x[i],x[r-i1]);
    }
}
int main()
{
    int t,n;
    cin>>t;
    memset(a,0,sizeof(a));
    memset(b,0,sizeof(b));
    memset(c,0,sizeof(c));
    while(t--)
    {
        scanf("%d", &n);
        for(int i=1;i<=n;i++)
        {
            scanf("%d", &a[i]);
            b[i]=a[i];
            c[i]=a[i];
        }
        for(int i=1;i<n;i++)
        {
            for(int j=i;j<=n;j++)
            {
                reverse(b,i,j);
                if(cmp(b,c))
                {
                    for(int k=1;k<=n;k++)
                    c[k]=b[k];
                }
                for(int k=1;k<=n;k++)
                    b[k]=a[k];
            }
        }
        for(int i=1;i<n;i++)
        printf("%d ",c[i]);
        printf("%d\n",c[n]);
    }
}
```

#### 方法

暴力枚举

#### Problems

1. reverse函数功能未实现
2. 题目理解出现偏差，给定序列理想状态为$p_i=i$而非任意序列，因此只需要找到第一个不符合字典序的元素进行操作

#### Solutions

```c++
void reverse()
```



### 后续

#### Hint 1

==*When is it best for the permutation to remain unchanged?*==

序列为1,2,3，……

#### Hint 2

*==What elements are obviously optimal and should remain as they are?==*

$p_i=i$

#### Hint 3

*==Find the first non-optimal element. What's the best way to fix it?==*

$p_i\ne i$,$p_k=i$

### 补题代码

```c++

```

