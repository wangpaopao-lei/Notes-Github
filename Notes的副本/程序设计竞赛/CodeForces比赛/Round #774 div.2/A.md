![image-20220306165936188](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220306165936188.png)

### 题目描述

有下标为1到n+1的序列，每一项都有两种取法：1到n-1随机取，或者取n^2^

给定n和sum，计算取n^2^的数的个数

### 分析

1. 假设所有数都取第一种情况，并且开到最大值，都取n-1
2. 有n+1个数，sum为n^2^-1
3. 所以n^2^的个数为$\frac{s}{n^2}$

### 代码

```c++
 /*
YOU WON'T FAIL UNLESS
AND UNTIL YOU STOP TRYING
-->Sarthak Gokhale
*/
#include <algorithm>
#include <cstdio>
#include <cstring>
#include <iostream>
using namespace std;

const int maxn=200000 +5;
int n,a[maxn],ans;
long long s,ss;
void solve();
bool cmp(int x,int y)
{
    return x>y;
}
int main(){
    int t;
    scanf("%d", &t);
    while(t--) solve();   

    return 0;
}

void solve(){
    s=0,ss=0;
    cin>>n;
    for(int i=1;i<=n;i++)
    cin>>a[i];
    sort(a+1,a+n+1,cmp);

    cout<<endl;
    if(n%2)
    {
        for(int i=1;i<=n/2;i++)
        s+=a[i];
        for(int i=n/2+1;i<=n;i++)
        ss+=a[i];
    }
    else{
        for(int i=1;i<=n/2-1;i++)
        s+=a[i];
        for(int i=n/2;i<=n;i++)
        ss+=a[i];
    }
    if(s>ss)
    cout<<"YES"<<endl;
    else cout<<"NO"<<endl;
}
```

