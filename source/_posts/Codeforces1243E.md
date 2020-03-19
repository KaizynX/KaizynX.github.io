---
title: CF1243E. Sum Balance
date: 2019-11-11 20:30:00
categories:
  - Codeforces
tags:
  - 数学

mathjax: true
---

## [题面](https://codeforces.com/contest/1243/problem/E)

## 思路

这题最重要的信息是 **每个数字 $a$ 都不相同**

首先容易得出每行的和应该要为 $(\sum {a_{ij}})/k$

如果不能整除,直接不行了

记第 $i$ 行的和为 $sum_i$, $(\sum{a_{ij}})/k = need$ 

要使第 $i$ 行满足要求,设拿走的是 $a_{ij}$

那么 $sum_i-a_{ij}+takein == need$

$takein = a_{ij}-(sum_i-need)$

那么找到 $takein$ 是否存在于某一行

那么那一行去掉的就是 $takein$, 用同样的方法求出这行要拿进来的

那么如果能回到最初的 $a_{ij}$,则可行

也就是形成一个环,因为 **每个数字都不相同**,所以一个拿走要拿来的只会一一对应

也就是这些环是独立不交叉的

那么链不行,环上有多个点同一行不行

找出一些环把所有行都恰好覆盖一次就是可行方案

找环用暴力

求方案用状压dp

[O(3^k)](https://cp-algorithms.com/algebra/all-submasks.html)

## 代码
```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 5e3+7;
const int K = 15;

int k, dfn;
int n[K], p[K], c[K], a[K][N];
long long all;
long long sum[K];
vector<pair<int, int>> dp[1<<K];
map<long long, int> mp;

inline void check(int col, int val) 
{
    int i = col, v = val, state = 1<<col;
    long long target;
    vector<pair<int, int>> tmp;

    while (true) {
        target = v-sum[i];
        if (!mp.count(target)) return;
        v = static_cast<int>(target);
        tmp.emplace_back(v, i);
        i = mp[target];
        if (v == val) break;
        if (state&(1<<i)) return;
        state |= 1<<i;
    }
    dp[state] = tmp;
}

int main()
{
    cin >> k;
    for (int i = 0; i < k; ++i) {
        cin >> n[i];
        for (int j = 0; j < n[i]; ++j) {
            cin >> a[i][j];
            sum[i] += a[i][j];
            mp.insert({a[i][j], i});
        }
        all += sum[i];
    }
    if (all%k) { cout << "No" << endl; return 0; }
    all /= k;
    for (int i = 0; i < k; ++i) sum[i] -= all;

    for (int i = 0; i < k; ++i) {
        for (int j = 0; j < n[i]; ++j) {
            check(i, a[i][j]);
        }
    }
    for (int i = 1; i < (1<<k); ++i) {
        if (dp[i].size()) continue;
        for (int j = i; j; j = (j-1)&i) {
            if (dp[j].size() && dp[i-j].size()) {
                dp[i] = dp[i-j];
                dp[i].insert(dp[i].end(), dp[j].begin(), dp[j].end());
            }
        }
    }
    if (dp[(1<<k)-1].empty()) cout << "No" << endl;
    else {
        cout << "Yes" << endl;
        for (auto pr : dp[(1<<k)-1]) {
            c[mp[pr.first]] = pr.first;
            p[mp[pr.first]] = pr.second;
        }
        for (int i = 0; i < k; ++i) {
            cout << c[i] << " " << p[i]+1 << endl;
        }
    }
    return 0;
}
```