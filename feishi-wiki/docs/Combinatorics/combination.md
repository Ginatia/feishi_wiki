---
title: combination
date: 2024-05-23 20:20:26
tags:
---

### 将N球放入X盒子

| 区分球 | 区分盒子 | 没有限制                            | 至多1个    | 至少1个                                        |
| ------ | -------- | ----------------------------------- | ---------- | ---------------------------------------------- |
| 1      | 1        | $x^{N}$                             | $P(N,X)$     | $X!S(N,X)=\sum_{i=0}^{X}(-1)^{X-i}C(X,i)i^{N}$ |
| 0      | 1        | $H(N,X)=C(N+X-1,N)$                 | $C(N,X)$   | $H(N-X,X)=C(N-1,N-X)=C(N-1,X-1)$               |
| 1      | 0        | $B(N,X)=\sum_{i=0}^{X}S(N,i)$       | $[N\le X]$ | $S(N,X)$                                       |
| 0      | 0        | $p_{\le X}(N)=\sum_{i=0}^{X}p_i(N)$ | [$N\le X$] | $p_{X}(N)\ or\  p_{\le X}(N-X)$                |

#### 区分球，区分盒子
**没有限制:**

[例题](https://onlinejudge.u-aizu.ac.jp/courses/library/7/DPL/5/DPL_5_A)

那么每个球有 $X$ 个选择，选择 $N$ 次

由乘法原理，答案为： $X^{N}$


**每个盒子至多容纳1个球:**

[例题](https://onlinejudge.u-aizu.ac.jp/courses/library/7/DPL/5/DPL_5_B)

第一个球有 $X$ 选择，第二个球有 $X-1$ 选择， $\dots$ , 第 $N$ 个球有 $X-N+1$ 个选择

由乘法原理，答案为: $X(X-1)(X-2)\dots(X-N+1)=P(N,X)$ , 计算排列 $P(N,X)即可$




**每个盒子至少容纳1个球**

[例题](https://onlinejudge.u-aizu.ac.jp/courses/library/7/DPL/5/DPL_5_C)

将一个有 $N$ 物品的集合分为 $X$ 个非空子集 的方法数 称之为第二类斯特林数 $S(N,X)$ 

由于 $X$ 盒子相互区分，所以答案为： $X!S(N,X)$

$S(N,X)=\frac{1}{X!}\sum_{i=0}^{X}(-1)^{X-i}C(X,i)i^{N}$

$X!S(N,X)=\sum_{i=0}^{X}(-1)^{X-i}C(X,i)i^{N}$

还可以通过动态规划计算:

$S(N,X)=XS(N-1,X)+S(N-1,X-1)$

#### 不区分球，区分盒子

[例题](https://onlinejudge.u-aizu.ac.jp/courses/library/7/DPL/5/DPL_5_D)

**没有限制**

$N$ 球选出 $X$ 个盒子放进去，允许盒子重复选择的方法数称之为 重叠组合 $H(N,X)=C(N+X-1,N)=C(N+X-1,X-1)$

**至多一个**

[例题](https://onlinejudge.u-aizu.ac.jp/courses/library/7/DPL/5/DPL_5_E)

$N$ 球选出 $X$ 个盒子放进去，不允许盒子重复选择，即为组合数，$C(N,X)$

**至少一个**

[例题](https://onlinejudge.u-aizu.ac.jp/courses/library/7/DPL/5/DPL_5_F)

由于存在每个盒子至少需要一个球的限制，那么可以每个盒子先放进去1个球  

那么问题就转为 **没有限制** 的情况，也就是重叠组合的情况  

答案为: $H(N-X,X)$

#### 区分球，不区分盒子

**没有限制**

[例题](https://onlinejudge.u-aizu.ac.jp/courses/library/7/DPL/5/DPL_5_G)

将 $N$ 个球分为至多 $X$ 个非空集合的方法数 称为 贝尔数  

$B(N,X)=\sum_{i=0}^{X}S(N,i)$  

**至多一个**

当 $N\le X$ 时，所有球都放入盒子，方法数为1  

$N>X$ 时，不能将所有球放入盒子，方法数为0

简记为: $[N\le X]$

这个符号为 "Iverson bracket",艾弗森括号

**至少一个**

[例题](https://onlinejudge.u-aizu.ac.jp/courses/library/7/DPL/5/DPL_5_I)

将一个有 $N$ 物品的集合分为 $X$ 个非空子集 的方法数 称之为第二类斯特林数 $S(N,X)$

#### 不区分球，不区分盒子

**没有限制**

[例题](https://onlinejudge.u-aizu.ac.jp/courses/library/7/DPL/5/DPL_5_J)

将 $N$ 球 划分到至多 $X$ 个盒子 的方法数称之为 划分数

根据公式: $p(n,k)=p(n,k-1)+p(n-k,n) 预先计算p$

再根据 $p_{\le X}(N)=\sum_{i=0}^{X}p(X,i)$ 加和即可  

**至多一个**

[例题](https://onlinejudge.u-aizu.ac.jp/courses/library/7/DPL/5/DPL_5_K)

此情况同之前一样，故直接给出答案: $[N\le X]$

**至少一个**

[例题](https://onlinejudge.u-aizu.ac.jp/courses/library/7/DPL/5/DPL_5_L)

由于每个都需要至少需要一个  

所以先对每个盒子都放一个:

答案为： $p_{\le X}(N-X)$  


#### 模板仅供参考

```cpp
using i64 = long long;
template <typename T, T MOD = 1000000007> struct Mint {
    inline static constexpr T mod = MOD;
    T v;
    Mint() : v(0) {}
    Mint(signed v) : v(v) {}
    Mint(long long t) {
        v = t % MOD;
        if (v < 0)
            v += MOD;
    }

    Mint pow(long long k) {
        Mint res(1), tmp(v);
        while (k) {
            if (k & 1)
                res *= tmp;
            tmp *= tmp;
            k >>= 1;
        }
        return res;
    }

    static Mint add_identity() { return Mint(0); }
    static Mint mul_identity() { return Mint(1); }

    Mint inv() { return pow(MOD - 2); }

    Mint &operator+=(Mint a) {
        v += a.v;
        if (v >= MOD)
            v -= MOD;
        return *this;
    }
    Mint &operator-=(Mint a) {
        v += MOD - a.v;
        if (v >= MOD)
            v -= MOD;
        return *this;
    }
    Mint &operator*=(Mint a) {
        v = 1LL * v * a.v % MOD;
        return *this;
    }
    Mint &operator/=(Mint a) { return (*this) *= a.inv(); }

    Mint operator+(Mint a) const { return Mint(v) += a; }
    Mint operator-(Mint a) const { return Mint(v) -= a; }
    Mint operator*(Mint a) const { return Mint(v) *= a; }
    Mint operator/(Mint a) const { return Mint(v) /= a; }

    Mint operator+() const { return *this; }
    Mint operator-() const { return v ? Mint(MOD - v) : Mint(v); }

    bool operator==(const Mint a) const { return v == a.v; }
    bool operator!=(const Mint a) const { return v != a.v; }
};
template <typename T, T MOD>
std::ostream &operator<<(std::ostream &os, Mint<T, MOD> m) {
    os << m.v;
    return os;
}

using Z = Mint<i64>;

struct Combination {
    std::vector<Z> fac, ifac;
    int N;
    Combination(int _N) : N(2 * _N), fac(2 * _N + 1), ifac(2 * _N + 1) {
      fac[0] = Z(1);
      for (int i = 1; i <= N; i++) {
        fac[i] = fac[i - 1] * Z(i);
      }
      ifac[N] = fac[N].inv();
      for (int i = N - 1; i >= 0; i--) {
        ifac[i] = ifac[i + 1] * Z(i + 1);
      }
    }

    Z C(int n, int k) {
        if (n < k or n < 0 or k < 0) {
            return Z(0);
        }
        return fac[n] * ifac[n - k] * ifac[k];
    }
    Z P(int n, int k) {
        if (n < k or n < 0 or k < 0) {
            return Z(0);
        }
        return fac[n] * ifac[n - k];
    }
    Z H(int n,int k){
      return C(n+k-1,n);
    }
    Z S(int n,int k){
      Z ans=0;
      for(i64 i=0;i<=k;i++){
        if((k-i)%2==0){
          ans+=C(k,i)*Z(i).pow(n);
        }
        else{
          ans-=C(k,i)*Z(i).pow(n);
        }
      }
      return ans*ifac[k];
    }
};

```