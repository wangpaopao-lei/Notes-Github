```c++
/*

     YOU WON'T FAIL UNLESS
     AND UNTIL YOU STOP TRYING
            -->Sarthak Gokhale
*/
```

![image-20220221150402909](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220221150402909.png)

### 解读

1. 遍历，对不满足非减序列性质的元素进行操作

2. 满足贪心性质
3. 每次只用维护前数减后数的最大值即可

### problem

1. 如何优化遍历次数
2. 原有顺序如何维护

### Solution

1. 从后往前遍历
2. 不知道

### 后续

![image-20220221163740204](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220221163740204.png)

1. 首先考虑边界，若$a_{n-1}>a_n$，最后两项无法操作，结果必为-1
2. 如果$a_n\geqslant 0$，那么必有解：将1~n-2所有元素替换成$a_{n-1}-a{n}$
3. 否则当且仅当原序列有序时成立

==不一定减出来要是正数==

### 补题代码

```c++
#include <algorithm>
#include <cstdio>
#include <cstring>
#include <iostream>
using namespace std;

const int MAXN=200005;
int p[MAXN],f[MAXN],n,c;

void solve();

int main(){
    int t;
    scanf("%d", &t);
    while(t--) solve();   

    return 0;
}

void solve(){
    cin>>n;
    c=0;
    memset(p,0,sizeof(p));
    memset(f,0,sizeof(f));
    for(int i=1;i<=n;i++)   cin>>p[i];
    if(p[n-1]>p[n])
    {
        cout<<-1<<endl;
        return ;
    }
    if(p[n]>=0)
    {
        for(int i=1;i<=n-2;i++)
        {
            if(p[i]!=p[n-1]-p[n])
            {
                f[i]=1;
                c++;
            }
        }
        cout<<c<<endl;
        for(int i=1;i<=n-2;i++)
        {
            if(f[i]==1)
            cout<<i<<" "<<n-1<<" "<<n<<endl;
        }
    }
    else {
        for(int i=1;i<n;i++)
        {
            if(p[i]>p[i+1])
            {
                cout<<-1<<endl;
                return;
            }
        }
        cout<<0<<endl;
    }

}
```

