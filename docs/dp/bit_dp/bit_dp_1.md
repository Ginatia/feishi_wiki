---
title: bit_dp_2
date: 2024-09-08
---

#### 位运算

| 作用 | 代码 |
| --- | --- |
| 设置第 $i$ 位为 1 | <code>b&#124;(1<<i)</code> |
| 设置第 $i$ 位为 0 | `b&!(1<<i)` |
| 检查第 $i$ 位为 1 | `b&(1<<i)!=0` |

#### Problem 1

$有 N 人与 N 任务,每个任务都分配给一个人,一个人只被分配一个任务$  

$cost[i][j] \quad 表示 第 i 个人完成第 j 项任务的花费$  

##### bit dp

$假设当前状态为 (k,mask)$

$意味着前面[0,k-1]的人都已经分配好任务$

$mask 是任务分配情况的集合,用二进制表示任务分配情况$

$若第 i 个任务可以分配给第 k 个人,那么当且仅当:$

` mask&(1<<i)==0`

```cpp
dp[k+1][mask|(1<<i)]
=std::min(dp[k+1][mask|(1<<i)],dp[k][mask]+cost[k][i])
```

**第一维枚举第k个人已经蕴含在mask的设置为1的任务数,所以可以省略**

```cpp
dp[mask|(1<<i)]=
std::min(dp[mask|(1<<i)],dp[mask]+cost[k][i])
```

###### 练习

$有 N 男人, N 女人 ,为其一一配对$  

$若第 i 个男人与第 j 个女人可以配对$

$则 a[i][j]=1,否则为0$  

$问有多少种配对方法?答案对 10^{9}+7 取模$  

**思路**

$首先枚举到第i个男人,使其与第j个女人配对的第j个女人还未被配对$  

$那么先枚举男人$

$再枚举已经配对的女人的集合$

$最后枚举可以与这个男人配对的女人$

$时间复杂度为 O(N^22^{N})$

$若枚举到了第i个男人,也就是区间[0,i-1],一共i个男人已经确定$

$那么前面i个男人已经确定配对$

$由于男女一一对应,那么也应该有i个女人在集合mask里被确定$  

$也就是说mask里有i个1$  

$根据此剪枝,时间复杂度被降低到 O(N2^{N})$

```cpp
using Z=Mint<i64>;

int calc_one(int n){
    int cnt_one=0;
    while(n){
        cnt_one+=n&1;
        n>>=1;
    }
    return cnt_one;
}

void solve(){
    int N;
    read(N);
    std::vector a(N,std::vector(N,0));
    for(int i=0;i<N;i++){
        for(int j=0;j<N;j++){
            read(a[i][j]);
        }
    }
    std::vector dp(N+1,std::vector<Z>((1<<N)));
    dp[0][0]=1;
    for(int i=0;i<N;i++){
        for(int mask=0;mask<(1<<N);mask++){
            if(i!=calc_one(mask)){
                continue;
            }
            for(int j=0;j<N;j++){
                if((mask>>j&1)||!a[i][j]){
                    continue;
                }
                dp[i+1][mask|(1<<j)]+=dp[i][mask];
            }
        }
    }
    std::cout<<dp[N][(1<<N)-1]<<'\n';
}
```
