---
title: abc160F - Distributing Integers
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2020-03-30 08:30:00
categories:
  - AtCoder
tags:
  - 换根DP
  - 排列组合
description: 一道略有难度的换根DP题
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/Atcoder-ABC160F/cover.jpg 
---

## 题意

[题目链接](https://atcoder.jp/contests/abc160/tasks/abc160_f)

给一个树的结点标序号，每次只能标相邻的点

问从每一个结点开始标，可行方案数

## 思路

对于一个结点 u 和他的子节点 vi

单考虑 vi 为根的子树，就相当于 [1, size(vi)] 的编号分配给这颗树的方案数

那么假设 vi 子树的方案数算好了, 更新 u 的时候, 要先分配给 vi size(vi)个编号

因为把分配给 vi 的编号离散化,就等价于 [1, size(vi)]

第一个子节点分配方案 C(size(u)-1, size(v1)), 第二个 C(size(u)-1-size(v1), size(v2)) 以此类推

-1 是 u 结点本身肯定是先编号的

然后就应该想到是换根DP了

换根之后(第二遍dfs)就感觉有点麻烦了

考虑原先父节点 u 对子节点 v 的影响

设这个影响值为 val, 就是 u 所有子节点的累计答案(就是第一遍下来的答案)去掉 v 的贡献再加上 u 父亲的贡献

之前计算 v 对 u 贡献时 C(size(u)-1, size(v1)), C(size(u)-1-size(v1), size(v2))...

因为是乘法，所以组合数可以提取出来，(因此搜索子节点的顺序不影响答案)

我们考虑去掉了子节点 v, 那么剩下的就是 size(u)-size(v) 给排列组合，相当于 C(size(u)-1-size(v1), size(v2)) 之后等等

所以我们去掉 v 贡献时, 去掉 C(size(u)-1, size(v))*dp[u][v] 即可

那么 u 父亲的贡献计算，u 父亲的大小就是 n-size(u)

先考虑 u 对 v 贡献，也就是 v 又分配给 u 结点 n-size(u) 个序号

同上，如果我们先考虑把序号分给 u 父亲，那么就不会对其他产生影响

## [代码](https://atcoder.jp/contests/abc160/submissions/11299402)

赛场上打的，有点乱

{% spoiler "代码" %}
```cpp
/*
 * @Author: Kaizyn
 * @Date: 2020-03-28 19:32:21
 * @LastEditTime: 2020-03-28 20:48:49
 */
#include <bits/stdc++.h>

// #define DEBUG

using namespace std;

const int N = 2e5+7;
const int MOD = 1e9+7;
const double eps = 1e-7;
const int INF = 0x3f3f3f3f;
typedef pair<int, int> pii;

template <int _MOD> struct Mint
{
    int v = 0;
    Mint() {}
    Mint(int _v) : v((_v%_MOD+_MOD)%_MOD) {}
    Mint(long long _v) : v(static_cast<int>((_v%_MOD+_MOD)%_MOD)) {}
    Mint operator = (const int &_v) { return *this = Mint(_v); }
    Mint operator = (const long long &_v) { return *this = Mint(_v); }
    bool operator ! () const { return !this->v; }
    bool operator < (const Mint &b) const { return v < b.v; }
    bool operator > (const Mint &b) const { return v > b.v; }
    bool operator == (const Mint &b) const { return v == b.v; }
    bool operator != (const Mint &b) const { return v != b.v; }
    bool operator <= (const Mint &b) const { return v < b.v || v == b.v; }
    bool operator >= (const Mint &b) const { return v > b.v || v == b.v; }
    Mint operator + (const Mint &b) const { return Mint(v+b.v); }
    Mint operator - (const Mint &b) const { return Mint(v-b.v); }
    Mint operator * (const Mint &b) const { return Mint(1ll*v*b.v); }
    Mint operator / (const Mint &b) const { return Mint(b.inv()*v); }
    Mint& operator += (const Mint &b) { return *this = *this+b; }
    Mint& operator -= (const Mint &b) { return *this = *this-b; }
    Mint& operator *= (const Mint &b) { return *this = *this*b; }
    Mint& operator /= (const Mint &b) { return *this = *this/b; }
    Mint operator - () const { return Mint(-v); }
    Mint& operator ++ () { return *this += 1; }
    Mint& operator -- () { return *this -= 1; }
    Mint operator ++ (int) { Mint tmp = *this; *this += 1; return tmp; }
    Mint operator -- (int) { Mint tmp = *this; *this -= 1; return tmp; }
    Mint pow(int p) const {
        Mint res(1), x(*this);
        while (p) {
            if (p&1) res = res*x;
            x *= x;
            p >>= 1;
        }
        return res;
    }
    Mint inv() const { return pow(_MOD-2); }
    friend istream& operator >> (istream &is, Mint &mt) { return is >> mt.v; }
    friend ostream& operator << (ostream &os, const Mint &mt) { return os << mt.v; }
};
using mint = Mint<MOD>;

int n;
int num[N];
mint fac[N], inv[N], res[N];
vector<int> e[N];
vector<mint> w[N];

inline mint C(const int &p, const int &q)
{
    return fac[p]*inv[q]*inv[p-q];
}

int dfs1(const int &u, const int &fa)
{
    num[u] = 0;
    for (int i = 0, v; i < (int)e[u].size(); ++i) {
        v = e[u][i];
        if (v == fa) continue;
        dfs1(v, u);
        num[u] += num[v];
    }
    for (int i = 0, v, sum = num[u]; i < (int)e[u].size(); ++i) {
        v = e[u][i];
        if (v == fa) continue;
        w[u][i] = C(sum, num[v])*res[v];
        res[u] *= w[u][i];
        sum -= num[v];
    }
    return ++num[u]; // itself
}

void dfs2(const int &u, const int &fa, const mint &val)
{
    for (int i = 0, v; i < (int)e[u].size(); ++i) {
        v = e[u][i];
        if (v == fa) continue;
        mint tmp = res[u]/(C(num[u]-1, num[v])*res[v]);
        tmp *= val*C(n-num[v]-1, n-num[u]);
        dfs2(v, u, tmp);
    }
    res[u] *= C(n-1, n-num[u])*val;
}

signed main()
{
    ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
    cin >> n;
    fac[0] = inv[0] = 1;
    for (int i = 1; i <= n; ++i) {
        fac[i] = fac[i-1]*i;
        inv[i] = fac[i].inv();
    }
    for (int i = 1, u, v; i < n; ++i) {
        cin >> u >> v;
        e[u].emplace_back(v);
        e[v].emplace_back(u);
    }
    for (int i = 1; i <= n; ++i) {
        w[i].resize(e[i].size(), 0);
        res[i] = 1;
    }
    dfs1(1, 0);
    dfs2(1, 0, 1);
    for (int i = 1; i <= n; ++i) cout << res[i] << endl;
    return 0;
}
```
{% endspoiler %}