---
title: CF1246A. p-binary
date: 2019-10-29 14:10:00
categories:
  - Codeforces
tags:
  - 数学
  - 位运算

mathjax: true
---

## [题面](https://codeforces.com/contest/1246/problem/A)

## 思路

如果去掉 $p$ 的影响, 就可以用二进制的目光看待 $n$

最少的数量, $n$ 的二进制每个 $1$ 都用特定的组成

而一个高位的 $1$ 可以拆成两个低位的 $1$

因此最多全部拆成 $2^0$ ,最多由 $n$ 个

因此范围其实很大...

分析可得答案不大,不妨枚举答案个数,此时考虑 $n-p*res$ 即可

## 代码
当时没想开,暴力拆分了(逃
```cpp
#include <bits/stdc++.h>
 
using namespace std;
 
const int N = 1e5+7;
 
int n, p;
long long pow2[32];
 
inline pair<long long, long long> count(int a)
{
    pair<long long, long long> res = { 0, 0 };
    for (int i = 0; i <= 30; ++i) {
        if ((a>>i)&1) {
            ++res.first;
            res.second += pow2[i];
        }
    }
    return res;
}
 
int main()
{
    pow2[0] = 1;
    for (int i = 1; i <= 30; ++i) pow2[i] = pow2[i-1]<<1;
    cin >> n >> p;
    // n-p*res > 0
    for (int x = 1; n-x*p > 0; ++x) {
        pair<long long, long long> tmp = count(n-x*p);
        if (x >= tmp.first && x <= tmp.second) {
            cout << x << endl;
            return 0;
        }
    }
    cout << -1 << endl;
    return 0;
}
```