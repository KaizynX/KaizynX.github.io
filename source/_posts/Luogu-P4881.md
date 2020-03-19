---
title: hby与tkw的基情
date: 2018-09-13 10:17:00
categories:
  - Luogu
tags:
  - 数学
  - 快速幂
  - 排列组合
  - 乘法逆元

mathjax: true
---

*[题目来源:洛谷P4881 hby与tkw的基情](https://www.luogu.org/problemnew/show/P4881)*

## 前言

解决本题所需要具备的知识

~~非战斗人员请即时撤离~~
- 快速幂
- 排列组合
- 高中数学之数列
- 除法取模(乘法逆元)
  
---

## 题意

给出 T 组 n, 求

$Ans=\sum\limits_{i=1}^ni   \times    s[i]   \times   [i\%2]$

其中$s[i]$代表长度为 i 的回文串的个数(只考虑小写字母)，最后面是bool表达式

输出答案$\%(10^9+7)$的值

$T\leq5×10^5 ,n\leq10^9$

## 思路

看到这个 n 的范围就整个人不太好了

肯定是把公式推导到 O(1) 吧

1. 首先最简单的就是$[i\%2]$

    i为奇数是 1, i为偶数是 0

    也就是说我们不需要考虑i为偶数的情况

2. 再者我们思考$s[i]$
    
    对于回文字串, 上面已经推出长度i是奇数

    也就是说我们要考虑1~⌈$\frac{i}{2}$⌉位的字母

    每一位上可以填a~z共26个字母, 一共就是$26^{⌈\frac{i}{2}⌉}$种

    这个不会重复不用说明吧

    所以:$s[i]=26^{⌈\frac{i}{2}⌉}$

至此已经可以用O(n)的办法求解了

我们继续化简

整合得到:

设$f(i)$是$n=i$时的$Ans$

$f(i)=\sum\limits_{i=1}^ni   \times    s[i]   \times   [i\%2]$

$=1   \times   26^1+3   \times   26^2+5   \times   26^3+...$

因此我们不妨把i除以2来把i为偶数的情况去掉

令$k={⌈\frac{i}{2}⌉}$

$f(k)=1   \times   26^1+3   \times   26^2+5   \times   26^3+...+(2   \times   k-1)   \times   26^k$

这时我大为震惊,这竟然要用到高中数学的数列知识

$f(k)=1  \times  26^1+3  \times  26^2+5  \times  26^3+...+(2  \times  k-1)  \times  26^k$

$26  \times  f(k)=1  \times  26^2+3  \times  26^3+5  \times  26^4+...+(2  \times  k-1)  \times  26^{k+1}$

下面减去上面的式子得到

$25  \times  f(k)=(2  \times  k-1)  \times  26^{k+1}-1  \times  26^1-2  \times  (\sum\limits_{i=2}^{k} 26^i)$

利用等比数列的求和公式得到

$25  \times  f(k)=(2  \times  k-1)  \times  26^{k+1}-26-2  \times  \frac{26^2  \times  (1-26^{k-1})}{1-26}$

化简步骤略略略

最后得到:

$f(k)=\frac{2  \times  k-1}{25}  \times  26^{k+1}-\frac{26}{25}-\frac{2}{25^2}  \times  26^{k+1}+\frac{2}{25^2}  \times  26^2$

其实已经完成了的,**但是**,答案很大还要取模

那么就不得不考虑除法的问题了

又一次感到了深深的恶意

**乘法逆元!!!**

通分得到:

$f(k)=\frac{1}{25^2}[(50  \times  k-27)   \times   26^{k+1}+702]$

问题至此终于算的上是真正意义上的解决了

对于$26^{k+1}$用快速幂

因为取模后数字还可能有$10^9$那么大

而$n$也有$10^9$那么大

$(50   \times   k-27)   \times   26^{k+1}$就极有可能爆$int$

~~反正遇到这种题无脑开long long不就行了~~

处理一下$26^2$也就是$625$的乘法逆元就可以了

关于 **乘法逆元 除法取模 快速幂** 的知识就不在此详细讲解

```cpp
#include <cstdio>
#include <iostream>

using namespace std;

const long long Mo = 1e9+7;

int T;
long long n;

// 快速幂
inline long long quick_mo(long long x, long long p, long long mo)
{
    long long res = 1;
    x %= mo;
    while(p)
    {
        if(p&1) res = res*x%mo;
        x = x*x%mo;
        p >>= 1;
    }
    return res;
}

// 费马小定理计算乘法逆元
inline long long mul_inverse(int x)
{
    return quick_mo(x, Mo-2, Mo);
}

int main()
{
    scanf("%d", &T);
    long long inverse = mul_inverse(625);
    while(T--)
    {
        scanf("%lld", &n);
        n = (n+1)/2;
        // f(n) = (1/625)*((50*n-27)*26^(n+1)+702)%(1e9+7)
        long long res = ((50*n-27)%Mo*quick_mo(26, n+1, Mo)%Mo+702)%Mo*inverse%Mo;
        printf("%lld\n", res);
    }
    return 0;
}
```