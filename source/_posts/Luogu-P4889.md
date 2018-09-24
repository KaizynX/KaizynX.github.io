---
title: kls与flag
date: 2018-09-24 13:45:00
categories:
  - Luogu
tags:
  - 数学
  - 离散化
  - 排列组合
---

# [Luogu P4889 kls与flag](https://www.luogu.org/problemnew/show/P4889)

## 思路

思路总是从暴力开始

- 暴力做法30分

    两两枚举

    条件是:

    两杆的高度的**和**或者**差**等于两杆的坐标距离

- 70分

    由数据范围可以猜测 $O(logN)$

    设第 $i$ 根杆的高度为 $h_i$

    每根杆子可以向左向右倒

    向左倒尖端坐标 $i-h_i$

    向右倒尖端坐标 $i+h_i$

    也就是说在某个坐标如果有两根能倒到这里

    就满足条件

    如果有多个点(设有 $k$ 个)能倒到这里

    那么就两两组合,共 $\frac{ (1+k) \times k}{2}$ 种

- 100分

    还是在70分的基础上

    发现数据范围太大了

    不如**离散化**

    记录每根杆能倒到的坐标

    共有 $2n$ 个

    排序后,就能轻易找到相同坐标点上的杆子个数

## 代码

```cpp
#include <cstdio>
#include <iostream>
#include <algorithm>

using namespace std;

const int Maxn = 2e5+7;

int n, m, tot;
long long ans, a[Maxn<<1];

int main()
{
    scanf("%d%d", &n, &m);
    for(int i = 1, x; i <= n; ++i)
    {
        scanf("%d", &x);
        a[++tot] = i-x;
        a[++tot] = i+x;
    }
    sort(a+1, a+1+tot);
    int cur = 1, cnt = 0;
    while(cur <= tot)
    {
        cnt = 0;
        while(cur+1 <= tot && a[cur+1] == a[cur])
        {
            ++cur;
            ++cnt;
        }
        ans += 1ll*cnt*(cnt+1)/2;
        ++cur;
    }
    printf("%lld\n", ans);
    return 0;
}
```