---
title: KMP+数位DP
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2020-04-02 11:00:00
mathjax: true
categories:
  - ACM
tags:
  - DP
  - KMP
  - 数位DP
  - 矩阵快速幂
description: 两道 KMP+数位DP 题
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/kmp_and_dp/cover.jpg 
---
# 例一

[luogu P3193 [HNOI2008]GT考试](https://www.luogu.com.cn/problem/P3193)

## 思路

容易想到是数位dp, 设 $dp[i]$ 是准考证号到第 $i$ 位的方案数

错误思路: $dp[i] = dp[i-1]*(当前位可选方案数)-(选了第i位重复的方案)$

简单的想重复的方案就是最后一段是不吉利数字,就有 $dp[i-m]$

但就如样例所示的不吉利数字如果是 111

在 $dp[i-1]$ 已经排除了 $i-1$ 位是 1, $i-2$ 位是 1 的情况

转移 $dp[i]$ 要在 $dp[i-1]$ 的基础上做排除, 就会重复排除 $i-1$ 位是 1, $i-2$ 位是 1 的情况

所以这时候就要考虑 KMP

设 $dp[i][j]$ 是到第 $i$ 位准考证, **正在**匹配不吉利数字的第 $j$ 位 时候的方案数

对于下一位加进来的数字,就可以根据 KMP 转移 $j$

- 如果匹配 $dp[i][j] -> dp[i+1][j+1]$
- 如果不匹配 $dp[i][j] -> dp[i+1][用KMP匹配找到某next?]$

最后答案要求不能完全匹配不吉利数字

设不吉利数组是 $[0, m)$ 那么 $j == m$ 就是全匹配了

$res=\sum\limits_{i=0}^{m-1}dp[n][i]$

因为 $n$ 很大，所以考虑 矩阵快速幂

预处理出一个数组

$g[i][k]$ 表示从不吉利数字匹配到 $i$ 位转移到第 $k$ 位的方案

对于不吉利数字每一位 $i$ 枚举新加入的数字 $0 -9$ 用KMP匹配出 $k$

此时状态转移方程为 $dp[i][j] = \sum\limits_{k=0}^{m-1}dp[i-1][k]\times g[k][j]$

构造矩阵

$$
\begin{bmatrix}  
f[i][0] & f[i][1] & \cdots & f[i][m]
\end{bmatrix}
\times
\begin{bmatrix}  
g[0][0] & g[0][1] & \cdots & g[0][m] \\
g[1][0] & g[1][1] & \cdots & g[1][m] \\
\vdots & \vdots & \ddots & \vdots \\
g[m][0] & g[m][1] & \cdots & g[m][m]
\end{bmatrix}
=
\begin{bmatrix}  
f[i+1][0] & f[i+1][1] & \cdots & f[i+1][m]
\end{bmatrix}
$$

**气哭,渲染错误,上图**
<img src="https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/kmp_and_dp/matrix.jpg " data-action="zoom">

$F_0\times G^n=F_n$

初始状态 $f[0][0]=1$

$F_n[\cdots]=G^n[0][\cdots]$

## 代码
{% spoiler "代码" %}
```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 30;
const int INF = 0x3f3f3f3f;
const double eps = 1e-7;
const double PI = acos(-1);
typedef pair<int, int> pii;

int MOD = 998244353;

template <typename T>
struct Martix {
    int n, m;
    T a[N][N];
    Martix(){}
    Martix(const int &_n) : n(_n), m(_n) { init(); }
    Martix(const int &_n, const int &_m) : n(_n), m(_m) { init(); }
    T* operator [] (const int &i) { return a[i]; }
    void init(const int &tag = 0) {
        for (int i = 0; i < n; ++i) memset(a[i], 0, sizeof(T)*(n+1));
        for (int i = 0; i < n; ++i) a[i][i] = tag;
    }
    friend Martix operator * (const Martix &m1, const Martix &m2) {
        Martix res(m1.n, m2.m);
        for (int i = 0; i < res.n; ++i)
            for (int j = 0; j < res.m; ++j)
                for (int k = 0; k < m1.m; ++k)
                    res.a[i][j] = (res.a[i][j]+m1.a[i][k]*m2.a[k][j])%MOD;
        return res;
    }
    Martix& operator *= (const Martix &mx) { return *this = *this*mx; }
    template <typename TT>
    Martix pow(const TT &p) const {
        Martix res(n, m), a = *this;
        res.init(1);
        for (TT i = p; i; i >>= 1, a *= a) if (i&1) res *= a;
        return res;
    }
    friend ostream& operator << (ostream &os, Martix<T> &mx) {
        for (int i = 0; i < mx.n; ++i)
            for (int j = 0; j < mx.m; ++j)
                os << mx[i][j] << " \n"[j==mx.m-1];
        return os;
    }
};

int n, m;
int nex[N];
string s;
Martix<int> g;

inline void get_next()
{
    nex[0] = nex[1] = 0;
    for (int i = 1, j = 0; i < (int)s.size(); ++i) {
        while (j && s[i] != s[j]) j = nex[j];
        nex[i+1] = s[i] == s[j] ? ++j : 0;
    }
}

inline void kmp()
{
    for (int i = 0; i < m; ++i) {
        for (int j = 0, k; j <= 9; ++j) {
            for (k = i; k && j != s[k]-'0'; k = nex[k]) {}
            if (j == s[k]-'0') ++k;
            ++g[i][k];
        }
    }
}

signed main()
{
    ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
    cin >> n >> m >> MOD >> s;
    g.n = g.m = m+1;
    get_next();
    kmp();
    g = g.pow(n);
    int res = 0;
    for (int i = 0; i < m; ++i) (res += g[0][i]) %= MOD;
    cout << res << endl;
    return 0;
}
```

{% endspoiler %}

# 例二

[leetcode P3193 [HNOI2008]GT考试](https://leetcode-cn.com/problems/find-all-good-strings/)

这题与上题不同的地方主要就是有上限下限限制

一般的解决技巧就是 [0, 上限] - [0, 下限)

转化成只有上限

设 $dp[i][j][k]$ 是枚举到第 $i$ 位正在匹配 $evil[j]$ , $k$ 表示是否达到上限

因为对每一位的字母有要求,所以我们修改一下 $g$ 数组

令 $g[i][j]$ 表示匹配 $evil[i]$ 时新加的字母是 $j$ 转移到的下标

$dp$ 状态转移应该就是经典的数位dp转移 ~~(其实是懒得写了)~~

tips: 因为难以得到 下限-1, 所以用 [0, 上限]-[0, 下限]+(下限是好字符串就减多了)

## 代码
{% spoiler "代码" %}
```cpp
class Solution {
public:
    int findGoodStrings(int n, string s1, string s2, string evil) {
        const int MOD = 1e9+7;
        int m = evil.size();
        int nex[n+3];
        int g[m+3][30];
        int dp[n+3][m+3][2];
        memset(g, 0, sizeof g);
        memset(nex, 0, sizeof nex);
        // get_next
        nex[0] = nex[1] = 0;
        for (int i = 1, j = 0; i < m; ++i) {
            while (j && evil[i] != evil[j]) j = nex[j];
            nex[i+1] = evil[i] == evil[j] ? ++j : 0;
        }
        // use 'a'+j to kmp i'th evil to the pos
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < 26; ++j) {
                if (evil[i]-'a' == j) g[i][j] = i+1;
                else g[i][j] = g[nex[i]][j];
            }
        }

        function<int(const string&)> calc = [&](const string &s) -> int {
            memset(dp, 0, sizeof dp);
            dp[0][0][1] = 1;
            for (int i = 1; i <= n; ++i) {
                for (int j = 0; j < m; ++j) {
                    for (int k = 0; k < 26; ++k) {
                        // 前面小了这一位可以选任意的
                        (dp[i][g[j][k]][0] += dp[i-1][j][0]) %= MOD;
                        // 前面相等,这一位要小于 s[i-1]
                        if (k < s[i-1]-'a') (dp[i][g[j][k]][0] += dp[i-1][j][1]) %= MOD;
                    }
                    // 上限的情况
                    (dp[i][g[j][s[i-1]-'a']][1] += dp[i-1][j][1]) %= MOD;
                }
            }
            int res = 0;
            for (int i = 0; i < m; ++i) (res += (dp[n][i][0]+dp[n][i][1])%MOD) %= MOD;
            return res;
        };

        int flag = s1.find(evil) == s2.npos;
        return (calc(s2)-calc(s1)+flag+MOD)%MOD;
    }
};
```

{% endspoiler %}