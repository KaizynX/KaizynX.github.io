---
title: 播放列表的数量
date: 2018-10-8 07:50:00
categories:
  - LeetCode
tags:
  - 数学
  - 排列组合
  - 容斥原理
  - 乘法逆元
  - DP

mathjax: true
---
# [LeetCode 920. 播放列表的数量](https://leetcode-cn.com/problems/number-of-music-playlists/)

## 题面

你的音乐播放器里有 N 首不同的歌，在旅途中，你的旅伴想要听 L 首歌（不一定不同，即，允许歌曲重复）。请你为她按如下规则创建一个播放列表：

- 每首歌至少播放一次。
- 一首歌只有在其他 K 首歌播放完之后才能再次播放。
  
返回可以满足要求的播放列表的数量。由于答案可能非常大，请返回它模 10^9 + 7 的结果。

## 数据范围

0 <= K < N <= L <= 100

## 样例

### 示例 1：

输入：N = 3, L = 3, K = 1

输出：6

解释：有 6 种可能的播放列表。[1, 2, 3]，[1, 3, 2]，[2, 1, 3]，[2, 3, 1]，[3, 1, 2]，[3, 2, 1].

### 示例 2：

输入：N = 2, L = 3, K = 0

输出：6

解释：有 6 种可能的播放列表。[1, 1, 2]，[1, 2, 1]，[2, 1, 1]，[2, 2, 1]，[2, 1, 2]，[1, 2, 2]

### 示例 3：

输入：N = 2, L = 3, K = 1

输出：2

解释：有 2 种可能的播放列表。[1, 2, 1]，[2, 1, 2]


### 示例 4：

输入：N = 3, L = 3, K = 0

输出：6

---
## 我的思路

假如我们第 i 个放的是 歌j

那么在第 i+1 到 i+K 都不能再放 歌j

我们可以用某种**支持区间修改的数据结构**来维护这个信息

以此可以求得第 i 个 可以选的歌的种类有几种

(我在这里用的是**树状数组**, ~~其实数据范围支持暴力求解~~)

---

再根据排列组合,就可以求出总的方案数

还有几个限制条件就是 N 首歌必须都 至少放一遍

此时方案是包括 N, N-1, N-2...等只要 K 能满足的情况

所以方案数还要减去没放完 N 首的

我们可以按照上述方法求出 N-1 首歌时的方案

那么求出来的就包括了 N-1, N-2, N-3 ... 首歌的情况,减去即可

然后还有根据排列组合,我们求的 N-1 首歌的情况

---

那么这 N-1 首歌又是哪 N-1 首? —— $C_N^{N-1}$

---

此时看运行 示例4 就会发现错了

错误原因是加上排列之后有重复的情况

假如 3 选 2,有 (1, 2), (2, 3), (1, 3)

那么 (1, 2) 包括了 (1, 2), (1), (2)

(2, 3) 包括了 (2, 3), (2), (3)

发现了吗,多减了一部分 (2)

这个就是传说中的**容斥原理**

假设 $f[i]$ 是我们用上述方法求出的 i 首歌(包含 i-1, i-2...) 的方案数

那么综合容斥原理最后结果就是

$C_N^0\times f[N] - C_N^1\times f[N-1] + C_N^2\times f[N-2] ...$

最后的范围, 一定是要歌的数量满足 K 的间隔还能放的

$= \sum\limits_{i=N}^{K+1} (C_N^i \times f[i])$ ——(额这个 -1 懒得推)

$= \sum\limits_{i=0}^{N-i > K} (-1)^{i} \times (C_N^{N-i} \times f[N-i])$

---
### 复杂度分析

1. 区间修改
   
   暴力 $O(N^2)$
   
   树状数组 $O(N \log N)$

2. 排列组合

    暴力 ,每遍都是 $O(N)$, 总的 $O(N^2)$

    数据太大还需要乘法逆元

    观察发现所求的排列组合都是 $C_N^i$

    所以可以递推, 总的复杂度 $O(N)$

3. 容斥原理

    上述分析的是计算一遍 $f[i]$ 的复杂度

    现在乘上 $N$

最后在大环境中, 发现瓶颈是在区间修改上(所以说排列组合...)

暴力 $O(N^3)$ , 优化 $O(N^2 \log N)$

---
### 代码

#### 暴力求排列组合,代码冗长
```cpp
class Solution {
public:
    const int MOD = 1e9+7;
    int tr[107] = {0};
    
    inline void add(int i, int x, int n)
    {
        for( ; i <= n; i += i&-i) tr[i] += x;
    }
    
    inline int query(int i)
    {
        int res = 0;
        for( ; i; i -= i&-i) res += tr[i];
        return res;
    }
    
    inline long long calc(int N, int L, int K)
    {
        memset(tr, 0, sizeof tr);
        long long res = 1;
        for(int i = 1, cur; i <= L; ++i)
        {
            cur = N+query(i);
            if(cur <= 0) return 0;
            res = res*cur%MOD;
            add(i+1, -1, L); add(i+K+1, 1, L);
        }
        return res;
    }
    
    inline long long qpow(long long a, int p)
    {
        long long res = 1;
        while(p)
        {
            if(p&1) res = res*a%MOD;
            p >>= 1;
            a = a*a%MOD;
        }
        return res;
    }
    
    inline long long mul_reverse(long long x)
    {
        return qpow(x, MOD-2);
    }
    
    inline long long C(int p, int q) // C_p^q
    {
        if(q == p || q == 0) return 1;
        if(q == p-1 || q == 1) return p;
        long long P = 1, Q = 1;
        for(int i = 1; i <= q; ++i)
        {
            P = P*(p-i+1)%MOD;
            Q = Q*i%MOD;
        }
        return P*mul_reverse(Q)%MOD;
    }
    
    int numMusicPlaylists(int N, int L, int K) {
        long long res = 0;
        for(int i = 0; N-i > K; ++i)
            res = (res+(i&1 ? -1 : 1)*C(N, N-i)*calc(N-i, L, K)%MOD+MOD)%MOD;
        return res;
    }
};
```
---
#### 递推排列组合
```cpp
class Solution {
public:
    const int MOD = 1e9+7;
    int tr[107] = {0};
    long long C[107] = {0}, inv[107] = {0};
    
    inline void add(int i, int x, int n)
    {
        for( ; i <= n; i += i&-i) tr[i] += x;
    }
    
    inline int query(int i)
    {
        int res = 0;
        for( ; i; i -= i&-i) res += tr[i];
        return res;
    }
    
    inline long long calc(int N, int L, int K)
    {
        memset(tr, 0, sizeof tr);
        long long res = 1;
        for(int i = 1, cur; i <= L; ++i)
        {
            cur = N+query(i);
            if(cur <= 0) return 0;
            res = res*cur%MOD;
            add(i+1, -1, L); add(i+K+1, 1, L);
        }
        return res;
    }
    
    inline void init(int N)
    {
        inv[1] = 1;
        for(int i = 2; i < N; ++i)
            inv[i] = (MOD-MOD/i)*inv[MOD%i]%MOD;
        
        C[0] = C[N] = 1;
        for(int i = 1; i < N; ++i)
            C[i] = C[N-i] = C[i-1]*(N-i+1)%MOD*inv[i]%MOD;
    }
    
    int numMusicPlaylists(int N, int L, int K) {
        init(N);
        long long res = 0;
        for(int i = 0; N-i > K; ++i)
            res = (res+(i&1 ? -1 : 1)*C[N-i]*calc(N-i, L, K)%MOD+MOD)%MOD;
        return res;
    }
};
```
---
## 大佬的想法

每每看到大佬们的代码都要忍不住落泪了

他们都是用 DP 写的 $O(n^2)$

$dp[i][j]$ 表示用 j 种歌, 一共放了 i 首长度的方案数

初始条件 $dp[0][0] = 1$

转移方程

1. $dp[i][j] += (N-j+1)\times dp[i-1][j-1]$

    第 i 首放一首新的歌曲 (之前没放过)

    那么这首新歌有 (N-j+1) 种选择

2. $dp[i][j] += max(j-K, 0) \times dp[i-1][j]$

    如果第 i 首放的是老歌 (之前放过的)

    但能不能放是个问题, 问题在于前面放过的歌有没有 K 冷却了

    此时之前放过的 j 首歌中

    第 i-1 首放的到第 i-K 首放的 还在冷却

    所以此时有 j-K 种选择

---

### 大佬的代码让我陶醉
```cpp
class Solution {
public:
    const int MOD = 1e9+7;
    long long dp[107][107];
    int numMusicPlaylists(int N, int L, int K) {
        memset(dp, 0, sizeof dp);
        dp[0][0] = 1;
        for(int i = 1; i <= L; ++i)
            for(int j = 1; j <= N; ++j)
                dp[i][j] = ((N-j+1)*dp[i-1][j-1]%MOD + dp[i-1][j]*max(j-K, 0))%MOD;
        return dp[L][N];
    }
};
```