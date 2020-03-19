---
title: 数列求和
date: 2018-10-25 12:30:00
categories:
  - Luogu
tags:
  - 数学
  - 快速幂
  - 排列组合
  - 乘法逆元

mathjax: true
---

# [Luogu P4948 数列求和](https://www.luogu.org/problemnew/show/P4948)

一道究极的数列题
前置知识点:
- 数列
- 排列组合
- 快速幂
- 乘法逆元

<!--more-->

## 题面

数列$b_i = i^k a^i$

求 $b_i$ 的前 $n$ 项和 $T_n mod (10^9+7)$

![数据范围](https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/2Luogu-P4948/data.png)

## 题解

~~很大程度~~借鉴[官方题解](https://www.luogu.org/blog/shanjingbo0221/T1-solution)

### 暴力

显然暴力可以搞个60分吧

**数据点1,2**

暴力枚举+快速幂 $O(n \log k)$

因为 $a^i$ 是可以递推的

**数据点3**

$k=0$

此时 $b_i = a^i$

等比数列求和 $\frac{a(a^n-a)}{a-1}$

加个乘法逆元

**数据点4**

$k=1$

此时 $b_i = i a^i$

$T_n = 1a^1+2a^2+3a^3+...+na^n$

$aT_n = 1a^2+2a^3+3a^4+...+na^{n+1}$

$(a-1)T_n = na^{n+1}-a-\sum\limits_{i=2}^{n}a^i$

$(a-1)T_n = na^{n+1}-\sum\limits_{i=1}^{n}a^i$

$(a-1)T_n = na^{n+1}-\frac{a(a^n-a)}{a-1}$

$T_n = \frac{na^{n+1}-\frac{a(a^n-a)}{a-1}}{a-1}$

**数据点5,6**

$k=2$

反正我推不来

---

### 40分代码

```cpp
#include <bits/stdc++.h>

using namespace std;

const int MOD = 1e9+7;

long long n, ans;
int a, k;

void exgcd(int a, int b, int &x, int &y)
{
    if(!b) { x = 1; y = 0; return; }
    exgcd(b, a%b, y, x);
    y -= a/b*x;
}

inline int mul_inverse(int a, int mo)
{
    int x, y;
    exgcd(a, mo, x, y);
    return (x%mo+mo)%mo;
}

inline long long qpow(long long a, long long p, int mo)
{
    long long res = 1;
    while(p)
    {
        if(p&1) res = res*a%mo;
        a = a*a%mo;
        p >>= 1;
    }
    return res;
}

int main()
{
    cin >> n >> a >> k;
    if(k == 0)
        ans = (qpow(a, n, MOD)-1)*a%MOD*mul_inverse(a-1, MOD)%MOD;
    else if(k == 1)
    {
        ans = (qpow(a, n, MOD)-1)*a%MOD*mul_inverse(a-1, MOD)%MOD;
        ans = mul_inverse(a-1, MOD)*(n%MOD*qpow(a, n+1, MOD)%MOD-ans+MOD)%MOD;
    }
    else
    {
        long long cur = 1; // cur = a^i
        for(int i = 1; i <= n; ++i)
        {
            cur = cur*a%MOD;
            ans = (ans+qpow(i, k, MOD)*cur%MOD)%MOD;
        }
    }
    cout << ans << endl;
    return 0;
}
```
---

总之敲了暴力之后就有个感觉

关键肯定是 $i^k$

### 满分做法

设 $T_n(k) = \sum\limits_{i=1}^{n}i^k a^i$

$T_n(k) = 1^ka^1+2^ka^2+3^ka^3+...+n^ka^n$

$aT_n(k) = 1^ka^2+2^ka^3+3^ka^4+...+n^ka^{n+1}$

$(a-1)T_n(k) = n^ka^{n+1}-a+\sum\limits_{i=2}^{n}((i-1)^k-i^k)a^i$

这时采用排列组合

$(i-1)^k = \sum\limits_{j=0}^{k}C_k^j \times i^j\times(-1)^{k-j}$

$(i-1)^k-i^k = \sum\limits_{j=0}^{k-1}C_k^j \times i^j\times(-1)^{k-j}$

代回进去

$(a-1)T_n(k) = n^ka^{n+1}-a+\sum\limits_{i=2}^{n}\sum\limits_{j=0}^{k-1} i^j\times a^i\times C_k^j \times (-1)^{k-j}$

这时又惊奇得望向 $T_n(k) = \sum\limits_{i=1}^{n}i^k a^i$

$T_n(k)-a = \sum\limits_{i=2}^{n}i^k a^i$

$(a-1)T_n(k) = n^ka^{n+1}-a+\sum\limits_{j=0}^{k-1} [C_k^j \times (-1)^{k-j} \times (T_n(j)-a)]$

由此就可以递推出最终结果是$T_n(k)$

然后初始状态 $T_n(0) = a^1+a^2+a^3+... +a^n = \frac{a(a^n-a)}{a-1}$

我们可以预处理排列组合 $C_i^j,(i \in [0, k], j\in [0, i])$

$n^k$ 用递推

总的复杂度是$O(k^2)$

## 代码

```cpp
#include <bits/stdc++.h>

using namespace std;

const int Maxk = 2e3+7;
const int MOD = 1e9+7;

long long n;
int a, k, inv;
int C[Maxk][Maxk];
long long T[Maxk];

void exgcd(int a, int b, int &x, int &y)
{
    if(!b) { x = 1; y = 0; return; }
    exgcd(b, a%b, y, x);
    y -= a/b*x;
}

inline int mul_inverse(int a, int mo)
{
    int x, y;
    exgcd(a, mo, x, y);
    return (x%mo+mo)%mo;
}

inline long long qpow(long long a, long long p, int mo)
{
    long long res = 1;
    a %= mo;
    while(p)
    {
        if(p&1) res = res*a%mo;
        a = a*a%mo;
        p >>= 1;
    }
    return res;
}

inline void init()
{
    for(int i = 0; i <= k; ++i)
    {
        C[i][0] = C[i][i] = 1;
        for(int j = 1; j <= i/2; ++j)
            C[i][j] = C[i][i-j] = (C[i-1][j-1]+C[i-1][j])%MOD;
    }

    inv = mul_inverse(a-1, MOD);
    T[0] = (qpow(a, n, MOD)-1+MOD)%MOD*a%MOD*inv%MOD;
}

int main()
{
    cin >> n >> a >> k;
    init();
    long long tmpn = n%MOD, // tmpn = n^i
              tmpa = qpow(a, n+1, MOD);

    for(int i = 1; i <= k; ++i)
    {
        T[i] = (tmpn*tmpa%MOD-a+MOD)%MOD;
        tmpn = tmpn*(n%MOD)%MOD;
        for(int j = 0, flag; j < i; ++j)
        {
            flag = (i-j)&1 ? -1 : 1;
            T[i] = (T[i]+((T[j]-a+MOD)%MOD*C[i][j]%MOD*flag+MOD)%MOD)%MOD;
        }
        T[i] = T[i]*inv%MOD;
    }

    cout << T[k] << endl;
    return 0;
}
```