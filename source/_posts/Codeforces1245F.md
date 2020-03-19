---
title: CF1245F. Daniel and Spring Cleaning
date: 2019-11-05 19:10:00
categories:
  - Codeforces
tags:
  - 数学
  - 位运算
  - DP

mathjax: true
---
## [题面](https://codeforces.com/contest/1245/problem/F)

## 思路

用位运算的目光来看

首先找到满足式子的条件

$a + b = a \bigoplus b$

```
  0 0 1 1

  0 1 0 1

+ 0 1 1 10

^ 0 1 1 00
```

可以证明当两个数的某一位都是 $1$ ,无法找到后一位的组合情况使之满足条件

于是每一位都有三个组合情况,没有限制的话就有 $3^n$ 种

于是我们加上限制条件

设函数 $calc(r,l)$ 为第一个数字 $\leq r$ , 第二个数 $\leq l$ 的情况数

易得答案就是 $calc(r,r)-2\times(r, l-1)+calc(l-1, l-1)$

设 $dp[i][j][k]$ 为最高位到第 $i$ 位为止的情况

$j$ 表示第一个数的情况 $0$ 表示这个数小于 $r$, $1$ 表示到这一位为止都等于 $r$

$k$ 表示第二个数的情况,同上

然后一共~~也就~~ $4\times 4 = 16$ 种情况

## 代码
```cpp
#include <bits/stdc++.h>
//#define DEBUG

using namespace std;

long long dp[32][2][2];

long long calc(int r, int l)
{
    // a^b == a+b
    // a <= r, b <= l
    memset(dp, 0, sizeof dp);
    int i = 30;
    while (i && !((r>>i)&1)) --i;
    dp[i+1][1][1] = 1;
    for ( ; i >= 0; --i) {
        if (((r>>i)&1) && ((l>>i)&1)) {
            dp[i][0][0] = 3*dp[i+1][0][0]+2*dp[i+1][0][1]+2*dp[i+1][1][0]+1*dp[i+1][1][1];
            dp[i][0][1] = 0*dp[i+1][0][0]+1*dp[i+1][0][1]+0*dp[i+1][1][0]+1*dp[i+1][1][1];
            dp[i][1][0] = 0*dp[i+1][0][0]+0*dp[i+1][0][1]+1*dp[i+1][1][0]+1*dp[i+1][1][1];
            dp[i][1][1] = 0*dp[i+1][0][0]+0*dp[i+1][0][1]+0*dp[i+1][1][0]+0*dp[i+1][1][1];
        } else if (((r>>i)&1) == 0 && ((l>>i)&1)) {
            dp[i][0][0] = 3*dp[i+1][0][0]+2*dp[i+1][0][1]+0*dp[i+1][1][0]+0*dp[i+1][1][1];
            dp[i][0][1] = 0*dp[i+1][0][0]+1*dp[i+1][0][1]+0*dp[i+1][1][0]+0*dp[i+1][1][1];
            dp[i][1][0] = 0*dp[i+1][0][0]+0*dp[i+1][0][1]+2*dp[i+1][1][0]+1*dp[i+1][1][1];
            dp[i][1][1] = 0*dp[i+1][0][0]+0*dp[i+1][0][1]+0*dp[i+1][1][0]+1*dp[i+1][1][1];
        } else if (((r>>i)&1) && ((l>>i)&1) == 0) {
            dp[i][0][0] = 3*dp[i+1][0][0]+0*dp[i+1][0][1]+2*dp[i+1][1][0]+0*dp[i+1][1][1];
            dp[i][0][1] = 0*dp[i+1][0][0]+2*dp[i+1][0][1]+0*dp[i+1][1][0]+1*dp[i+1][1][1];
            dp[i][1][0] = 0*dp[i+1][0][0]+0*dp[i+1][0][1]+1*dp[i+1][1][0]+0*dp[i+1][1][1];
            dp[i][1][1] = 0*dp[i+1][0][0]+0*dp[i+1][0][1]+0*dp[i+1][1][0]+1*dp[i+1][1][1];
        } else {
            dp[i][0][0] = 3*dp[i+1][0][0]+0*dp[i+1][0][1]+0*dp[i+1][1][0]+0*dp[i+1][1][1];
            dp[i][0][1] = 0*dp[i+1][0][0]+2*dp[i+1][0][1]+0*dp[i+1][1][0]+0*dp[i+1][1][1];
            dp[i][1][0] = 0*dp[i+1][0][0]+0*dp[i+1][0][1]+2*dp[i+1][1][0]+0*dp[i+1][1][1];
            dp[i][1][1] = 0*dp[i+1][0][0]+0*dp[i+1][0][1]+0*dp[i+1][1][0]+1*dp[i+1][1][1];
        }
#ifdef DEBUG
        for (int j = 0; j < 2; ++j) {
            for (int k = 0; k < 2; ++k) {
                printf("dp[%d][%d][%d]=%I64d ", i, j, k, dp[i][j][k]);
            }
        }
        putchar('\n');
#endif
    }
    return dp[0][0][0]+dp[0][0][1]+dp[0][1][0]+dp[0][1][1];
}

int main()
{
    int t, l, r;
    cin >> t;
    while (t--) {
        cin >> l >> r;
#ifdef DEBUG
        cout << calc(r, r) << " " << calc(r, l-1) << " " << calc(l-1, l-1) << endl;
#endif
        if (!l && !r) cout << 1 << endl;
        else if (!l) cout << calc(r, r) << endl;
        else cout << (calc(r, r)-2*calc(r, l-1)+calc(l-1, l-1)) << endl;
    }
    return 0;
}

```