---
title: sum of floors
date: 2024-10-24 14:20:00
tags:
---

#### sum of floors

$$
\sum_{i=1}^{N}\left \lfloor \frac{N}{i} \right\rfloor 
= \sum_{\substack{L=1\\ R=\left\lfloor \frac{N}{\left\lfloor \frac{N}{L} \right\rfloor} \right\rfloor\\L=R+1}}^{N}(R-L+1)\times \left\lfloor \frac{N}{L} \right\rfloor
$$

$$
\sum_{i=1}^{N}f(i)\left\lfloor \frac{N}{i} \right\rfloor=\sum (S_f(R)-S_f(L-1))\times \left\lfloor \frac{N}{i} \right\rfloor \\
S_f是f的前缀和函数
$$

[Enumerate Quotients](https://judge.yosupo.jp/problem/enumerate_quotients)

```cpp
using i64 = long long;
void solve() {
  i64 N;
  std::cin>>N;
  std::vector<i64>a;

  for(i64 L=1,R=1;L<=N;L=R+1){
    R=N/(N/L);
    a.push_back(N/L);
  }
  std::sort(a.begin(),a.end());
  std::cout<<a.size()<<'\n';
  for(size_t i=0;i<a.size();i++){
    std::cout<<a[i]<<" \n"[i+1==a.size()];
  }
}
```

[Sum of Divisors](https://cses.fi/problemset/task/1082)


$$
\sum_{d|N}d \mod 10^{9}+7 \\
=\sum_{d=1}^{N}d \left\lfloor \frac{N}{d} \right\rfloor \mod 10^{9}+7 \\
设f(d)=d \\
S_f(d)=\sum_{i=1}^{d}i=\frac{(d+1)d}{2} \\
answer=\sum (S_f(R)-S_f(L-1))\times \left\lfloor \frac{N}{L} \right\rfloor \mod 10^{9}+7
$$

```cpp
using Z=Mint<i64>;
void solve() {
  i64 N;
  std::cin>>N;
  Z ans=0;

  auto S_f=[&](i64 n){
    return Z((1+n))*Z(n)/Z(2);
  };

  for(i64 L=1,R;L<=N;L=R+1){
    R=N/(N/L);
    ans+=(S_f(R)-S_f(L-1))*Z(N/L);
  }
  std::cout<<ans<<'\n';
}
```

[E. Sum of Remainders](https://codeforces.com/problemset/problem/616/E)

$$
\sum_{i=1}^{m} n \mod i \mod 10^{9}+7 \\
因为:n\mod i=n-\lfloor \frac{n}{i} \rfloor \times i \\
原式=\sum_{i=1}^{m}n-\sum_{i=1}^{m}\lfloor \frac{n}{i} \rfloor \times i \\
=nm-\sum_{i=1}^{m}\lfloor \frac{n}{i} \rfloor \times i \\
若m>n，则i\in (n,m], \lfloor \frac{n}{i} \rfloor =0\\
没有贡献不需要计算
$$

```cpp
using Z=Mint<i64>;
void solve() {
  i64 N,M;
  std::cin>>N>>M;
  Z ans=Z(N)*Z(M);

  Z Z2=Z(1)/Z(2);

  auto S_f=[&](i64 n){
    return Z((1+n))*Z(n)*Z2;
  };
  i64 UP=std::min(N,M);
  for(i64 L=1,R;L<=UP;L=R+1){
    R = std::min(UP, N / (N / L));
    ans -= (S_f(R) - S_f(L - 1)) * Z(N / L);
  }
  std::cout<<ans<<'\n';
}
```

[E - Max/Min](https://atcoder.jp/contests/abc356/tasks/abc356_e)

$$
\sum_{i=1}^{N-1}\sum_{j=i+1}^{N}\left \lfloor \frac{\max(A_i,A_j)}{\min(A_i,A_j)}\right \rfloor \\ 
由于是对所有对(i,j)进行计算 \\
所以重新排列不影响总的结果 \\
不妨升序排序 \\
原式=\sum_{i<j}\left \lfloor \frac{A_j}{A_i}\right \rfloor \\
又由于 1\le A_i\le 10^{6} \\
故可以记录数字的个数,设记录个数数组为cnt \\
不妨固定i\\
当\left \lfloor \frac{A_j}{A_i}\right \rfloor =1时,A_j\in [A_i,2A_i) \\
设\left \lfloor \frac{A_j}{A_i}\right \rfloor=d \\
那么此时A_j\in [dA_i,(d+1)A_i) \\
我们只需要知道在区间[dA_i,(d+1)A_i),有多少个数 \\
区间个数，做前缀和即可 \\
设M=max(A) \\
转化枚举下标i为枚举值x \in[1,M],再枚举x的倍数为dx,保证dx\le M \\
设区间个数数组为S,其为cnt的前缀和数组,采用左闭右开结构\\
设L=dx,R=min((d+1)x,M+1) \\
由于可能出现A_i=A_j,又因为这样的贡献为1 \\
所以需要减去多余的贡献 \\
这样的贡献是\binom{cnt[x]}{2}*1 \\
贡献为\sum_{x=1}^{M}\sum_{d=1}^{dx\le M}(S[R]-S[L])*d*cnt[x]-\frac{cnt[x](cnt[x]+1)}{2} \\
时间复杂度为调和级数\\
$$

```cpp
using i64 = long long;

void solve() {
  int N;std::cin>>N;
  std::vector<int>a(N);
  for(auto&x:a){
    std::cin>>x;
  }
  std::sort(a.begin(),a.end());
  int M=a.back();
  std::vector<i64>cnt(M+1);
  for(auto&x:a){
    cnt[x]++;
  }
  std::vector<i64>S(M+2);
  for(int i=0;i<=M;i++){
    S[i+1]=S[i]+cnt[i];
  }

  auto sum=[&S](int l,int r){
    return S[r]-S[l];
  };

  i64 ans=0;

  for(int x=1;x<=M;x++){
    int y=cnt[x];
    for(int d=1;d*x<=M;d++){
      int L=d*x,R=std::min(M+1,(d+1)*x);
      ans+=sum(L,R)*d*y;
    }
    ans-=1LL*y*(y+1)/2;
  }
  std::cout<<ans<<'\n';
}
```



