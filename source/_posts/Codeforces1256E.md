---
title: CF1256E. Yet Another Division Into Teams
date: 2019-11-05 19:35:00
categories:
  - Codeforces
tags:
  - 动态规划
  - 数学

mathjax: true
---
## [题面](https://codeforces.com/contest/1256/problem/E)

## 思路

首先显然要前排序,相邻的放一组

一组的人数是最少 3 人(题目要求), 最多 5 人,因为 6 人就可以分成两组

显然易证, 设有 $a_1, a_2 ... a_n$ 这组的值是 $a_n-a_1$

分成两组 $a_1, a_2 ... a_i$ 和 $a_{i+1}, a_{i+2} ... a_n$ 值为 $a_n-a-{i+1}+a_i-a_1$

显然同样长度分组越多,值越小

然后就动规呗

$dp[i] = \min\limits_{i-5 \leq j \leq i-3} dp[j]+a[i].v-a[j+1].v$

把 $[j+1, i]$ 分成一组

## 代码
```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 2e5+7;
const int INF = 0x3f3f3f3f;

int n;
int t[N], dp[N], last[N];

struct Node
{
    int v, id;
    friend bool operator < (const Node &x, const Node &y) {
        return x.v < y.v;
    }
} a[N];

int main()
{
    cin >> n;
    memset(dp, 0x3f, sizeof dp);
    for (int i = 1; i <= n; ++i) {
        cin >> a[i].v;
        a[i].id = i;
    }
    sort(a+1, a+n+1);
    dp[0] = 0;
    for (int i = 1; i <= n; ++i) {
        for (int j = max(0, i-5); j <= i-3; ++j) {
            // dp[i] = min(dp[i], dp[j]+a[i].v-a[j+1].v);
            if (dp[i] > dp[j]+a[i].v-a[j+1].v) {
                dp[i] = dp[j]+a[i].v-a[j+1].v;
                last[i] = j;
            }
        }
    }
    int tot = 0;
    for (int i = n; i; i = last[i]) {
        ++tot;
        for (int k = i; k > last[i]; --k) {
            t[a[k].id] = tot;
        }
    }

    cout << dp[n] << " " << tot << endl;
    for (int i = 1; i <= n; ++i) {
        cout << (t[i] ? t[i] : 1) << " \n"[i==n];
    }
    return 0;
}
```