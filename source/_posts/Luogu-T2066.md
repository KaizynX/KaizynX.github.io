---
title: 大循环
date: 2018-11-16 14:00:00
categories:
  - Luogu
tags:
  - 数学
  - 找规律
  - 杨辉三角
  - 乘法逆元
  - 排列组合

---

**非常**有趣的~~数学~~找规律题目

<!--more-->
# [Luogu  T2066 大循环](https://www.luogu.org/problemnew/show/T2066)

这题花了我好久啊

首先一眼看出有蹊跷

两眼看不出什么奇技淫巧

乖乖敲暴力

暴力源码↓

```cpp
ans = f((int)(q%MOD))*work(1, n+1)%MOD;

inline long long f(int x)
{
    long long res = 0;
    for(int i = m; i >= 0; --i)
        res = (res*x+a[i])%MOD;
    return res;
}

int work(int cur, int last)
{
    if(b[cur][last]) return b[cur][last];
    if(cur == k) return last-1;
    for(int i = 1; i < last; ++i)
        (b[cur][last]+=work(cur+1, i))%=MOD;
    return b[cur][last];
}
```

10分...没事我只是先试试手

接着考虑记忆化

```cpp
ans = f((int)(q%MOD))*b[2][n]%MOD;

void work()
{
    for(int i = 2; i <= n-k+2; ++i) b[k][i] = (b[k][i-1]+i-1)%MOD;
    for(int i = k-1; i > 1; --i)
        for(int j = k-i+2; j <= n-i+2; ++j) // b[i] = j
            b[i][j] = (b[i][j-1]+b[i+1][j-1])%MOD;
}
```

现在有40分了,剩下的RE了,超出空间限制

于是我摸索了一下转移方程

发现用二维就可以了,滚动数组!!!

继而发现其实一维的也可以啊

`b[i][j] = b[i][j-1]+b[i+1][j-1]`

假如一维的话, `b[i][j]` 就相当于 `b[i][j-1]`

```cpp
ans = f((int)(q%MOD))*b[n-k+1]%MOD;

void work()
{
    for(int i = 1; i <= n; ++i) b[i] = i;
    for(int i = 2; i <= k; ++i)
        for(int j = 1; j <= n-i+1; ++j)
            (b[j] += b[j-1]) %= MOD;
}
```

嗯,还是40分,不过这次从RE变成TLE了

其实聪明的小朋友应该看出状态转移的玄奇了(而蒟蒻我是打了份work()的表才打出来的)

你看b数组,某个位置等于左边加上左上角,翻一下就是,挤一挤,就是杨慧三角

也就是排列组合

```cpp
#include <bits/stdc++.h>

using namespace std;

const int Maxn = 5e5+7;
const int MOD = 1e9+7;

int n, m, k;
long long q, ans;
int a[Maxn];
int mul[Maxn];

inline long long f(int x)
{
    long long res = 0;
    for(int i = m; i >= 0; --i)
        res = (res*x+a[i])%MOD;
    return res;
}

inline void init()
{
    mul[1] = 1;
    for(int i = 2; i <= n; ++i)
        mul[i] = (int)((1ll*mul[i-1]*i)%MOD);
}

void exgcd(int a, int b, int &x, int &y)
{
    if(!b) { x = 1; y = 0; return; }
    exgcd(b, a%b, y, x);
    y -= a/b*x;
}

inline int mul_inverse(int b)
{
    int x, y;
    exgcd(b, MOD, x, y);
    return (x%MOD+MOD)%MOD;
}

inline long long C(int p, int q) // C_p^q
{
    if(p < q) return 0;
    int inv1 = mul_inverse(mul[q]), inv2 = mul_inverse(mul[p-q]);
    return 1ll*mul[p]*inv1%MOD*inv2%MOD;
}

int main()
{
    scanf("%d%d%d%lld", &n, &m, &k, &q);
    for(int i = 0; i <= m; ++i) scanf("%d", a+i);
    init();
    ans = f((int)(q%MOD))*C(n, k)%MOD;
    printf("%lld\n", ans);
    return 0;
}
```