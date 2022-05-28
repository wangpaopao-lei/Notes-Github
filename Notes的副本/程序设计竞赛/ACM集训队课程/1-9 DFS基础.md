## 二叉树的遍历

递归实现

## n个数的全排列

![image-20220125220124450](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220125220124450.png)

![image-20220125222743869](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220125222743869.png)

## 迷宫

迷宫搜索

BFS用来求最短，但是不符合此题要求

### 奇偶性剪枝

把地图当做01相间的，步数的奇偶性即可一次看出

![image-20220126130316900](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220126130316900.png)

![image-20220126130534679](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220126130534679.png)

```C
void dfs(int step)
{
    if(step==n+1)
    {
        for(int i=1;i<=n;i++)
        {
           printf("%d ",num[i]);
        }
        printf("\n");
        return;
    }
    for(int i=1;i<=n;i++)
    {
        if(vis[i]==0)
        {
            num[step]=i;
            vis[i]=1;
            dfs(step+1);
            vis[i]=0;
        }
    }
}
```

