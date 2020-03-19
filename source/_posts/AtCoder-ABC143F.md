---
title: ABC143F - Distinct Numbers
date: 2019-10-29 13:50:00
categories:
  - AtCoder
tags:
  - 二分
  - 贪心

mathjax: true
---

## [题面](https://atcoder.jp/contests/abc143/tasks/abc143_f)

给 $N$ 堆卡片,每堆有 $A_i$ 个

选择一个 $K$ 

每次从任意 $K$ 堆中各拿一张

问 $K=1,2...N$ 最多能拿几张

### 数据范围

$1 \leq N \leq 3 \times 10^5$

$1 \leq A_i \leq N$

## 思路

二分答案,判断

设最多可以拿 $mid$ 次

对于 $Ai \geq mid$ 可以每次都拿

对于 $Ai < mid$ 策略贪心易得 每次从最多的几堆取

因为不够取 $mid$ 次,所以肯定会取完并由其他堆顶替

当有一堆取完了,那么取完之前这堆有 $1$ 个,那么替补的里面最大的也是 $1$,所以保证最大效率取

即能取 $\lfloor \frac{sum}{K} \rfloor$ 次

## 代码
```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 3e5+7;

int n, m;
int cnt[N], sum[N];

int main()
{
    cin >> n;
    for (int i = 1, a; i <= n; ++i) {
        cin >> a;
        ++cnt[a];
    }
    sort(cnt+1, cnt+N);
    reverse(cnt+1, cnt+N);
    m = 1;
    while (cnt[m+1]) ++m;
    reverse(cnt+1, cnt+m+1);
    for (int i = 1; i <= m; ++i)
        sum[i] = sum[i-1]+cnt[i];

    for (int k = 1, l, r, mid; k <= n; ++k) {
        if (k > m) {
            cout << 0 << endl;
            continue;
        }
        l = 0; r = n;
        while (l < r) {
            mid = (l+r+1)>>1;
            int p = lower_bound(cnt+1, cnt+m+1, mid)-cnt-1;
            if (m-p >= k || sum[p]/(k-m+p) >= mid) l = mid;
            else r = mid-1;
        }
        cout << l << endl;
    }
    return 0;
}
```
