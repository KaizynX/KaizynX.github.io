---
title: Scarlet的字符串不可能这么可爱
date: 2018-10-07 21:10:00
categories:
  - Luogu
tags:
  - 数学
  - 排列组合
  - 快速幂
mathjax: true
---

# [Luogu T48749 [1007]Scarlet的字符串不可能这么可爱](https://www.luogu.org/problemnew/show/T48749)

来自比赛[【LGR-053】洛谷10月月赛I](https://www.luogu.org/contestnew/show/11084) 的 B 题

## 思路

直接讲正解了

当一个字符串是回文字符串, 有两类情况

1. 偶数回文字符串
2. 奇数回文字符串

都是从中间开始对称

如果是暴力的找回文串算法

一定是以一个点为中心,往两边找,知道找到两边不相同的为止

那么首要条件是最开始对称的两个要相同

如果最中心的两个都不相同,那么后面再怎么回文也不是了

我们要做的就是扼杀在摇篮

要求 没有任何一个长度超过1的回文连续子串

1. 偶数回文字符串
   
    任意相邻两个不得相同

2. 奇数回文字符串

    任意三个连续的字符 $c_1, c_2, c_3$

    满足 $c_1 \neq c_3$

~~显然~~按上述条件扼杀掉萌芽之后就不会产生回文串

这样我们就可以求得总的方案数

假设没有任何约束条件,求

k 种字符构成,长度为 L 的 没有任何一个长度超过1的回文连续子串 的个数

(此处排列组合知识,类似全排列)

如果从头到尾开始构造

$c_1$ 有 k 种选择

$c_2$, 因为 $c_2 \neq c_1$, 所以有 k-1 种选择

$c_3$, 因为 $c_3 \neq c_2$ 且 $c_3 \neq c_1$, 所以有 k-2 种选择

$c_4$, 因为 $c_4 \neq c_3$ 且 $c_4 \neq c_2$, 所以有 k-2 种选择

...

$c_L$ 有 k-2 种

最后结果就是 $k\times (k-1) \times (k-2)^{L-2}$

**注意特判** $L \geq 2$

---

上面的就是核心的思路了, 知道上面的加上一点暴力就可以 80 了(哈哈哈)

~~好吧好吧~~我们考虑剩下的 20 分

当给你一个限制条件: s 位上必须是 w

由上面知道 w 具体是什么其实并没有什么关系

既然 s 位已经决定了, s 位会影响到 s-2, s-1, s+1, s+2 位上的选择数

所以我们不妨从 s 位开始从左往右找方案

1. 先从 s 开始往左
   
   s-1 位有 k-1 种

   s-2 位有 k-2 种

   s-3 位有 k-2 种

   ...

   1 位有 k-2 种

   共有 $(k-1)\times (k-2)^{s-2}$

   这里也需要特判 $s \geq 2$

2. 再从 s 开始往右
   
   s+1 位受到 s-1(我们先找了左边) 和 s 位影响, 有 k-1 种

   s+2 位有 k-2 种

   s+3 位有 k-2 种

   ...

   L 位有 k-2 种

   共有 $(k-2)^{L-s}$

   这里也需要特判 $L-s \geq 0$

---

最后还有特判+快速幂

## 代码
```cpp
#include <bits/stdc++.h>

using namespace std;

long long k, L, s, w, p, ans;

inline long long qpow(long long a, long long x)
{
    long long res = 1;
    while(x)
    {
        if(x&1) res = res*a%p;
        x >>= 1;
        a = a*a%p;
    }
    return res;
}

int main()
{
    cin >> k >> L >> p >> s;
    long long k_1 = (k-1)%p, k_2 = (k-2)%p;
    k %= p;
    if(s)
    {
        cin >> w;
        if(L == 1) ans = 1;
        else if(s == 1 || s == L)
            ans = k_1*qpow(k_2, L-2)%p;
        else
        {
            ans = k_1*qpow(k_2, s-2)%p; // left
            ans = ans*qpow(k_2, L-s)%p; // right
        }
        cout << ans << endl;
    }
    else
    {
        if(L == 1) ans = k;
        else ans = k*k_1%p*qpow(k_2, L-2)%p;
        cout << ans << endl;
    }
    return 0;
}
```