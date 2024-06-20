在长度为 $N的数列，a_1,a_2,\ldots,a_n 中$  

- 满足 **条件** 的 **最短** 连续子序列
- 满足 **条件** 的 **最长** 连续子序列
- 满足 **条件** 的 连续子序列的 **个数**  

[The Number of Windows](https://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=DSL_3_C)

求得满足条件: $\sum_{i=l}^{r}a_i \le X 的区间[l,r] 个数$  

$尝试固定左端点 l,然后移动右端点 r$  

$当\ r\ 移动到第一个不满足条件的下标时，也就是区间加和 > X时$

$发现这些区间\ [l,i]\ i \in [l,r)一定是符合要求的$  

$这些区间一共有: r-l 个$

$每次固定左端点l,然后找到第一个不符合条件的右端点r,是经典的二分搜索，O(N\log N)$  

$但是可以发现逐渐移动左端点l,右端点r只会向后移动$  

$l,r都只会移动 N次,所以这是一个O(N)算法$


```cpp
#include <bits/stdc++.h>
using i64 = long long;
void solve()
{
    int N,Q;
    std::cin>>N>>Q;
    std::vector<i64>a(N);
    for(auto&x:a)std::cin>>x;

    while(Q--){
        i64 x;std::cin>>x;
        i64 ans=0;
        i64 sum=0;
        for(int i=0,j=0;i<N;i++){
            while(j<N and sum+a[j]<=x){
                sum+=a[j++];
            }
            ans+=j-i;
            if(i==j)j++;
            else sum-=a[i];
        }
        std::cout<<ans<<'\n';
    }
}
int main()
{
    std::cin.tie(nullptr)->sync_with_stdio(false);
    solve();
    return 0;
}
```

[Subsequence](https://vjudge.net/problem/POJ-3061)

$求得满足:\sum_{i=l}^{r}a_i \ge S,\min({r-l+1})$  

$固定左端点l,移动右端点r,当sum \ge S时,r是第一个不符合条件的端点$  

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
// using i64 = long long;
typedef i64 long long;
void solve()
{
    int N;i64 S;
    std::cin>>N>>S;
    std::vector<i64>a(N);
    for(int i=0;i<N;i++)std::cin>>a[i];
    int ans=N+1;
    i64 sum=0;
    for(int i=0,j=0;i<N;i++){
        while(j<N and sum<S){
            sum+=a[j++];
        }
        if(sum<S)break;
        ans=std::min(ans,j-i);
        if(i==j)j++;
        else{
            sum-=a[i];
        }
    }
    std::cout<<(ans==N+1?0:ans)<<'\n';
}
int main()
{
    // std::cin.tie(nullptr)->sync_with_stdio(false);
    int t;
    std::cin >> t;
    while (t--)
    {
        solve();
    }
    return 0;
}

```

[C - 列](https://atcoder.jp/contests/abc032/tasks/abc032_c)

$求得满足:\ \prod_{i=l}^{r}s_i \le K,\max({r-l+1})$  

由于是求最长满足条件序列长度，那么如果数组中包含0，那么答案即为序列长度 $N$  

$否则，当mul > K 时,右端点r固定$  

```cpp
#include <bits/stdc++.h>
using i64 = long long;
void solve()
{
  int N,K;
  std::cin>>N>>K;
  std::vector<i64>a;
  bool zero=false;
  for(int i=0;i<N;i++){
    i64 x;std::cin>>x;
    if(x==0)zero=true;
    else a.push_back(x);
  }
  if(zero){
    std::cout<<N<<'\n';
    return;
  }

  int ans=0;
  i64 mul=1;
  for(int i=0,j=0;i<N;i++){
    while (j < N and (mul * a[j] <= K)) {
      mul*=a[j++];
    }
    ans=std::max(ans,j-i);
    if(i==j)j++;
    else{
      mul /= a[i];
    }
  }
  std::cout<<ans<<'\n';
}
int main()
{
    std::cin.tie(nullptr)->sync_with_stdio(false);
    solve();
}
```


[C - 単調増加](https://atcoder.jp/contests/abc038/tasks/abc038_c)

