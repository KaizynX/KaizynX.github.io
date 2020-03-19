---
title: 情侣？给我烧了！
date: 2018-10-06 17:30:00
categories:
  - Luogu
tags:
  - 数学
  - 排列组合
  - 乘法逆元
mathjax: true
---
# [Luogu T44341 情侣？给我烧了！](https://www.luogu.org/problemnew/show/T44341)

来自比赛[迷途の家の水题大赛](https://www.luogu.org/contest/show?tid=11260) B 题

## 思路

假设 $i$ 对情侣两两互不配对的方案数是 $f[i]$

那么当有 $k$ 对情侣配对的方案数是

1. 选择 $k$ 对情侣 $C_n^k$
2. 选择 $k$ 个座位并可以按照任意顺序坐 $A_n^k$
3. 每对情侣两个人可以交换位置,共 $2^k$
4. 剩下的 $n-k$ 对情侣不能有坐在一起的 $f[n-k]$

最终结果就是 $2^k \times C_n^k \times A_n^k \times f[n-k]$

之后的问题就是求解 $f[n-k]$

正着推有点复杂 ~~我推不出来~~

所以不妨逆着想

$f[i]$ 就是 $i$ 对人的所有排列减去 有任意对数情侣配对的

所以就可以得到一个递推式

注意此时所有人的排列是 $i$ **对**人, 共 $2i$ **个**人

$f[i] = A_{2i}^{2i} - \sum\limits_{j=1}^{i} (2^j \times C_i^j \times A_i^j \times f[i-j])$

都到这个地步了

只要预处理出 $A_i^j, C_i^j, f[i], 2^i$ 即可

### 预处理 $A_i^j$

$A_i^j = \prod\limits_{j=i-j+1}^{i} j$

$A_i^j = A_i^{j-1} \times (i-j+1)$

### 预处理 $C_i^j$

当然你可以线性推

但是因为取模,要用到乘法逆元

这个数据范围当然可以线性逆元

最终还是 $O(n^2)$

不过不妨采用

$C_i^j = C_{i-1}^{j-1}+C_{i-1}^j$

### 预处理 $f[i]$

前面预处理的范围都只是 $n$

但是这里要用到 $A_{2i}^{2i}$, 上限在 $2n$

扩大 $A_i^j$ 的预处理范围未免太浪费

我们只需要预处理 $A_{2i}^{2i} = (2i)!$

剩下好说

## 代码
```cpp
#include <bits/stdc++.h>

using namespace std;

const int Maxn = 1e3+7;
const int MOD = 998244353;

int T, n;
long long A[Maxn][Maxn], C[Maxn][Maxn], pow2[Maxn], f[Maxn], P[Maxn<<1];

inline void init();
inline void read(int&);
void write(int);

int main()
{
    init();
    read(T);
    while(T--)
    {
        read(n);
        for(int i = 0; i <= n; ++i)
        {
            long long res = A[n][i]*C[n][i]%MOD*pow2[i]%MOD*f[n-i]%MOD;
            write((int)res); putchar('\n');
        }
    }
    return 0;
}

inline void init()
{
    const int N = 1e3;

    pow2[0] = 1;
    for(int i = 1; i <= N; ++i)
        pow2[i] = (pow2[i-1]<<1)%MOD;

    // P[i] = i!
    P[1] = 1;
    for(int i = 2; i <= N*2; ++i)
        P[i] = P[i-1]*i%MOD;

    for(int i = 1; i <= N; ++i)
    {
        A[i][0] = 1;
        A[i][1] = i;
        for(int j = 2; j <= i; ++j)
            A[i][j] = A[i][j-1]*(i-j+1)%MOD;
    }

    for(int i = 1; i <= N; ++i)
    {
        C[i][0] = C[i][i] = 1;
        for(int j = 1; j <= i/2; ++j)
            C[i][j] = C[i][i-j] = (C[i-1][j-1]+C[i-1][j])%MOD;
    }

    // i队的不配对方案数
    f[0] = 1; f[1] = 0;
    for(int i = 2; i <= N; ++i)
    {
        // 有j队匹配
        for(int j = 1; j <= i; ++j)
            f[i] = (f[i]+C[i][j]*A[i][j]%MOD*pow2[j]%MOD * f[i-j]%MOD)%MOD;
        f[i] = (P[i*2]-f[i]+MOD)%MOD;
    }
}

inline void read(int &x)
{
    char c;
    while(!isdigit((c=getchar())));
    x = c-'0';
    while(isdigit((c=getchar())))
        x = (x<<1)+(x<<3) + c-'0';
}

void write(int x)
{
    if(x/10) write(x/10);
    putchar(x%10+'0');
}
```