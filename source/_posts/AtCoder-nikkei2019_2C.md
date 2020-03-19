---
title: AtCoder - Swaps
date: 2019-11-10 19:15:00
categories:
  - AtCoder
tags:
  - 排序

mathjax : true
---
## [题面](https://atcoder.jp/contests/nikkei2019-2-qual/tasks/nikkei2019_2_qual_c)

## 思路

保持 $a, b$ 相对位置不变,把 $b$ 排序

用 **选择排序** 可以 $n-1$ 次操作把 $a$ 排为升序

显然这样的配对是最优的,如果这都不满足条件,那就真的不行了

再看题目要求 $n-2$ 次,那么就当少交换一次,仅有两个数位置不对

设位置不对的是 $a_i, a_j(i < j)$ (排好的情况)

现在少交换了一次,变成 $a_j$ 对 $b_i$, $a_i$ 对 $b_j$

因为 $a_i \leq b_i, a_j \leq b_j$

所以 $a_i \leq b_j$, 只需满足 $a_j \leq b_i$

所以最保险的方案是 $j == i+1$

如果这能满足,那肯定可以

之后呢,一定不行了吗

不,如果排序不需要 $n-1$ 次也可

什么情况下排序不需要 $n-1$ 次?

由样例一得 某个数字排序前后位置没改变

(这么想其实还不大对)

正确思路是, 选择排序得时候, 对于$a_i$, 它现在位置为 $i$, 排序后为 $j$

那么就要交换 $a_i, a_j$, 再把 $a_j$ 和目标位置交换

会发现这样得交换形成了一个环

设一个环的大小为 $m$, 则需要 $m-1$ 次交换

所以如果这样的交换环存在两个及以上,则满足

## 代码
```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1e5+7;

int n, m;
int bb[N], d[N<<1], to[N];

struct Node 
{
    int a, b, id;
} p[N];

inline bool cmpa(const Node &x, const Node &y) { return x.a < y.a; }
inline bool cmpb(const Node &x, const Node &y) { return x.b < y.b; }

int main()
{
    cin >> n;
    for (int i = 1; i <= n; ++i) cin >> p[i].a, d[i] = p[i].a;
    for (int i = 1; i <= n; ++i) cin >> p[i].b, d[n+i] = p[i].b;
    sort(d+1, d+n*2+1);
    m = unique(d+1, d+n*2+1)-d-1;
    for (int i = 1; i <= n; ++i) {
        p[i].a = lower_bound(d+1, d+m+1, p[i].a)-d;
        p[i].b = lower_bound(d+1, d+m+1, p[i].b)-d;
    }
    sort(p+1, p+n+1, cmpb);
    for (int i = 1; i <= n; ++i) {
        bb[i] = p[i].b;
        p[i].id = i;
    }
    sort(p+1, p+n+1, cmpa);
    for (int i = 1; i <= n; ++i) {
        if (p[i].a > bb[i]) {
            cout << "No" << endl;
            return 0;
        }
        to[p[i].id] = i;
    }
    for (int i = 1; i < n; ++i) {
        if (p[i+1].a <= bb[i]) {
            cout << "Yes" << endl;
            return 0;
        }
    }
    int sz = 0, now = 1;
    do {
        ++sz;
        now = to[now];
    } while (now != 1);
    if (sz == n) cout << "No" << endl;
    else cout << "Yes" << endl;
    return 0;
}
```