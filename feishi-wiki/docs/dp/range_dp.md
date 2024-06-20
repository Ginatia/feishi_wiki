[N - Slimes](https://atcoder.jp/contests/dp/tasks/dp_n)

$N 个数，每次合并相邻的两个数$  

$合并之后的数为两数之和x+y，费用为x+y$

$问将所有数合并之后的最小费用$  

$如果现在处理的是区间[l,r]$

$枚举分割点m，通过分割点m所得到的最小花费为:$

$f(l,m)+f(m+1,r)+\sum_{i=l}^{r}a_i$

```cpp
#include <bits/stdc++.h>
using i64 = long long;
void solve()
{
    int N;std::cin>>N;
    
    std::vector<i64>a(N+1);
    for(int i=1;i<=N;i++)std::cin>>a[i];

    const i64 INF=1e18;
    std::vector dp(N+1,std::vector<i64>(N+1,INF));
    std::vector vis(N+1,std::vector<bool>(N+1,false));

    std::vector<i64>pre(N+1);
    for(int i=1;i<=N;i++)pre[i]=pre[i-1]+a[i];

    auto dfs = [&](auto self, int L, int R) -> i64 {
        if (L == R)
            return 0;
        if(vis[L][R])
            return dp[L][R];
        vis[L][R]=true;

        i64 ans=INF;
        for(int i=L;i<R;i++){
            ans=std::min({ans,self(self,L,i)+self(self,i+1,R)});
        }
        i64 sum=pre[R]-pre[L-1];
        return dp[L][R]=ans+sum;
    };

    std::cout<<dfs(dfs,1,N)<<'\n';
}
int main()
{
    std::cin.tie(nullptr)->sync_with_stdio(false);
    solve();
    return 0;
}

```

[1039. 多边形三角剖分的最低得分](https://leetcode.cn/problems/minimum-score-triangulation-of-polygon/description/)

你有一个凸的 n 边形，其每个顶点都有一个整数值。给定一个整数数组 values ，其中 $values[i]$ 是第 i 个顶点的值（即 顺时针顺序 ）。

假设将多边形 剖分 为 n - 2 个三角形。对于每个三角形，该三角形的值是顶点标记的乘积，三角剖分的分数是进行三角剖分后所有 n - 2 个三角形的值之和。

返回 多边形进行三角剖分后可以得到的最低分

$如果已经确定两个三角形的两个端点,那么枚举第三个端点即可得到一个三角形$  

$dp(i,j):从 i 到j的多边形的最低得分$

$dp(i,j)=\max_{k=i+1}^{j-1}(dp(i,k)+dp(k,j)+v(i)*v(j)*v(k))$

```cpp
class Solution {
public:
    int minScoreTriangulation(vector<int>& values) {
        int n=values.size();
        std::vector dp(n+1,std::vector<int>(n+1,0));
        std::vector vis(n+1,std::vector<bool>(n+1,false));
        const int INF=1e9;
        auto dfs=[&](auto self,int i,int j)->int{
            if(i+1==j){
                return 0;
            }
            if(vis[i][j])return dp[i][j];
            vis[i][j]=true;
            int ans=INF;
            for(int k=i+1;k<j;k++){
                ans=std::min(ans,self(self,i,k)+self(self,k,j)+values[i]*values[j]*values[k]);
            }
            return dp[i][j]=ans;
        };

        return dfs(dfs,0,n-1);
    }
};
```






