---
title: Portal1
date: 2018-10-05 14:00:00
categories:
  - Luogu
tags:
  - DP
---

# [Luogu U41568 Portal1](https://www.luogu.org/problemnew/show/U41569)

来自比赛 [NOIP提高组模拟赛Day1](https://www.luogu.org/contest/show?tid=11101) B题

## 思路

大背景应该是背包问题

再加上各色各样的限制条件还要输出路径

我们假设 dp[k] 表示在时间 k 可以得到的最大资源

对于某个物品 i , 在 D[i] 时刻就不能用了

所以状态更新的范围,也就是时间 t 的范围是 [1, D[i]-1]

由背包问题可以~~显然~~得出 `dp[i] = max(dp[i], dp[i-T[i]]+C[i]`

只不过多了个条件 D[i] 嘛

至于顺序嘛(题目错了,不是按照编号,是按照Hack的顺序)

~~显然~~按照 D[i] 从小到大输出

## 代码
```cpp
#include <bits/stdc++.h>

using namespace std;

const int Maxn = 107;
const int Maxt = 25;
const int Maxd = 2e3+7;

int n, D, cnt;
int dp[Maxd], rk[Maxn], res[Maxn];
bitset<Maxn> choose[Maxd];

struct Portal
{
    int t, d, c, id;
    bool operator < (const Portal &b) const
    {
        return d < b.d;
    }
} a[Maxn];

bool cmp(int x, int y)
{
    return rk[x] < rk[y];
}

int main()
{
    scanf("%d", &n);
    for(int i = 1; i <= n; ++i)
    {
        scanf("%d%d%d", &a[i].t, &a[i].d, &a[i].c);
        a[i].id = i;
        D = max(D, a[i].d);
    }
    sort(a+1, a+n+1);
    for(int i = 1; i <= n; ++i)
    {
        rk[a[i].id] = i;
        // the time finish it
        for(int j = a[i].d-1; j >= a[i].t; --j)
        {
            // dp[j] = max(dp[j], dp[j-a[i].t]+a[i].c);
            int tmp = dp[j-a[i].t]+a[i].c;
            if(tmp > dp[j])
            {
                dp[j] = tmp;
                choose[j] = choose[j-a[i].t];
                choose[j][a[i].id] = 1;
            }
        }
    }
    int ans = 0;
    for(int i = 1; i < D; ++i)
        if(dp[ans] < dp[i])
            ans = i;
    printf("%d\n%d\n", dp[ans], choose[ans].count());
    if(choose[ans].count() == 0) return 0;
    for(int i = 1; i <= n; ++i)
        if(choose[ans][i])
            res[++cnt] = i;
    sort(res+1, res+cnt+1, cmp);
    for(int i = 1; i < cnt; ++i)
        printf("%d ", res[i]);
    printf("%d\n", res[cnt]);
    return 0;
}
```