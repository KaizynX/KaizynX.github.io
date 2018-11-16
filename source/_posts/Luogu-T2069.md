---
title: 生日礼物
date: 2018-11-16 13:30:00
categories:
  - Luogu
tags:
  - 数学
  - 质因数分解
  - 素数

mathjax: true
---

我觉得还算简单的一道数学题吧

<!--more-->

# [Luogu T2069 生日礼物](https://www.luogu.org/problemnew/show/T2069)

## 题面

$lcm(a, b) == n$ ,求$a, b$的对数

输入 n, 输出方案数

## 数据范围

n <= 10^16

## 思路

考虑两个数的lcm

分解质因数的话

$a = p_1^{qa_1} \times p_2^{qa_2} \times ... \times p_m^{qa_m}$

$b = p_1^{qb_1} \times p_2^{qb_2} \times ... \times p_m^{qb_m}$

假设 $c = lcm(a,b)$

$c = p_1^{qc_1} \times p_2^{qc_2} \times ... \times p_m^{qc_m}$

那么有

$qc_i = max(qa_i, qb_i), i \in [1, m]$

那么就可以开始排列了

对于每一个可以分解出来的质数 $p_i$

$a,b$ 至少有一个的质数是 $qc_i$

也就是 $qa_i == qc_i || qb_i == qc_i$

假如两个都是 $qc_i$ 就只有一种情况

假如只有一个是 $qc_i$ ,那么另一个可以是 $[0, qc_i-1]$

因为 $p_i^0 = 1$ ,就相当于这个数分解不出这样一个质数

结果大概是对于 $lcm(a,b)分解质因数$

然后答案是 $\prod\limits_{i=1}^{m}qc_i*2+1$

引申到大范围来说,如果某个质数$p$并不在分解出的范围内

那么对结果的影响也就是乘上一个 1, 所以也是正确的

---

核心思路如上

本题还有一些要注意的地方

质因数分解理想复杂度是 $O(\sqrt{n})$

事实上并非如此

如果 n 一开始就是一个大素数,就T了

因此应对的策略其实也很简单

一开始特判一下

还有就是如果 n 是几个大素数的乘积

那也比较麻烦,所以每分解一个质数再判断一下

ps: 即使用非常朴素的判断方法也快得一批

---

## 代码
```cpp
#include <bits/stdc++.h>

using namespace std;

long long n, ans = 1;

inline bool is_prime(long long x)
{
    if(x == 1) return false;
    for(long long i = 2; i*i <= x; ++i)
        if(x%i == 0) return false;
    return true;
}

int main()
{
    cin >> n;
    if(is_prime(n)) return puts("3")&0;
    for(long long i = 2; i <= n; ++i)
    {
        if(n%i == 0)
        {
            int cnt = 0;
            while(n%i == 0)
            {
                n /= i;
                cnt++;
            }
            ans *= cnt*2+1;
            if(is_prime(n))
            {
                ans *= 3;
                break;
            }
        }
    }
    cout << ans << endl;
    return 0;
}
```