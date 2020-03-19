---
title: CF1256D. Binary String Minimizing
date: 2019-11-05 19:30:00
categories:
  - Codeforces
tags:
  - 贪心

---
## [题面](https://codeforces.com/contest/1256/problem/D)

## 思路

贪心策略,先把前面的 0 移到前面 (显然最优!!!不证了)

把某个 0 移到前面的代价是 这个 0 前面 1 的个数

## 代码
```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1e6+7;

int q, n;
long long k;
string str;

int main()
{
    cin >> q;
    while (q--) {
        cin >> n >> k >> str;
        int j = 0;
        for (int i = 0, cnt = 0; i < n; ++i) {
            if (str[i] == '1') {
                ++cnt;
            } else {
                if (k >= cnt) {
                    k -= cnt;
                    ++j;
                    putchar('0');
                } else {
                    for ( ; j < i-k; ++j) putchar('1');
                    putchar('0');
                    for (j = i-k+1; j <= i; ++j) putchar('1');
                    for (j = i+1; j < n; ++j) putchar(str[j]);
                    break;
                }
            }
        }
        for ( ; j < n; ++j) putchar('1');
        putchar('\n');
    }
    return 0;
}
```