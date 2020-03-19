---
title: Agent1
date: 2018-10-05 13:10:00
categories:
  - Luogu
tags:
  - 数学
  - 排列组合
  - 快速幂
mathjax: true
---

# [Luogu U41568 Agent1](https://www.luogu.org/problemnew/show/U41568)

来自比赛 [NOIP提高组模拟赛Day1](https://www.luogu.org/contest/show?tid=11101) A题

## 思路

因为 $\forall A_i < B_i$

所以我们要选择 A, B

不妨在把 N 划分成左右两个部分

A 从左边选任意个($\geq 1$)

B 同上

具体做法呢,为了防止重复的东西

我们假设中间的是 $i$

且 A 必须选 $i$ ($1 \leq i < N$)

$i$ 的左边还有 i-1 个,从中选择 0~i-1个

所以 A 的方案有 $\sum\limits_{j=0}^{i-1} {C_{i-1}^{j}} = 2^{i-1}$

那么考虑 B

$i$ 的右边还有 n-i 个

因为 B 不为空

所以 B 的方案有 $\sum\limits_{j=1}^{n-i} {C_{n-i}^{j}} = 2^{n-i}-1$

综上, 当中间的是 $i$ 时

方案有 $2^{i-1} \times (2^{n-i}-1) = 2^{n-1}-2^{i-1}$

最终结果为 $\sum\limits_{i=1}^{n-1} (2^{n-1}-2^{i-1})$

$= (n-1)\times 2^{n-1} - \sum\limits_{i=1}^{n-1} 2^{i-1}$

后面那个用等比数列的求和公式可以得到

$\sum\limits_{i=1}^{n-1} 2^{i-1} = 2^0 \times \frac{1-2^{n-1}}{1-2} = 2^{n-1}-1$

所以最终结果为 $(n-2)\times 2^{n-1} + 1$

用快速幂即可求解

时间复杂度 $O(\log n)$

## 代码
```cpp
#include <bits/stdc++.h>

using namespace std;

const int MOD = 1e9+7;

int n;

inline long long quickm(long long a, int p)
{
    long long res = 1;
    while(p)
    {
        if(p&1) res = res*a%MOD;
        p >>= 1;
        a = a*a%MOD;
    }
    return res;
}

int main()
{
    cin >> n;
    cout << (quickm(2, n-1)*(n-2)+1)%MOD << endl;
    return 0;
}
```