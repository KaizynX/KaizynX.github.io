---
title: 珈百璃堕落的开始
date: 2018-09-25 21:10:00
categories:
  - Luogu
tags:
  - 数学
  - DP

mathjax: true
---
# [P4832 珈百璃堕落的开始](https://www.luogu.org/problemnew/show/P4832)

## 题意

给出 $n$ 个由 $s,+,c$ 组成式子

表示 $\sin^2(x)$ 和 $\cos^2(x)$ 的和

其中 $x = \frac{\pi}{7}$

选择其中任意几个式子,使和恰好为整数

求最大整数

## 样例

### 输入

    3
    s+c
    s+c+s
    c

### 输出

    3

### 解释

三个式子全部加起来为 3

## 数据范围

$nm \leq 5\times 10^7$

$m \leq 10^6$

## 思路

建议在做此题前完成以下两题

[P2170 选学霸](https://www.luogu.org/problemnew/show/P2170)

[P1282 多米诺骨牌](https://www.luogu.org/problemnew/show/P1282)

这三题的核心的DP思路类似,难度递增

---

首先肯定是字符串处理,处理成 $s$ 和 $c$ 的个数

继而我们知道 $\sin^2(x) +\cos^2(x) = 1$

那么就是求某些式子的和 $s$ 的个数恰好等于 $c$ ,且最多

这里穿插一句,我想当然是这样,不过题解给出了严谨证明,如下:

证明 $\sin^2(x),\cos^2(x),(x=\frac{\pi}{7})$ 是无理数

设 $\cos(x) = A$

则 $\cos(2x) = 2A^2-1$

则 $\cos(2x) = 8A^4-8A^2+1$

又 $\cos(3x)=4A^3-3A$

所以 $\cos(3x)+\cos(4x)=0$

所以 $8A^4+4A^3-8A^2-3A+1=0$

*下面那句没懂*

根据整系数方程有理根判定定理，该方程没有理根

所以 $\cos^2(x)(x=\frac{\pi}{7}$ 是无理数

同理 $\sin^2(x)(x=\frac{\pi}{7}$ 是无理数

证毕

算了,这似乎并不是很重点

---

类比给出两道题的思路

我们不妨设 $s = 1, c = -1$

每个式子就可以用数值表示

所以要找某些和为0的式子

这是前提,再是找值最大的

这个问题就像给你一些物品和一个值 $V$

要求找一些物品使值得和接近 $V$

就是 $abs(V-sum)$ 最小

分类讨论,一种是比 $V$ 小,接近 $V$

另一种是比 $V$ 大,接近 $V$

这样就有点熟悉了吧

**01背包!!!**

正过来的背包和反过来的背包

正着的是这样的

```cpp
for(int i = 1; i <= n; ++i)
    for(int j = V; j >= minv; --j)
        if(j-v[i] >= minv)
            dp[j] = max(dp[j], dp[j-v[i]]+w[i]);
```

反着也很容易

```cpp
for(int i = 1; i <= n; ++i)
    for(int j = V; j <= maxw; ++j)
        if(j+v[i] <= maxv)
            dp[j] = max(dp[j], dp[j+v[i]]+w[i]);
```

当然如果实在不好理解就开多维背包数组,不要搞什么骚操作

接下来先考虑怎么找到和为整数的

也就是和为0的

也就是 $V =0$

这又需要一些技巧了

我们知道数组下标是不能为负的

那我们不妨**重新定义 0**

假如把 100 定为 0

那么 103 就是 3

98 就是 -2

这样负数的问题就解决了

那么此时能够找到和为整数的式子了

---

上述的就像是

背包只能装 $V$ 体积物品,每个物品体积 $v_i$ 求最多能装几个物品

而我们现在求的是 $V$ 的背包,每个物品有体积 $v_i$ ,价值 $w_i$

求最大价值(加个条件背包装满)

所以我们考虑物品(式子)的状态,不仅要有体积(s,c相对个数),还要有价值

那么某个物品的价值就是 $s$ 和 $c$ 配对的个数

$w_i = \min(s, c)$

然后在状态转移的时候,如果有配对的,那么价值要增加

这里可以分很多类讨论...

这里简单介绍几类

$dp[j]$ 表示当前 $v=j$ 时的最大价值

(只介绍 $j > 0$ 时的状况)

当前式子是 $v_i, w_i$

显然选择这个式子的话价值会加上 $w_i$

1. $v_i > 0$

    显然不会有增加的价值量

2. $v_i < 0$

   就表明有 $s$ 和 $c$ 匹配了,要加上匹配来的价值

    - $j+v_i >0$

        显然新匹配的价值量是 $-v_i$

    - $j+v_i < 0$

        此时匹配后反而 $c$ 更多了

        新匹配的价值量是 $j$
    
大致就是这么三类

$j < 0$ 时类似

---

## 代码

```cpp
#include <cstdio>
#include <cstring>
#include <iostream>

using namespace std;

const int Maxm = 2e6+7;
const int Zero = 1e6;

int n;
char s[Maxm];
int f[Maxm], v[Maxm], c[Maxm];
// f[i] s-c == i max value

int main()
{
    int upv, lov;
    memset(f, -1, sizeof f);
    f[Zero] = 0;
    upv = lov = Zero;

    scanf("%d", &n);
    for(int i = 1, nums, numc; i <= n; ++i)
    {
        nums = numc = 0;
        scanf("%s", s);
        for(int j = 0, len = strlen(s); j < len; ++j)
        {
            if(s[j] == 's') nums++;
            if(s[j] == 'c') numc++;
        }
        v[i] = min(nums, numc);
        c[i] = nums-numc;
        if(c[i] == 0) f[Zero] += v[i];
        else if(c[i] > 0) upv += c[i];
        else if(c[i] < 0) lov += c[i];
    }
    for(int i = 1; i <= n; ++i)
    {
        if(c[i] == 0) continue;
        if(c[i] > 0)
        {
            for(int j = upv; j >= lov+c[i]; --j)
            {
                if(f[j-c[i]] == -1) continue;
                if(j >= c[i]) // plus the diff
                    f[j] = max(f[j], f[j-c[i]]+v[i]);
                else if(j <= 0) // minus the diff
                    f[j] = max(f[j], f[j-c[i]]+v[i]+c[i]);
                else // (0, c[i])
                    f[j] = max(f[j], f[j-c[i]]+v[i]+(c[i]-j));
            }
        }
        else if(c[i] < 0)
        {
            for(int j = lov; j <= upv+c[i]; ++j)
            {
                if(f[j-c[i]] == -1) continue;
                if(j <= c[i]) // plus the diff
                    f[j] = max(f[j], f[j-c[i]]+v[i]);
                else if(j >= 0) // minus the diff
                    f[j] = max(f[j], f[j-c[i]]+v[i]+(-c[i]));
                else // (c[i], 0)
                    f[j] = max(f[j], f[j-c[i]]+v[i]+(j-c[i]));
            }
        }
    }
    printf("%d\n", f[Zero]);
    return 0;
}
```