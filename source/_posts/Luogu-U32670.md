---
title: 小凯的数字
date: 2018-09-24 13:00:00
categories:
  - Luogu
tags:
  - 数学
---

# [小凯的数字](https://www.luogu.org/problemnew/show/U32670)

## 题意

小凯写下一串数字 $l(l+1)(l+2)...(r-1)r$

例如:l=2,r=5时，数字为:2345

l=8,r=12时数字为:89101112

求这个数 %9 的值

Q组询问

## 数据范围

$Q \leq 10^4$

$0 < l \leq r \leq 10^{12}$


## 思路

*其实可以类比除以3*

先想想暴力的做法

要构造出这样的数字

就要把 l 乘上 10^k 在加上 l+1

依此进行下去

于是我们注意到 10^k

显然发现 $10^k \div 9 = 111...111 余1$

即 $10^k \%9 = 1$

所以

$( a \times 10^k + b ) \% 9$

$= ( a \times 10^k ) \% 9 + b \% 9$

$= a \% 9 \times 10^k \% 9 + b \% 9$

$= a\% 9 + b \% 9$

那么问题就转化成

$( \sum\limits _{i=l} ^r i )\% 9$

根据求和公式

$\frac { (l+r) \times (r-l+1) }{2} \% 9$

坑点就是中间运算可能会爆long long

## 代码

```cpp
#include <cstdio>
#include <iostream>

using namespace std;

int Q;
long long l, r;

int main()
{
    cin >> Q;
    while(Q--)
    {
        cin >> l >> r;
        long long a = l+r, b = r-l+1;
        if(a%2 == 0) a /= 2;
        else b /= 2;
        a %= 9;
        b %= 9;
        cout << a*b%9 << endl;
    }
    return 0;
}
```