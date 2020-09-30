---
title: ACL Beginner Contest F - Heights and Pairs
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2020-09-30 22:30:00
mathjax: true
categories:
  - AtCoder
tags:
  - NTT
  - 排列组合
  - 容斥
description: 用分治NTT解决容斥问题
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/Atcoder-ABL-F/cover.jpg
---
# 题面

[链接](https://atcoder.jp/contests/abl/tasks/abl_f)

有 $2N$ 个人每个人有一个高度 $h_i$

要求两两配对满足

1. 每个人只能配对一次
2. 每对俩人高度不同

问方案数

# 思路

## 易得

记 $ff[s]$ 为至少有 $s$ 对人高度相同情况下的方案数(就是容斥原理)

$ans=\sum\limits_{s=0}^{n}{(-1)^{s}\times ff[s]}$

记 $fff[x]$ 为 $2x$ 个人任意配对的方案数

$fff[x]=fff[x-1]\times(2x-1)$

新加入两个可以配对也可以选择和前面的任意一人组合

然而容斥只想到 $2^n$ 做法

## $O(n^2)$ dp

记 $dp[i][j]$ 为 $[1, i]$ 高度的人配对后剩下 $j$ 人未配对的方案数

初始条件 $dp[0][0]=1$

记高度 $i$ 的人一共有 $t[i]$ 个

枚举这 $t[i]$ 个人中有 $j$ 个人和之前的配对了,剩下 $t[i]-j$ 人未配对

转移方程 $dp[i][k] = \sum{C_{t[i]}^{j}\times A_{k+j-(t[i]-j)}^{j}\times dp[i-1][k+j-(t[i]-j)]}$

这是以我的水平能想到的最快的算法了呜呜

## 分治NTT

这你能信,上面那个容斥可以用分治NTT做出来

考虑当前的高度 $i$ 有 $t[i]$ 个人

那么当前高度对 $s$ 的大小贡献为 $x$ 时,方案数是 $C_{t[i]}^{2x}\times fff[x]$

求 $ff[]$ 其实就是把每个高度的贡献数组 NTT 起来,暴力 NTT 的话是 $O(n^2\log n)$

于是采用分治 NTT ,但是这个分治好像和我在洛谷上板子题不大一样呜呜

分治不是按照 $ff[]$ 数组的下标分治, 而是按照高度分治

先分别求出 $[l,mid],[mid+1,r]$ 高度的 NTT 乘积,再把两者卷积

$\sum\limits_{所有高度}t[i]=N$

因为分治,每个高度的贡献最多被卷 $\log n$ 次

总的复杂度是 $O(n\log^2 n)$ 妙啊

需要注意的就是在算答案的时候当 $s$ 大小为 $x$ 时还要乘上 $fff[n−x]$,因为还有 $n−x$ 组没有分配完

# 代码

## dp代码

{% spoiler "代码" %}
```cpp
signed main() {
  cin >> n;
  for (int i = 1; i <= n*2; ++i) {
    cin >> a[i];
    ++t[a[i]];
    m = max(m, a[i]);
  }
  comb.init(n);
  dp[0][0] = 1;
  for (int i = 1; i <= m; ++i) {
    for (int j = 0; j <= t[i]; ++j) { // j's paired with brefore ones
      for (int k = j; k <= n; ++k) if (k-j+t[i]-j <= n) {
        dp[i][k-j+t[i]-j] += comb.C(t[i], j)*comb.A(k, j)*dp[i-1][k];
      }
    }
  }
  cout << dp[m][0] << endl;
  return 0;
}
```

{% endspoiler %}

## 正解代码

{% spoiler "代码" %}
```cpp
#include <bits/stdc++.h>

// #define DEBUG

using namespace std;

const double eps = 1e-7;
const double PI = acos(-1);
typedef pair<int, int> pii;
const int MOD = 998244353;
const int INF = 0x3f3f3f3f;
const int N = 1e5+7;

template <typename T, typename H>
inline T qpow(const T &a, const H &p, const int &mo = MOD) {
  long long res = 1, x = a;
  for (H i = p; i; i >>= 1, x = x*x%mo)
    if (i&1) res = res*x%mo;
  return static_cast<T>(res);
}

struct Combination {
  int fac[N], inv[N];
  void init(const int &n) {
    fac[0] = inv[0] = fac[1] = inv[1] = 1;
    for (int i = 2; i <= n; ++i) {
      fac[i] = 1ll*fac[i-1]*i%MOD;
      inv[i] = 1ll*(MOD-MOD/i)*inv[MOD%i]%MOD;
    }
    for (int i = 2; i <= n; ++i) {
      inv[i] = 1ll*inv[i]*inv[i-1]%MOD;
    }
  }
  int A(const int &p, const int &q) {
    return p >= q ? 1ll*fac[p]*inv[p-q]%MOD : 0;
  }
  int C(const int &p, const int &q) {
    return p >= q ? 1ll*fac[p]*inv[q]%MOD*inv[p-q]%MOD : 0;
  }
};

// array [0, n)
namespace NTT {
  static const int SIZE = (1<<17)+3;
  const int G = 3;
  int len, bit;
  int rev[SIZE];
  long long f[SIZE], g[SIZE];
  template <class T>
  void ntt(T a[], int flag = 1) {
    for (int i = 0; i < len; ++i)
      if (i < rev[i]) swap(a[i], a[rev[i]]);
    for (int base = 1; base < len; base <<= 1) {
      long long wn = qpow(G, (MOD-1)/(base*2)), w;
      if (flag == -1) wn = qpow(wn, MOD-2);
      for (int i = 0; i < len; i += base*2) {
        w = 1;
        for (int j = 0; j < base; ++j) {
          long long x = a[i+j], y = w*a[i+j+base]%MOD;
          a[i+j] = (x+y)%MOD;
          a[i+j+base] = (x-y+MOD)%MOD;
          w = w*wn%MOD;
        }
      }
    }
  }
  template <class T>
  void work(T a[], const int &n, T b[], const int &m) {
    len = 1; bit = 0;
    while (len < n+m) len <<= 1, ++bit;
    for (int i = 0; i < n; ++i) f[i] = a[i];
    for (int i = n; i < len; ++i) f[i] = 0;
    for (int i = 0; i < m; ++i) g[i] = b[i];
    for (int i = m; i < len; ++i) g[i] = 0;
    for (int i = 0; i < len; ++i)
      rev[i] = (rev[i>>1]>>1)|((i&1)<<(bit-1));
    ntt(f, 1); ntt(g, 1);
    for (int i = 0; i < len; ++i) f[i] = f[i]*g[i]%MOD;
    ntt(f, -1);
    long long inv = qpow(len, MOD-2);
    for (int i = 0; i < n+m-1; ++i) f[i] = f[i]*inv%MOD;
  }
}

int n, m;
int a[N], fff[N]; // fff[i] pair 2i randly
vector<int> ff[N];
Combination comb;

void solve(int l, int r) {
  if (l == r) return;
  int m = (l+r)>>1;
  solve(l, m);
  solve(m+1, r);
  int ll = ff[l].size(), lr = ff[m+1].size();
  NTT::work(ff[l].data(), ll, ff[m+1].data(), lr);
  ff[l].resize(ll+lr-1);
  for (int i = 0; i < ll+lr-1; ++i) ff[l][i] = NTT::f[i];
}

signed main() {
  ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
  cin >> n;
  for (int i = 1; i <= n*2; ++i) cin >> a[i];
  comb.init(n*2);
  fff[0] = 1;
  for (int i = 1; i <= n; ++i) fff[i] = fff[i-1]*(i*2-1ll)%MOD;
  sort(a+1, a+2*n+1);
  for (int i = 1, j; i <= 2*n; i = j) {
    j = i+1;
    while (j <= n*2 && a[j] == a[i]) ++j;
    ++m;
    for (int k = 0; k*2 <= j-i; ++k)
      ff[m].emplace_back(1ll*comb.C(j-i, k*2)*fff[k]%MOD);
  }
  solve(1, m);
  #ifdef DEBUG
  for (int &i : ff[1]) cout << i << " ";
  cout << endl;
  #endif
  int res = 0;
  for (int i = 0; i < (int)ff[1].size(); ++i) {
    int val = 1ll*fff[n-i]*ff[1][i]%MOD;
    if (i&1) val = (MOD-val)%MOD;
    (res += val) %= MOD;
  }
  cout << res << endl;
  return 0;
}
```

{% endspoiler %}

# 参考资料

写这篇博客时官方题解并未发布,且百度上只有一篇[题解](https://www.cnblogs.com/lsq147/p/13737738.html)