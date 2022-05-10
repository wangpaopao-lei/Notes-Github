![image-20220306183637571](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220306183637571.png)

### 题目描述

给定长度为n的序列，每个元素可以被涂成蓝色或者红色，判断是否能在元素个数红色小于蓝色的情况下，使得红色元素之和大于蓝色元素之和

### 分析

==赛时分析==

1. 使用贪心算法，首先对序列从大到小排序
2. 若n为偶数，红色为前n/2-1个，其余为蓝色
3. 若n为奇数，红色为前n/2个，其余为蓝色
4. 比较两个sum的大小

==赛后分析==

当n为偶数时，前n/2-1个涂成红色没问题，但是蓝色只能涂后n/2个，这样才能满足红色刚好比蓝色多一个。

贪心都贪不明白！！！

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
        for(int i=n/2+1;i<=n;i++)
        ss+=a[i];
    }
    if(s>ss)
    cout<<"YES"<<endl;
    else cout<<"NO"<<endl;
}
```

