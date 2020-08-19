---
title: 卷积
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2020-04-01 23:00:00
mathjax: true
categories:
  - ACM
tags:
  - 卷积
  - FFT
description: 卷积学习笔记
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/convolution/cover.jpg 
---
# 卷积初步

[本菜鸡的板子参考](https://kaizynx.github.io/2018/11/04/template/#%E5%BF%AB%E9%80%9F%E5%82%85%E9%87%8C%E5%8F%B6%E5%8F%98%E6%8D%A2-FFT)

傻逼起见,数组为 $f[0, n]$

## 一般线性卷积

$f[k]=\sum\limits_{i=0}^{k}a[i]\times b[k-i]$

a 的下标加上 b 的下标等于一个数

类比至多项式乘法 $x^i \times x^j = x^{i+j}$

使用 FFT 就可以快速求出 f 每一项

## 二般线性卷积

$f[k]=\sum\limits_{i=0}^{n-k}a[i]\times b[i+k]$

考虑一般卷积 $\sum\limits_{i=0}^{n-k}a[i]\times b[n-k-i] = f'[n-k]$

于是令 $f[k]=f'[n-k], b'[i] = b[n-i]$

此时 $f'[n-k]=\sum\limits_{i=0}^{n-k}a[i]\times b'[n-k-i]$

$=\sum\limits_{i=0}^{n-k}a[i]\times b[i+k] = f[k]$

### 例题

[hdoj6061 RXD and functions](http://acm.hdu.edu.cn/showproblem.php?pid=6061)

[参考博客](https://www.cnblogs.com/Yumesenya/p/7470832.html)

{% spoiler "代码" %}
```cpp
int n, m, a;
int c[N];
long long f[N], g[N], powa[N];
long long fac[N], inv[N];

inline void solve()
{
    for (int i = 0 ; i <= n; ++i) cin >> c[i];
    cin >> m;
    a = 0;
    for (int i = 1, x; i <= m; ++i) {
        cin >> x;
        a = (a-x+MOD)%MOD;
    }
    powa[0] = 1;
    for (int i = 1; i <= n; ++i) powa[i] = (powa[i-1]*a%MOD+MOD)%MOD;
    for (int i = 0; i <= n; ++i) {
        f[i] = c[n-i]*fac[n-i]%MOD;
        g[i] = powa[i]*inv[i];
    }
    NTT::work(f, n+1, g, n+1);
    for (int i = 0; i <= n; ++i)
        cout << NTT::f[n-i]*inv[i]%MOD << " ";
    cout << endl;
}

signed main()
{
    ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
    fac[0] = inv[0] = 1;
    for (int i = 1; i < N; ++i) {
        fac[i] = fac[i-1]*i%MOD;
        inv[i] = qpow(fac[i], MOD-2);
    }
    while (cin >> n) solve();
    return 0;
}
```

{% endspoiler %}

## 三般线性卷积

$f[k]=\sum\limits_{i=0}^{n}a[i]\times b[k-i]$ 其中 $b[i]=i^2,-n \leq i \leq n$

因为 $k-i$ 可能为负, 令 $b'[i] = b[i-n] = (i-n)^2,0 \leq i \leq 2n$

$f'[n+k]=\sum\limits_{i=0}^{n}a[i]\times b'[n+k-i]$

$=\sum\limits_{i=0}^{n}a[i]\times b[k-i] = f[k]$

[hdoj4656 Evaluation](http://acm.hdu.edu.cn/showproblem.php?pid=4656)

[参考博客](https://www.cnblogs.com/candy99/p/6754278.html)

{% spoiler "代码" %}
```cpp
int n, b, c, d;
int a[N];
long long f[N], g[N<<1], p[N], fac[N], inv[N], c2[N];

void init()
{
    fac[0] = fac[1] = inv[0] = inv[1] = 1;
    for (int i = 2; i < N; ++i) {
        fac[i] = fac[i-1]*i%MOD;
        inv[i] = (MOD-MOD/i)*inv[MOD%i]%MOD;
    }
    for (int i = 2; i < N; ++i) (inv[i] *= inv[i-1]) %= MOD;
}

inline void solve()
{
    long long pw = 1;
    for (int i = 0; i < n; ++i, (pw *= d) %= MOD) {
        f[i] = a[n-1-i]*fac[n-1-i]%MOD;
        g[i] = pw*inv[i]%MOD;
    }
    MTT::work(f, n, g, n);
    for (int i = 0; i < n; ++i) p[i] = MTT::f[n-1-i];

    pw = 1;
    for (int i = 0; i < n; ++i, (pw *= b) %= MOD) {
        c2[i] = qpow(c, 1ll*i*i%(MOD-1));
        f[i] = pw*c2[i]%MOD*p[i]%MOD*inv[i]%MOD;
        g[i+(n-1)] = g[-i+(n-1)] = qpow(c2[i], MOD-2);
    }
    // for (int i = -n+1; i < n; ++i) g[i+(n-1)] = qpow(qpow(c, 1ll*i*i), MOD-2);
    MTT::work(f, n, g, n*2-1);
    for (int i = 0; i < n; ++i)
        cout << MTT::f[n-1+i]*c2[i]%MOD << endl;
}

signed main()
{
    ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
    init();
    while (cin >> n >> b >> c >> d) {
        for (int i = 0; i < n; ++i) cin >> a[i];
        solve();
    }
    return 0;
}
```

{% endspoiler %}

## 循环卷积

咕咕咕

# 二进制卷积

[本菜鸡的板子参考](https://kaizynx.github.io/2018/11/04/template/#%E5%BF%AB%E9%80%9F%E6%B2%83%E5%B0%94%E4%BB%80%E5%8F%98%E6%8D%A2-FWT)

数组一般为 $f[0, 2^n)$

## 简介

### 快速沃尔什变换|FWT

[推导详解](https://www.luogu.com.cn/blog/command-block/wei-yun-suan-juan-ji-yu-ji-kuo-zhan)

[公式参考](https://www.cnblogs.com/GavinZheng/p/11721127.html)

[洛谷例题](https://www.luogu.com.cn/problem/P4717)

复杂度 $O(n\log n) | O(n2^n)$

$FWT(A\pm B)=FWT(A)\pm FWT(B)$

$FWT(cA)=cFWT(A)$

定义⊕为任意集合运算

$FWT(A\bigoplus B)=FWT(A)\times FWT(B)$

求 $C_i = \sum\limits_{i=j\bigoplus k}{a_j b_k}$

#### 或运算
$FWT(A)[i] = \sum\limits_{j|i=i}{A[j]}$

$FWT(A) = [FWT(A_0),FWT(A_0+A_1)]$

$IFWT(A) = [IFWT(A_0),IFWT(A_1)-IFWT(A_0)]$


#### 与运算
$FWT(A)[i] = \sum\limits_{i\&j=j}{A[i]}$

$FWT(A) = [FWT(A_0+A_1),FWT(A_1)]$

$IFWT(A) = [IFWT(A_0)-IFWT(A_1),IFWT(A_1)]$

#### 异或运算
令 $d(x)$ 为 $x$ 在二进制下拥有的1的数量

$FWT(A)[i] = \sum\limits_{d(j\&i)为偶数}{A[j]}-\sum\limits_{d(k\&i)为奇数}{A[k]}$

$FWT(A) = [FWT(A_0+A_1),FWT(A_0-A_1)]$

$IFWT(A) = [\frac{IFWT(A_1-A_0)}{2},\frac{IFWT(A_1+A_0)}{2}]$

### 快速莫比乌斯变换|FMT

据说 FWT 做的事情完全包含 FMT 且常数是一半(咕之

### 快速子集变换(子集卷积)|FST

$C_k = \sum\limits_{i\&j=0,i|j=k}{A_i B_j}$

复杂度 $O(n\log^2 n) | O(n^22^n)$

## 应用

### 倍增子集卷积

[hdu6851](http://acm.hdu.edu.cn/showproblem.php?pid=6851)

设多项式 $A = \sum\limits_{i=0}^{2^n-1}{a_i x^i},B=\sum\limits_{i=0}^{2^n-1}{b_i x^i}$

求 $C = A*B = \sum\limits_{i=0}^{2^n-1}{x^i \sum\limits_{d\subseteq i}{a_d b_{i-d}}}$

按照每个状态的最高位进行分组，然后卷 $n$ 次

复杂度 $O(\sum\limits_{i=1}^{n}{i^2 2^i}) = O(n^2 2^n)$

以上为官方说辞?以下为个人理解

给出 $A$ 求 $C$ 满足

$C_k = \sum\limits^{所有能组成k的子集集合S}{\prod\limits_{i\in S}{A_i}}$

不知道怎么表达就是 $\forall i \in S,j \in S, i \neq j \rightarrow i\&j =0$ 且集合内所有数按位或等于 $k$

如果直接拿两个 $A$ 子集卷积会有很多重复

可以先赋 $C=[1,0,0,0\cdots]$

像背包一样,把 $A$ 按照最高位依次拿去更新,就不会重复

就是把 $A$ 分成 $A_{0001},A_{001?},A_{01??},A_{1???}$ 这么些物品这个意思

### 杂糅好题

[CF914G Sum the Fibonacci](https://kaizynx.github.io/2020/08/16/Codeforces914G/)

差不多各种板子都用上了,还要加上一定的理解掌握

详情我另写了篇[博客](https://kaizynx.github.io/2020/08/16/Codeforces914G/)

# 参考资料

[浅谈FFT--从DFT到*CZT,及一些技巧](https://www.luogu.com.cn/blog/105254/qian-tan-fft-zong-ft-dao-fft)

[位运算卷积(FWT)与其扩展](https://www.luogu.com.cn/blog/command-block/wei-yun-suan-juan-ji-yu-ji-kuo-zhan)
