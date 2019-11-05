---
title: AGC040B - Two Contests
date: 2019-11-05 20:00:00
categories:
  - AtCoder
tags:
  - 数学

mathjax: true
---
## [题面](https://atcoder.jp/contests/agc040/tasks/agc040_a)

## 思路

设 $maxL = \max\limits_i^n l_i, minR = \min\limits_i^n r_i$

那么分成两组的时候,答案为 $(minR1-maxL1)+(minR2-maxL2)$

其中一定有 $minR1 == minR || minR2 == minR$, $maxL$ 相同

1. $maxL, minR$ 在同一组

    那么无论加哪个到这一组,这一组的结果都不会改变,即求另一组最大值

    易证 加入一个点,这组的结果只会不变或者缩小

    所以另一组就是 剩下点的最大长度

2. $maxL, minR$ 不是同一组

    那么有这么两组 $[maxL', minR],[maxL, minR']$

    对于第一组,要使左边界为 $maxL'$ 那么比 $maxL'$ 小的点都要加入第二组

    可以得出此时的 $minR'$ ,因为再加入一个点, $minR'$ 只会变小或不变

    不妨把剩下的点都加入第一组

## 代码
```cpp
#include <bits/stdc++.h>
#define DEBUG

using namespace std;

const int N = 1e5+7;
const int INF = 1e9;

int n, res;

struct Node
{
    int l, r;
    friend bool operator < (const Node &x, const Node &y) { return x.l < y.l; }
} a[N];


int main()
{
    cin >> n;
    for (int i = 1; i <= n; ++i) {
        cin >> a[i].l >> a[i].r;
    }
    sort(a+1, a+n+1);
    reverse(a+1, a+n+1);
    int rr = 1;
    for (int i = 1; i <= n; ++i) {
        if (a[i].r < a[rr].r) rr = i;
    }
    if (1 == rr) {
        for (int i = 2; i <= n; ++i) {
            res = max(res, a[i].r-a[i].l+1);
        }
        res += a[1].r-a[1].l+1;
        cout << res << endl;
        return 0;
    }
    int minr = a[1].r;
    for (int i = 2; i <= n; ++i) {
        if (i == rr) continue;
        res = max(res, max(0, a[rr].r-max(a[i].l, a[rr].l)+1)+max(0, minr-a[1].l+1));
        res = max(res, max(0, a[rr].r-a[1].l+1)+a[i].r-a[i].l+1);
        minr = min(minr, a[i].r);
    }
    res = max(res, max(0, minr-a[1].l+1)+a[rr].r-a[rr].l+1);
    cout << res << endl;
    return 0;
}
```