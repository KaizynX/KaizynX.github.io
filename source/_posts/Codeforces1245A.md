---
title: CF1245A. Good ol' Numbers Coloring
date: 2019-11-05 18:50:00
categories:
  - Codeforces
tags:
  - 数学
  - 扩展欧几里得

mathjax: true
---

## [题面](https://codeforces.com/contest/1245/problem/A)

## 思路

由**扩展欧几里得**可得

$ax+by=c$

其中 $c \% \gcd(a,b) == 0$

易得,如果 $\gcd(a,b) == 1$ 那么就能把后面填满

由于 $x \geq 0, y \geq 0$, 所以前面有几个不能填

总之互素就是有限

## 代码
```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1e2+7;

int t, a, b;

int main()
{
    cin >> t;
    while (t--) {
        cin >> a >> b;
        cout << (__gcd(a, b) == 1 ? "Finite" : "Infinite") << endl;
    }
    return 0;
}
```