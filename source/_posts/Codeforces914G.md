---
title: CF914G Sum the Fibonacci
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2020-08-16 18:00:00
mathjax: true
categories:
  - Codeforces
tags:
  - 卷积
  - FWT
  - FST
description: 一道涉及方方面面的关于二进制卷积的题
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/Codeforces914G/cover.jpg
---
# 题面

[洛谷链接](https://www.luogu.com.cn/problem/CF914G)

[原题地址](https://codeforces.com/problemset/problem/914/G)

# 思路

首先很难过的告诉你这是道纯纯的板子题,但又不是蠢蠢的板子题

你需要对板子有一定的理解才行

首先处理一下输入,重定义 $s_i$ 为值为 $i$ 的数量

然后只要你够熟悉板子,就知道可以分别 FST, FWT 卷积一下就可以求出

$(s_a | s_b),(s_d \bigotimes s_e)$ 的各种方案的数量 注: $s_a\& s_b = 0$

稍加思索,数量乘上对应的斐波那契值就是该二进制对应的权值

记 $(s_a | s_b),s_a \& s_b = 0$ 的权值数组为 $aor$

记 $(s_d \bigotimes s_e)$ 的权值数组为 $axor$

再把 $s$ 数组乘上斐波那契也变成权值

求 $aor_a \& s_b \& axor_c = 2^i$

我第一个想法是枚举这个 $i$ 此时所需考虑的即第 $i$ 位为 1 的 $a,b,c$

把 $a,b,c$ 的第 $i$ 位去掉,再做**子集卷积**即可满足要求

复杂度 $O(17\times 16^2 2^{16})$

时限 4s 该做法就 4.5s TLE (菜得不会卡常)

这我就迷惑了,听说 $3^n$ 都给暴艹过去我这怎么不行

然后仔细再看 $A_j \& B_k = 2^i$

发现在做子集卷积的时候,只要满足或起来的位数比状态位数恰好多一位即可

即 `fwt[__builtin_popcount(i)+1][i]` 懂的都懂

复杂度就是子集卷积的复杂度 $O(n^2 2^n)$

# 代码

都是板子,主要看 `main()`

{% spoiler "代码" %}
```cpp
#include <bits/stdc++.h>

using namespace std;

const int MOD = 1e9+7;
const int N = 1e6+7;
const int B = 17;

template <typename T>
T exgcd(const T a, const T b, T &x, T &y) {
  if (!b) { x = 1; y = 0; return a; }
  T d = exgcd(b, a%b, y, x);
  y -= a/b*x;
  return d;
}

template <typename T>
inline T mul_inverse(const T &a, const T &mo = MOD) {
  T x, y;
  exgcd(a, mo, x, y);
  return (x%mo+mo)%mo;
}

namespace FWT {
#define forforfor for (int l = 2; l <= len; l <<= 1)\
                  for (int i = 0, k = l>>1; i < len; i += l)\
                  for (int j = 0; j < k; ++j)

  const int SIZE = 1<<17;
  int len;
  int f[SIZE], g[SIZE];
  template <class T> void init(T a[], const int &n, T b[], const int &m) {
    len = 1;
    while (len < max(n, m)) len <<= 1;
    for (int i = 0; i < n; ++i) f[i] = a[i];
    for (int i = n; i < len; ++i) f[i] = 0;
    for (int i = 0; i < m; ++i) g[i] = b[i];
    for (int i = m; i < len; ++i) g[i] = 0;
  }
  template <class T> void fwt_or(T a[], const int x = 1) {
    forforfor a[i+j+k] = (a[i+j+k]+1ll*a[i+j]*x)%MOD;
  }
  template <class T> void fwt_and(T a[], const int x = 1) {
    forforfor a[i+j] = (a[i+j]+1ll*a[i+j+k]*x)%MOD;
  }
  template <class T> void fwt_xor(T a[], const int x = 1) {
    forforfor {
      (a[i+j] += a[i+j+k]) %= MOD;
      a[i+j+k] = (a[i+j]-2*a[i+j+k]%MOD+MOD)%MOD;
      a[i+j] = 1ll*a[i+j]*x%MOD; a[i+j+k] = 1ll*a[i+j+k]*x%MOD;
    }
  }
  template <class T> void work_or(const T a[], const int &n, const T b[], const int &m) {
    init(a, n, b, m); fwt_or(f); fwt_or(g);
    for (int i = 0; i < len; ++i) f[i] = 1ll*f[i]*g[i]%MOD;
    fwt_or(f, MOD-1); // fwt_or(x, -1)
  }
  template <class T> void work_and(const T a[], const int &n, const T b[], const int &m) {
    init(a, n, b, m); fwt_and(f); fwt_and(g);
    for (int i = 0; i < len; ++i) f[i] = 1ll*f[i]*g[i]%MOD;
    fwt_and(f, MOD-1); // fwt_and(x, -1)
  }
  template <class T> void work_xor(const T a[], const int &n, const T b[], const int &m) {
    init(a, n, b, m); fwt_xor(f); fwt_xor(g);
    for (int i = 0; i < len; ++i) f[i] = 1ll*f[i]*g[i]%MOD;
    fwt_xor(f, mul_inverse(2)); // fwt_xor(x, 1/2)
  }
#undef forforfor
} // namespace FWT

namespace FST {
  const int W = 17;
  const int N = 1<<W;
  int len, bit;
  int f[W+2][N], g[W+2][N], h[W+2][N], res[N];
  template <class T> void fwt(T a[], const int x = 1) {
    for (int l = 2; l <= len; l <<= 1)
    for (int i = 0, k = l>>1; i < len; i += l)
    for (int j = 0; j < k; ++j)
      a[i+j+k] = (a[i+j+k]+1ll*a[i+j]*x)%MOD;
  }
  template <class T> void work(const T a[], const int &n, const T b[], const int &m) {
    len = 1; bit = 0;
    while (len < max(n, m)) len <<= 1, ++bit;
    for (int i = 0; i <= bit+1; ++i)
      for (int j = 0; j < len; ++j)
        f[i][j] = g[i][j] = h[i][j] = 0;
    for (int i = 0; i < n; ++i) f[__builtin_popcount(i)][i] = a[i];
    for (int i = 0; i < m; ++i) g[__builtin_popcount(i)][i] = b[i];
    for (int i = 0; i <= bit+1; ++i) {
      fwt(f[i]); fwt(g[i]);
      for (int j = 0; j <= i; ++j)
        for (int k = 0; k < len; ++k)
          h[i][k] = (h[i][k]+1ll*f[j][k]*g[i-j][k])%MOD;
      fwt(h[i], MOD-1); // fwt(h[i], -1)
    }
    for (int i = 0; i < len; ++i) res[i] = h[__builtin_popcount(i)][i];
  }
} // namespace FST

int n;
int s[1<<B], fib[1<<B], aor[1<<B], axor[1<<B], aand[1<<B];

signed main() {
  ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
  fib[0] = 0; fib[1] = 1;
  for (int i = 2; i < 1<<B; ++i) fib[i] = (fib[i-2]+fib[i-1])%MOD;

  cin >> n;
  for (int i = 1, si; i <= n; ++i) cin >> si, ++s[si];

  FST::work(s, 1<<B, s, 1<<B);
  for (int i = 0; i < 1<<B; ++i) aor[i] = 1ll*FST::res[i]*fib[i]%MOD;
  FWT::work_xor(s, 1<<B, s, 1<<B);
  for (int i = 0; i < 1<<B; ++i) axor[i] = 1ll*FWT::f[i]*fib[i]%MOD;
  FWT::work_and(aor, 1<<B, axor, 1<<B);
  memcpy(aand, FWT::f, sizeof aand);
  for (int i = 0; i < 1<<B; ++i) s[i] = 1ll*s[i]*fib[i]%MOD;

  int res = 0;
  FST::work(aand, 1<<B, s, 1<<B);
  for (int i = 0; i < 1<<B; ++i)
    (res += FST::h[__builtin_popcount(i)+1][i]) %= MOD;
  cout << res << endl;
  return 0;
}
```

{% endspoiler %}

另附上述 4.5s TLE 代码

{% spoiler "代码" %}
```cpp
int a[1<<B-1], b[1<<B-1];
signed main() {
  // 略略略
  FST::work(s, 1<<B, s, 1<<B);
  for (int i = 0; i < 1<<B; ++i) aor[i] = 1ll*FST::res[i]*fib[i]%MOD;
  FWT::work_xor(s, 1<<B, s, 1<<B);
  for (int i = 0; i < 1<<B; ++i) axor[i] = 1ll*FWT::f[i]*fib[i]%MOD;
  FWT::work_and(aor, 1<<B, axor, 1<<B);
  memcpy(aand, FWT::f, sizeof aand);
  for (int i = 0; i < 1<<B; ++i) s[i] = 1ll*s[i]*fib[i]%MOD;
  int res = 0;
  for (int bit = 0; bit < B; ++bit) {
    // (sa|sb)&sc&(sd^se) == 1<<bit
    for (int i = 0, j; i < 1<<B-1; ++i) {
      j = (i&((1<<bit)-1))+(i>>bit<<bit+1)+(1<<bit);
      a[i] = aand[j];
      b[i] = s[j];
    }
    FST::work(a, 1<<B-1, b, 1<<B-1);
    for (int i = 0; i < 1<<B-1; ++i) (res += FST::res[i]) %= MOD;
  }
  cout << res << endl;
  return 0;
}
```

{% endspoiler %}