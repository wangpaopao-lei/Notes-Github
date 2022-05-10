![image-20220306184844197](C:\Users\Wang Lei\AppData\Roaming\Typora\typora-user-images\image-20220306184844197.png)

### 题目描述

1. 定义两种powerful numbers：2的n次幂，某个数的阶乘
2. 给定n，在不重复使用powerful numbers的情况下，判断能否拆分为powerful numbers数之和
3. 可以的话输出最少的powerful numbers使用数，不能输出-1



### 分析

1. 假设只有2的n次幂这一种powerful numbers，则可以把原数用二进制表示，为1的位数即是幂相加
2. 加入阶乘数后，穷举3到14的所有阶乘数是否加入，每种情况下将余下的 数表示为二进制
3. 这个过程中取最少的表示

==2的n次幂其实是可以重复的==

证明当2的n次幂有重复时，都不是最优解

### 代码

```c++
#include <bits/stdc++.h>
using namespace std;

const long long MAXAI = 1000000000000ll;
// 最低位1
int get_first_bit(long long n){
	return 63 - __builtin_clzll(n);
}
// 二进制表示后的1的个数
int get_bit_count(long long n){
	return __builtin_popcountll(n);
}

int main(){
	ios::sync_with_stdio(0);cin.tie(0);cout.tie(0);//重载io
	int t; cin >> t;
	for(int test_number = 0; test_number < t; test_number++){
		long long n; cin >> n;
		//Computing factorials <= MAXAI
		vector<long long> fact;
		long long factorial = 6, number = 4;
		while(factorial <= MAXAI){
			fact.push_back(factorial);
			factorial *= number;
			number++;
		}
		//Computing masks of factorials
		vector<pair<long long, long long>> fact_sum(1 << fact.size());
		fact_sum[0] = {0, 0};
		for(int mask = 1; mask < (1 << fact.size()); mask++){
			auto first_bit = get_first_bit(mask);
			fact_sum[mask].first = fact_sum[mask ^ (1 << first_bit)].first + fact[first_bit];
			fact_sum[mask].second = get_bit_count(mask);
		}
		long long res = get_bit_count(n);
		for(auto i : fact_sum){
			if(i.first <= n){
				res = min(res, i.second + get_bit_count(n - i.first));
			}
		}
		cout << res << "\n";
	}
	
	return 0;
}
```

