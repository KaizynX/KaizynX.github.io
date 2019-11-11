---
title: CF1243C. Tile Painting
date: 2019-11-11 19:55:00
categories:
  - Codeforces
tags:
  - 扩展欧几里得
  - 数学

mathjax: true
---
## [题面](https://codeforces.com/contest/1243/problem/C)

## 思路

扩展欧几里得

$ax+by=c$ , $c$ 是 $\gcd(a, b)$ 的倍数

那么两个因子为 $a, b$ 则能覆盖 $\gcd(a,b)$ 倍数的地方

如果在 $n$ 范围内有 $ax == by$ 那么这个相交的块就要涂成一种颜色

也就是 $a, b$ 的块都是一种颜色

所以当且仅当 $n$ 分解质因数只有一个素数时,答案为这个素数

否则为 $1$

## 代码
```cpp
#include <bits/stdc++.h>

using namespace std;

int cnt;
long long n, p;

int main()
{
    cin >> n;
    if (n == 1) {
        cout << 1 << endl;
        return 0;
    }
    for (long long i = 2; i*i <= n; ++i) {
        if (n%i) continue;
        p = i;
        if (++cnt > 1) {
            cout << 1 << endl;
            return 0;
        }
        while (n%i == 0) n /= i;
    }
    if (n > 1) ++cnt, p = n;
    if (cnt > 1) cout << 1 << endl;
    else cout << p << endl;
    return 0;
}
```