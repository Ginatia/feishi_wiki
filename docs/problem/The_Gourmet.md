[The Gourmet](https://open.kattis.com/problems/gourmeten)

一共有 $M$ 分钟，$N$ 个菜，第 $i$ 道菜需要吃 $T_i$ 分钟  

求出刚好吃 $M$ 分钟的吃菜方案，区分顺序  

完全背包+区别顺序  

既然需要区别顺序，那么由背包从小到大更新方案数即可  

反之不区分顺序则由物品顺序更新

也就是区别先枚举容量还是先枚举物品  

```cpp
using i64 = long long;
void solve() {
  int M,N;
  std::cin>>M>>N;
  std::vector<int>t(N);
  for(auto&x:t){
    std::cin>>x;
  }
  std::vector<int>dp(M+1,0);
  dp[0]=1;
  for(int i=0;i<=M;i++){
    for(int j=0;j<N;j++){
      int k=i-t[j];
      if(k>=0 && dp[k]){
        dp[i]+=dp[k];
      }
    }
  }
  std::cout<<dp[M]<<'\n';
}
```

