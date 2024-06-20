#### 约数系下的zeta变换

$F(n)=\sum_{n|i}f(i),1\le n\le N$  

#### 约数系下的mobius变换

$f(n)=\sum_{n|i}\mu(\frac{i}{n})F(i),1\le n\le N$

N=12时

$F(i)$

- $F(1)=f(1)+f(2)+\ldots f(10)$  
- $F(2)=f(2)+f(4)+f(6)+f(8)+f(10)+f(12)$  
- $F(3)=f(3)+f(6)+f(9)+f(12)$  
- $F(4)=f(4)+f(8)+f(12)$  
- $F(5)=f(5)+f(10)$  
- $F(6)=f(6)+f(12)$  
- $F(7)=f(7)$  
- $F(8)=f(8)$  
- $F(9)=f(9)$  
- $F(10)=f(10)$
- $F(11)=f(11)$  
- $F(12)=f(12)$  

$f(i)$  

- $f(1)=F(1)-F(2)-F(3)-F(5)-F(7)-F(11)+F(6)+F(10)$  
- $f(2)=F(2)-F(4)-F(6)-F(10)+F(12)$  
- $f(3)=F(3)-F(6)-F(9)$  
- $f(4)=F(4)-F(8)-F(12)$  
- $f(5)=F(5)-F(10)$  
- $f(6)=F(6)-F(12)$  
- $f(7)=F(7)$
- $f(8)=F(8)$  
- $f(9)=F(9)$  
- $f(10)=F(10)$  
- $f(11)=F(11)$  
- $f(12)=F(12)$  

zeta变换就是累积和，mobius变换就是差分

#### gcd卷积

$h(n)=\sum_{\gcd(i,j)=n}f(i)g(j)$  


#### 前置反演结论

$$
[n=1]=\epsilon(n)=\sum_{d|n}\mu(d) \\
=\sum_{\substack{d=1\\ d|n}}^{n}\mu(d)=\sum_{d=1}^{n}\mu(d)[d|n] \\
$$

##### example
$$
f=\sum_{i=1}^{N}\sum_{j=1}^{N}[\gcd(i,j)=1]\\
计算这个式子，通常使用上面的反演结论:\\
[gcd(i,j)=1]=\sum_{d|\gcd(i,j)}\mu(d) \\
f=\sum_{i=1}^{N}\sum_{j=1}^{N}\sum_{d|\gcd(i,j)}\mu(d)\\
=\sum_{i=1}^{N}\sum_{j=1}^{N}\sum_{\substack{d|i\\ d|j}}\mu(d)\\
=\sum_{i=1}^{N}\sum_{j=1}^{N}\sum_{d=1}^{N}\mu(d)[d|i][d|j]\\
=\sum_{d=1}^{N}\mu(d)\sum_{i=1}^{N}[d|i]\sum_{j=1}^{N}[d|j]\\
=\sum_{d=1}^{N}\mu(d)\left\lfloor\frac{N}{d}\right\rfloor^{2}\\
下面使用gcd卷积:\\
f=h(1)=\sum_{\gcd(i,j)=1}f(i)f(j)\\
f(i):[1,N]有几个数等于i\\
显然f(i)=1,1\le i\le N\\
$$

```cpp
void solve()
{   
    int n;
    std::cin>>n;
    std::vector<Z> f(n + 1, 1);
    auto h = gcd_conv(f, f);
    Z ans = 0;

    for (int d = 1; d <= n; d++) {
      ans += Z(mobius[d]) * Z((n / d)).pow(2);
    }

    if (ans != h[1]) {
      std::cout << "NO\n";
      std::cout << n << '\n';
      std::cout << ans << ' ' << h[1] << '\n';
      return;
    }
}
```

[P1390 公约数的和](https://www.luogu.com.cn/problem/P1390)

$\sum_{i=1}^{n}\sum_{j=i+1}^{n}\gcd(i,j)$  

首先上面的式子统计了 $1\le i<j\le n$   

这并不好处理

我们知道: $1\le i,j\le n 和 1\le i=j \le n$ 是更好处理的  

前者不考虑 $(i,j)$ 的顺序  

后者只需考虑 $i=j$  

$那么后者 记为(S_y)$:

$\sum_{i=1}^{n}gcd(i,i)$

$=\sum_{i=1}^{n}i$  

$=\frac{(1+n)n}{2}$

$前者 记为(S_x)$:

$$
设f(i)=1,1\le i\le n\\
\sum_{i=1}^{n}\sum_{j=1}^{n}\gcd(i,j)\\
=\sum_{g=1}^{n}\sum_{\gcd(i,j)=g}g\times f(i)f(j)\\
=\sum_{g=1}^{n}g\sum_{\gcd(i,j)=g}f(i)f(j)\\
由\gcd卷积得:\\
上式=\sum_{g=1}^{n}h(g)\times g
$$

答案即为: $\frac{(S_x-S_y)}{2}$

```cpp
void solve()
{   
    int n;
    std::cin>>n;
    std::vector<i64> f(n + 1,1);
       
    auto h = gcd_conv(f, f);

    i64 ans=-1LL*(n+1)*n/2LL;
    for(int i=1;i<=n;i++){
        ans+=1LL*h[i]*i;
    }
    ans/=2;
    std::cout<<ans<<'\n';
}
```

[P3911 最小公倍数之和](https://www.luogu.com.cn/problem/P3911)

$\sum_{i=1}^{N}\sum_{j=1}^{N}lcm(A_i,A_j)$

$$
原式=\sum_{1\le i,j\le N}\frac{A_i\times A_j}{\gcd(A_i,A_j)}\\
=\sum_{g=1}^{N}\sum_{\substack{\gcd(A_i,A_j)=g\\1\le i,j\le N}}\frac{A_i\times A_j}{g}\\
设u=A_i,v=A_j,MX=\max(A) \\
设f(u)=u\times cnt(u),cnt(u):[1,MX]中，u的个数\\
由gcd卷积:\quad h(g)=\sum_{\gcd(u,v)=g}f(u)f(v)\\
原式=\sum_{g=1}^{MX}\frac{h(g)}{g}\\
$$

```cpp
void solve()
{   
    int n;
    std::cin>>n;
    std::vector<int>a(n);
    for(auto&x:a)std::cin>>x;

    int MX=*std::max_element(a.begin(),a.end());

    std::vector<i64>f(MX+1);

    for(auto x:a){
        f[x]++;
    }

    for(int i=0;i<=MX;i++){
        f[i]*=i;
    }
    
    auto h=gcd_conv(f, f);

    i64 ans=0;
    for(int i=1;i<=MX;i++){
        ans+=h[i]/i;
    }
    std::cout<<ans<<'\n';
}
```

#### 练习

[C - LCMs](https://atcoder.jp/contests/agc038/tasks/agc038_c)


#### 模板仅供参考

```cpp
const int N=1e6+1e3;
std::vector<int>prime;
std::vector<bool>is_prime(N+1,true);
std::vector<int>mobius(N+1,1);

void sieve(){
    is_prime[0]=is_prime[1]=false;
    for(int p=2;p<=N;p++){
        if(!is_prime[p])continue;
        prime.push_back(p);
        mobius[p]=-1;
        for(int q=2*p;q<=N;q+=p){
            is_prime[q]=false;
            if((q/p)%p==0)mobius[q]=0;
            else mobius[q]=-mobius[q];
        }
    }
}

template<typename T>
std::vector<T> fast_zeta(std::vector<T>F){
    int n=F.size();
    for(int p=2;p<n;p++){
        if(!is_prime[p])continue;
        for(int k=(n-1)/p;k>=1;k--){
            F[k]+=F[k*p];
        }
    }
    return F;
}

template<typename T>
std::vector<T> fast_mobius(std::vector<T>f){
    int n=f.size();
    for(int p=2;p<n;p++){
        if(!is_prime[p])continue;
        for(int k=1;k*p<n;k++){
            f[k]-=f[k*p];
        }
    }
    return f;
}

template<typename T>
std::vector<T> gcd_conv(const std::vector<T>&f,const std::vector<T>&g){
    int n=std::max(f.size(),g.size());
    auto F=fast_zeta(f);
    auto G=fast_zeta(g);
    std::vector<T>H(n);
    for(int i=1;i<n;i++)H[i]=F[i]*G[i];
    return fast_mobius(H);
}
```

