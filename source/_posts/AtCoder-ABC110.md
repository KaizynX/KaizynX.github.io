---
title: AtCoder ABC110 D-Factorization 
date: 2018-09-24 11:14:29
categories:
  - AtCoder
tags:
  - 数学
  - 排列组合
  - 质因数分解
  - 乘法逆元

mathjax: true
---

# [Factorization](https://beta.atcoder.jp/contests/abc110/tasks/abc110_d)

## 题意

给你两个整数$N, M$

问有多少个长度为 $N$ 的序列 $a$ 满足

$a_1 \times a_2 \times ... \times a_N = M$

答案对 $10^9+7$ 取模

当存在任意个数 $i$ 使得 $a_i' \neq a_i''$ 则两个序列不同

## 数据范围

- 所有的输入都是整数
- $1 \leq N \leq 10^5$
- $1 \leq M \leq 10^9$

## 样例

### 输入

    2 6

### 输出

    4

### 解释

$(1,6),(2,3)(3,2),(6,1)$

## 思路

显然 $a_i$ 可以被 $M$ 整除

我们对 $M$ 进行分解质因数

$$M = p_1^{b_1} \times p_2^{b_2} \times ... \times p_k^{b_k} $$

所以

$$ a_i = p_1^{c_{i1}} \times p_2^{c_{i2}} \times ... \times p_k^{c_{ik}} $$

此时有

$$ \prod\limits_{i=1}^N a_i = \prod\limits_{i=1}^k p_i^{\sum\limits_{j=1}^N c_{ij}}$$

~~其实不用看上面那个式子显然~~可得

$$ c_{1j} + c_{2j} + ... + c_{Nj} = b_j $$

因此问题就转化为了将 $b_i$ 划分成 $N$ 个部分(可以有0)

这里用到排列组合的知识

问题等价于在 $bi$ 中塞入 $N-1$ 个隔板

为了方便求解,不妨增添 $N-1$个物品再划分成 $N$ 个部分(此时每部分都 $\geq 1$)

划分完后每部分减去1就变回了原来的问题

所以对 $bi$ 的划分方案数就是 $C_{bi+N-1}^{N-1}$

又因为 $C_p^q = C_p^{p-q}$

所以 $C_{bi+N-1}^{N-1} = C_{bi+N-1}^{bi}$

计算时可以取 $C_{bi+N-1}^{\min(N-1, bi)}$

根据乘法原理,总的方案数就是

$$ \prod\limits_{i=1}^k C_{bi+N-1}^{b_i}$$

接下来就是实现的问题了

- 质因数分解

    我们只需要记录 $b_i$ 即可

```cpp
inline void Prime_factorization(int x, vector<int> &v)
{
    for(int i = 2, cnt; i <= x; ++i)
    {
        if(x%i) continue;
        cnt = 0;
        while(x%i == 0)
        {
            x /= i;
            cnt++;
        }
        v.emplace_back(cnt);
    }
    if(x > 1) v.emplace_back(x);
}
 ```
- 排列组合计算

    $C_p^q = \frac{p(p-1)(p-2)...(p-q+1)}{q!}$

    我们想想最坏的情况

    $M = 2^b$

    如果 $M$ 可以分解成其他质因数

    那么显然 $b_i$ 的和小于全是2的

    那么最多有 $log_2 M$ 个质因数

    也就是说我们计算 $C_{b+N-1}^b$ 时

    最大会有 $C_{100000}^{30}$

    显然爆 int 爆 long long

    那么考虑如何取模

    此处涉及除法

    第一念头想到了乘法逆元

    第二念头仔细一想

    除数最大有 $30!$ (阶乘)

    这个好像……~~直接上高精度~~

    于是我想到某暴力

    因为 $C_p^q$ 最后时整数

    就是说除数是能和被除数整除的

    也就是说对除数和被除数分解质因数也是能约掉的

    想想还是对的,没有错的!

    但是我懒得这么做,真的麻烦

    于是我就没分解开始约数

    我们看的计算公式,上面有 $q$ 项

    下面也有 $q$ 项,且是 1~q

    而有根据鸽巢定理

    连续的q个数中肯定有一个数能被q整除

    于是我这么写
```cpp
inline long long comb(int a, int b) // C_a^b
{
    if(b == 0) return 1;
    vector<int> x, y;
    for(int i = a; i > a-b; --i)
        x.emplace_back(i);
    for(int i = 2; i <= b; ++i)
        y.emplace_back(i);
    for(int i = 0; i < x.size(); ++i)
        for(int j = y.size()-1; j >= 0; --j)
        {
            if(x[i]%y[j] == 0)
            {
                x[i] /= y[j];
                y[j] = 1;
            }
        }
    long long res = 1;
    for(int i : x)
        res = res*i%Mo;
    return res;
}
```
可惜不足的是偏偏 WA 了一个点

找不到什么坑,总之这个做法欠缺完美~~因为我的懒~~

解决方案

1. 对除数被除数分别分解质因数再约
   
   复杂度:高

2. 利用鸽巢原理将 $q$ 个数字对应起来约掉

    如何对应尚不明确...~~埋下巨坑~~

想必我的代码是因为某一个对应错了,导致没有约完

---

那么我们不妨考虑另一种做法~~参看大佬的代码~~

还是乘法逆元

那么我可能没想到

$a^{-1} = (a\%p)^{-1}$

假设用费马小定理求解逆元

$a^{-1} = a^{p-2}\%p$

而因为

$a^k = (a\%p)^k$

所以得证

$a^{-1} = (a\%p)^{-1}$

## 代码


```cpp
#include <bits/stdc++.h>

using namespace std;

const int Mo = 1e9+7;

int n, m;
vector<int> v;

inline long long quickm(long long a, int p) // a^p%Mo
{
    long long res = 1;
    while(p)
    {
        if(p&1) res = res*a%Mo;
        a = a*a%Mo;
        p >>= 1;
    }
    return res;
}

inline long long mul_inverse(long long x)
{
    return quickm(x, Mo-2);
}

inline void Prime_factorization(int x, vector<int> &v)
{
    for(int i = 2, cnt; i <= x; ++i)
    {
        if(x%i) continue;
        cnt = 0;
        while(x%i == 0)
        {
            x /= i;
            cnt++;
        }
        v.emplace_back(cnt);
    }
    if(x > 1) v.emplace_back(x);
}

inline long long comb(int a, int b) // C_a^b
{
    if(b == 0) return 1;
    long long p = 1, q = 1;
    for(int i = 1; i <= b; ++i)
    {
        p = p*a%Mo; a--;
        q = q*i%Mo;
    }
    return p*mul_inverse(q)%Mo;
}

int main()
{
    cin >> n >> m;
    Prime_factorization(m, v);
    long long ans = 1;
    for(int i : v)
        ans = ans*comb(i+n-1, min(i, n-1))%Mo;
    cout << ans << endl;
    return 0;
}
```

### 时间复杂度

质因数分解 $O ( \sqrt{M} )$

质因数的个数( $b$ 最大值) $log M$

乘法逆元 $O ( log 10^9)$ 与  $O ( log M)$ 等数量级

所以计算一次排列组合 $O ( b + log 10^9)$ 

等价于 $O ( log M )$

全部的排列组合 $O ( ( logM )^2 )$

最终复杂度还是在分解质因数上 $O ( \sqrt{M} )$

好像和 $N$ 没扯上任何关系...

数量级上 $O ( \sqrt{M} ) \Longrightarrow O ( N )$

---