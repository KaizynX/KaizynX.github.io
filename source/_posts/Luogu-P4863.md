---
title: JerryC Loves Driving
date: 2018-09-25 17:00:00
categories:
  - Luogu
tags:
  - 数学

mathjax: true
---
# [Luogu P4863 JerryC Loves Driving](https://www.luogu.org/problemnew/show/P4863)

## 题意

给 $A,B$ 求

$\sum\limits_{i=A}^B \sum\limits_{j=1}^{i}\lfloor \frac{i}{j} \rfloor \times (-1)^j$

## 数据范围

$1⩽A⩽B⩽2\times10^7$

## 思路

$O(n)$ 算法

$\sum\limits_{i=A}^B \sum\limits_{j=1}^{i}\lfloor \frac{i}{j} \rfloor \times (-1)^j$

$=\sum\limits_{j=1}^{i}\sum\limits_{i=A}^B \lfloor \frac{i}{j} \rfloor \times (-1)^j$

这样就可以处理掉 $(-1)^j$

$=-\sum\limits_{i=A}^B \lfloor \frac{i}{1} \rfloor + \sum\limits_{i=A}^B \lfloor \frac{i}{2} \rfloor - \sum\limits_{i=A}^B \lfloor \frac{i}{3} \rfloor ...(-1)^B \times \sum\limits_{i=A}^B \lfloor \frac{i}{B} \rfloor$

然后我们再想办法 $O(1)$ 求 $\sum\limits_{i=A}^B \lfloor \frac{i}{j} \rfloor$

向下取整是个神奇的东西,向下取整后结果就会变成形如

$k,k,k+1,k+1,k+1.....k+m,k+m,k+m$

例如:当 $A=4,B=13,j=3$

|i|4|5|6|7|8|9|10|11|12|13|
|-|-|-|-|-|-|-|-|-|-|-|-|
|$\lfloor \frac{i}{j} \rfloor$|1|1|2|2|2|3|3|3|4|4|

设 $k=\lfloor \frac{i}{j} \rfloor$

那么 $\lfloor \frac{i}{j} \rfloor$ 的 $i$ 的个数就有 $j$ 个

当然两端的是特殊状况

左端是 $\lfloor \frac{A}{j} \rfloor$ ,我们记作 $a$

左端是 $\lfloor \frac{B}{j} \rfloor$ ,我们记作 $b$

于是中间就是 $[a+1,b-1]$

可以用等差数列求和公式算得

中间的和 $\sum\limits_{i=a+1}^{b-1} i \times j = j \times \frac{(a+b)\times (b-a-1)}{2}$

我们再看两边的,**不难**看出

左边长度为 $((\lfloor \frac{A}{j} \rfloor+1)\times j) -A$

右边长度为 $B-(\lfloor \frac{B}{j} \rfloor \times j) + 1$

继续考虑细节问题

1. 原式中是 $\lfloor \frac{i}{j} \rfloor$ 不存在 $i < j$

    当 $i < j$ 时 $\lfloor \frac{i}{j} \rfloor = 0$

    所以变式没有影响

    但是!会对我们的求和操作造成影响

    解决办法: 当 $j > A$ 时, 令 $A = j$

2. 当 $\lfloor \frac{A}{j} \rfloor = \lfloor \frac{B}{j} \rfloor$ 时
   
   我们会重负计算

   故特判即可

3. 当 $\lfloor \frac{A}{j} \rfloor +1 = \lfloor \frac{B}{j} \rfloor$ 时

    不存在中间那部分

    而这对求和造成影响

    所以也需要特判

4. 正负号很好判断

5. 以防万一,开long long

    但是开了会降速很多(变成大概2倍了吧)

    但还是能过

    稍微分析一下

    运算过程中产生的最大中间量应该为

    $j\times \frac {(a+b)\times (b-a-1)}{2}$

    求和最大 $\frac{B^2}{j^2}$

    所以结果大概是 $\frac{B^2}{j}$

    就是会爆啊

## 代码

```cpp
#include <cstdio>
#include <iostream>

using namespace std;

int A, B;
long long ans;

int main()
{
    cin >> A >> B;
    for(int j = 1, a, b; j <= B; ++j)
    {
        if(j > A) A = j;
        long long tag = j&1 ? -1 : 1;
        a = A/j, b = B/j;
        if(a == b) ans += tag*(B-A+1)*a;
        else
        {
            ans += tag*( ((a+1)*j-A)*a + (B-b*j+1)*b );
            if(a+1 < b) ans += tag*j*(a+b)*(b-a-1)/2;
        }
    }
    cout << ans << endl;
    return 0;
}
```
## 写在后面

因为数据范围 $10^7, O(n)$ 算法苟过

其实还有 $\sqrt{n}$ 的算法

有待研究