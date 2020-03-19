---
title: Candy Distribution Again
date: 2018-09-16 09:16:00
categories:
  - AtCoder
tags:
  - 数学

mathjax: true
---

只写出第一道签到题目

---

## 题意

有N个人,x个物品分成N份

当且仅当第i人分到恰好ai个这个人才会高兴

问最多几个高兴

2 <= N <= 100
1 <= x <= 10^9
1 <= ai <= 10^9

## 样例

**输入**

    3 70
    20 30 10

**输出**

    2

**解释**

One optimal way to distribute sweets is (20,30,20).

## 思路

因为恰好,所以想到一个贪心的策略

首先满足需求小的,即ai小的

**证明**

设ai < aj, aj = ai+k

如果满足j花去aj个物品

则一定存在方案满足i且多出k个物品来满足下一个人

保证了决策的最优性

**证毕**

所以按ai按升序排序

按顺序判断剩余物品能否满足

需要注意的就是最后一个人

如果剩下的物品>an,是不行的

感觉这题的坑点就是N<=100的范围让我始终不敢写啊

```cpp
#include <cstdio>
#include <iostream>
#include <algorithm>

using namespace std;

const int Maxn = 1e2+7;

int n, x, ans;
int a[Maxn];

int main()
{
    scanf("%d%d", &n, &x);
    for(int i = 0; i < n; ++i)
        scanf("%d", a+i);
    sort(a, a+n);
    for(int i = 0; i < n && x >= a[i]; ++i)
    {
        x -= a[i];
        // fit the last one
        if(i != n-1 || x == 0) ans++;
    }
    printf("%d\n", ans);
    return 0;
}
```

