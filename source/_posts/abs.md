---
title: 绝对值
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2020-04-22 09:50:00
mathjax: true
categories:
  - ACM
tags:
  - abs
description: 绝对值的仨题
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/abs/cover.jpg 
---
# 绝对值的性质

记 $\vert a\vert \Leftrightarrow abs(a)$

$\vert a \vert =\begin{cases}
a, a \geq 0 \\\\
-a, a \leq 0 \\\\
\end{cases}$

$\vert a-b \vert = \max(a,b)-\min(a,b)$

$\vert a-b \vert = \max\{a-b,b-a\}$

$\max(a,b)=\frac{a+b+\vert a-b \vert}{2}$

$\min(a,b)=\frac{a+b-\vert a-b \vert}{2}$

# 例题

## [leetcode 1330. 翻转子数组得到最大的数组值](https://leetcode-cn.com/problems/reverse-subarray-to-maximize-array-value/)

### 题意

给定一个数组 $a$ ,可以翻转(reverse)一个区间一次

求 $\sum \vert a_i-a_{i+1} \vert$ 最大值

### 思路1

观察发现

1. 翻转一个数(长度为 1 的区间)不变

2. 翻转后改变的值只和所选区间左右端点有关,区间中间的贡献无影响


我们先计算出没有翻转(或者翻转一个数)情况下的值记为 $base$

设翻转区间为 $[l, r]$

则翻转后值为 $base-\vert a_{l}-a_{l-1}\vert-\vert a_{r}-a_{r+1}\vert+\vert a_{l-1}-a_{r}\vert+\vert a_{l}-a_{r+1}\vert$

记 $dif=-\vert a_{l}-a_{l-1}\vert-\vert a_{r}-a_{r+1}\vert+\vert a_{l-1}-a_{r}\vert+\vert a_{l}-a_{r+1}\vert$

$\because \vert a-b \vert = \max(a-b,b-a)$

$\therefore$ 拆开绝对值后 $a_{l-1},a_{r}$ 异号, $a_{l},a_{r+1}$ 异号

记 $f_1, f_2 \in \{1,-1\}$ 

$dif=f_1a_{l-1}+f_2a_{l}-\vert a_{l}-a_{l-1}\vert-(f_1a_{r}+f_2a_{r+1}+\vert a_{r}-a_{r+1}\vert)$

记 $F(l)=f_1a_{l-1}+f_2a_{l}-\vert a_{l}-a_{l-1}\vert$

$G(r)=f_1a_{r}+f_2a_{r+1}+\vert a_{r}-a_{r+1}\vert$

要使得 $dif$ 最大,即求 $\max F(l)-\min G(r)$

枚举所有情况

{% spoiler "代码" %}
```cpp
class Solution {
public:
    int maxValueAfterReverse(vector<int>& nums) {
        static const int INF = 1e9;
        int n = nums.size(), base = 0, res, maxf, ming;
        for (int i = 1; i < n; ++i) base += abs(nums[i-1]-nums[i]);
        res = base;
        for (int i = 1; i < n-1; ++i) {
            res = max(res, base-abs(nums[i]-nums[i+1])+abs(nums[i+1]-nums[0]));   // reverse [0, i]
            res = max(res, base-abs(nums[i]-nums[i-1])+abs(nums[i-1]-nums[n-1])); // reverse [i, n-1]
        }
        for (int f1 : {-1, 1}) {
            for (int f2 : {-1, 1}) {
                maxf = -INF;
                ming = INF;
                for (int i = 1; i < n; ++i) {
                    maxf = max(maxf, f1*nums[i-1]+f2*nums[i]-abs(nums[i-1]-nums[i]));
                    ming = min(ming, f1*nums[i-1]+f2*nums[i]+abs(nums[i-1]-nums[i]));
                }
                res = max(res, base+maxf-ming);
            }
        }
        return res;
    }
};
```

{% endspoiler %}

### 思路2

记 $dif=-\vert a_{l}-a_{l-1}\vert-\vert a_{r}-a_{r+1}\vert+\vert a_{l-1}-a_{r}\vert+\vert a_{l}-a_{r+1}\vert$

因为 $l, r$ 之间关系难料,不妨考虑拆掉 $l, r$ 有关的项

不妨假设 $a_{r} \leq a_{l-1}$ , 如果条件不成立,就重新假设我们翻转的是 $[r, l]$ 就成立了(逃

咳咳,总之他们的关系没有影响,但一定要严谨得来说的话,设变量 $i, j$

+ 如果 $a_{r} \leq a_{l-1}, i=l,j=r$ 
+ 如果 $a_{l-1} \leq a_{r}, i=r+1,j=l-1$ 


如此这般,保证了 $a_{r} \leq a_{i-1}$

$
\begin{aligned}  
dif&=-\vert a_{l}-a_{l-1}\vert-\vert a_{r}-a_{r+1}\vert+\vert a_{l-1}-a_{r}\vert+\vert a_{l}-a_{r+1}\vert \\\\
dif&=-\vert a_{i}-a_{i-1}\vert-\vert a_{j}-a_{j+1}\vert+\vert a_{i-1}-a_{j}\vert+\vert a_{i}-a_{j+1}\vert \\\\
&=a_{i-1}-\vert a_{i}-a_{i-1}\vert+\vert a_{i}-a_{j+1}\vert-a_{j}-\vert a_{j}-a_{j+1}\vert \\\\
\end{aligned}
$

此时分类讨论(上面的不算)

+ 如果 $a_{j+1} \leq a_{i}$

    $
\begin{aligned} 
dif&=a_{i-1}+a_{i}-\vert a_{i}-a_{i-1}\vert-(a_{j}+a_{j+1}+\vert a_{j}-a_{j+1}\vert) \\\\
&=2\times\big( \min(a_{i-1},a_{i})-max(a_{j}, a_{j+1}) \big )
\end{aligned}
$
    
    此时要求最大值,则最大化 $\min(a_{i-1},a_{i})$ 最小化 $max(a_{j}, a_{j+1})$

    就是分别取最值嘛

+ 如果 $a_{i} \leq a_{j+1}$

    $dif=a_{i-1}-a_{i}-\vert a_{i}-a_{i-1}\vert-(a_{j}-a_{j+1}+\vert a_{j}-a_{j+1}\vert)$

    $\because a_{i-1}-a_{i}-\vert a_{i}-a_{i-1}\vert \leq 0, a_{j}-a_{j+1}+\vert a_{j}-a_{j+1}\vert \geq 0$

    $\therefore dif \leq 0$


最后记得特判一下 $l = 0$ 或者 $r=n-1$ 的情况

{% spoiler "代码" %}
```cpp
class Solution {
public:
    int maxValueAfterReverse(vector<int>& nums) {
        int n = nums.size(), base = 0, res, minmax = 1e9, maxmin = -1e9;
        for (int i = 1; i < n; ++i) base += abs(nums[i-1]-nums[i]);
        res = base;
        for (int i = 1; i < n-1; ++i) {
            res = max(res, base-abs(nums[i]-nums[i+1])+abs(nums[i+1]-nums[0]));   // reverse [0, i]
            res = max(res, base-abs(nums[i]-nums[i-1])+abs(nums[i-1]-nums[n-1])); // reverse [i, n-1]
        }
        for (int i = 1; i < n; ++i) {
            minmax = min(minmax, max(nums[i-1], nums[i]));
            maxmin = max(maxmin, min(nums[i-1], nums[i]));
        }
        res = max(res, base+2*(maxmin-minmax));
        return res;
    }
};
```

{% endspoiler %}

## [hdoj6435 Problem J. CSGO](http://acm.hdu.edu.cn/showproblem.php?pid=6435)

### 题意
有 $n$ 个 $MW$ 类物品和 $m$ 个 $SW$ 类物品

每个物品有一个属性 $S$ 和 $K$ 个属性 $X$

求 $S_{MW}+S_{SW}+\sum\limits_{i=1}^{k} \vert X_{MW}[i]-X_{SW}[i] \vert$ 的最大值

$n \leq 10^5, K \leq 5$

### 思路
想必聪明的你一定注意到了 $K$ 的取值范围,此中一定有鬼!

$\vert a-b \vert = a-b$ 或 $b-a$

我们不妨考虑物品每个属性的符号,我们选定的两个物品的同种属性的符号一定是相反的

那么无非两种情况,一正一负,一负一正,我们猜测要么正确,要么相反

正确不用管,我们看一下如果猜错了 设正确答案是 $a-b \geq 0$ ,猜测为 $b-a \leq 0$

猜错得到的结果肯定劣于正确结果,那我们就可以放心大胆的枚举所有情况了

详细地说就是记录物品每种属性取正负($2^k$ 种)情况下的贡献

合并两种物品只要保证这两种物品的属性选取正负完全相反即可(位运算)

贪心可得当物品属性选取正负确定时,要取值最大的物品

*感觉讲起来没有代码清楚*

$O(nk2^k)$

{% spoiler "代码" %}
```cpp
#include <bits/stdc++.h>

// #define DEBUG

using namespace std;

const int N = 1e5+7;
const int K = 5;
const int MOD = 998244353;
const long long INF = 1e18;
const double eps = 1e-7;
const double PI = acos(-1);
typedef pair<int, int> pii;

int n, m, k;
int xmw[N][K], xsw[N][K];
long long vmw[N][1<<K], vsw[N][1<<K];
long long mmw[1<<K], msw[1<<K];

inline void solve()
{
    memset(mmw, 0x8f, sizeof mmw);
    memset(msw, 0x8f, sizeof msw);

    scanf("%d %d %d", &n, &m, &k);
    for (int i = 1, s; i <= n; ++i) {
        scanf("%d", &s);
        for (int j = 0; j < k; ++j) scanf("%d", &xmw[i][j]);
        for (int j = 0; j < 1<<k; ++j) {
            vmw[i][j] = s;
            for (int l = 0; l < k; ++l)
                vmw[i][j] += ((j>>l)&1 ? 1 : -1)*xmw[i][l];
            mmw[j] = max(mmw[j], vmw[i][j]);
        }
    }
    for (int i = 1, s; i <= m; ++i) {
        scanf("%d", &s);
        for (int j = 0; j < k; ++j) scanf("%d", &xsw[i][j]);
        for (int j = 0; j < 1<<k; ++j) {
            vsw[i][j] = s;
            for (int l = 0; l < k; ++l)
                vsw[i][j] += ((j>>l)&1 ? 1 : -1)*xsw[i][l];
            msw[j] = max(msw[j], vsw[i][j]);
        }
    }
    long long res = -INF;
    for (int i = 0, j, base = (1<<k)-1; i < 1<<k; ++i) {
        j = base^i;
        res = max(res, mmw[i]+msw[j]);
    }
    printf("%lld\n", res);
}

signed main()
{
    int testcase = 1;
    scanf("%d", &testcase);
    for (int i = 1; i <= testcase; ++i) {
        solve();
    }
    return 0;
}
```

{% endspoiler %}

$O(n2^k)$

{% spoiler "代码" %}
```cpp
#include <bits/stdc++.h>

// #define DEBUG

using namespace std;

const int N = 1e5+7;
const int K = 5;
const int MOD = 998244353;
const long long INF = 1e18;
const double eps = 1e-7;
const double PI = acos(-1);
typedef pair<int, int> pii;

int n, m, k;
int xmw[K], xsw[K];
long long vmw[1<<K], vsw[1<<K], mmw[1<<K], msw[1<<K];
int lg2[1<<K];

inline int lowbit(const int &x) { return x&-x; }

inline void solve()
{
    memset(mmw, 0x8f, sizeof mmw);
    memset(msw, 0x8f, sizeof msw);

    scanf("%d %d %d", &n, &m, &k);
    for (int i = 1, s; i <= n; ++i) {
        scanf("%d", &s);
        vmw[0] = s;
        for (int j = 0; j < k; ++j) {
            scanf("%d", &xmw[j]);
            vmw[0] -= xmw[j];
        }
        mmw[0] = max(mmw[0], vmw[0]);
        for (int j = 1, l; j < 1<<k; ++j) {
            l = lowbit(j);
            vmw[j] = vmw[j^l]+2*xmw[lg2[l]];
            mmw[j] = max(mmw[j], vmw[j]);
        }
    }
    for (int i = 1, s; i <= m; ++i) {
        scanf("%d", &s);
        vsw[0] = s;
        for (int j = 0; j < k; ++j) {
            scanf("%d", &xsw[j]);
            vsw[0] -= xsw[j];
        }
        msw[0] = max(msw[0], vsw[0]);
        for (int j = 1, l; j < 1<<k; ++j) {
            l = lowbit(j);
            vsw[j] = vsw[j^l]+2*xsw[lg2[l]];
            msw[j] = max(msw[j], vsw[j]);
        }
    }
    long long res = -INF;
    for (int i = 0, j, base = (1<<k)-1; i < 1<<k; ++i) {
        j = base^i;
        res = max(res, mmw[i]+msw[j]);
    }
    printf("%lld\n", res);
}

signed main()
{
    for (int i = 0; i < K; ++i) lg2[1<<i] = i;
    int T;
    scanf("%d", &T);
    while (T--) solve();
    return 0;
}
```

{% endspoiler %}

## [codeforces1093G. Multidimensional Queries](https://codeforces.com/problemset/problem/1093/G)

### 题意
与上一题(hdoj)类似,区别是多次询问,求给定区间内的最大值

### 思路
加以数据结构维护最值即可(口胡起来真容易

这里用了单点修改区间查询的线段树

{% spoiler "代码" %}
```cpp
#include <bits/stdc++.h>

// #define DEBUG

using namespace std;

const int N = 2e5+7;
const int K = 5;
const int MOD = 998244353;
const int INF = 0x3f3f3f3f;
const double eps = 1e-7;
const double PI = acos(-1);
typedef pair<int, int> pii;

template <typename T, typename U = greater<T>>
struct SegmentTree
{
    U cmp = U();
    int n;
    T tr[N<<2], lazy[N<<2], init_val = cmp(0, 1) ? INF : -INF;
    SegmentTree(){}
    T mv(const T &x, const T &y) { return cmp(x, y) ? x : y;}
    void build(const int &_n, const T &k = 0) { n = _n; _build(1, n, k); }
    void modify(const int &x, const T &k) { _modify(x, k, 1, n); }
    T query(const int &l, const int &r) { return _query(l, r, 1, n); }
private :
    void push_up(const int &i) { tr[i] = mv(tr[i<<1], tr[i<<1|1]); }
    void push_down(const int &i) {
        if (!lazy[i]) return;
        tr[i<<1] += lazy[i];
        tr[i<<1|1] += lazy[i];
        lazy[i<<1] += lazy[i];
        lazy[i<<1|1] += lazy[i];
        lazy[i] = 0;
    }
    void _build(const int &l, const int &r, const T &k = 0, const int &i = 1) {
        lazy[i] = 0;
        if (l == r) { tr[i] = k; return; }
        int mid = (l+r)>>1;
        _build(l, mid, k, i<<1);
        _build(mid+1, r, k, i<<1|1);
        push_up(i);
    }
    void _modify(const int &x, const T &k, const int &trl, const int &trr, const int &i = 1) {
        if (trl == x && trr == x) {
            tr[i] = k;
            return;
        }
        push_down(i);
        int mid = (trl+trr)>>1;
        if (x <= mid) _modify(x, k, trl, mid, i<<1);
        else _modify(x, k, mid+1, trr, i<<1|1);
        push_up(i);
    }
    T _query(const int &l, const int &r, const int &trl, const int &trr, const int &i = 1) {
        if (trl >= l && trr <= r) return tr[i];
        push_down(i);
        int mid = (trl+trr)>>1;
        T res = init_val;
        if (l <= mid) res = mv(res, _query(l, r, trl, mid, i<<1));
        if (r >  mid) res = mv(res, _query(l, r, mid+1, trr, i<<1|1));
        return res;
    }
};

int n, k, q;
int a[N][K];
int va[N][1<<K];
int lg2[1<<K];
SegmentTree<int> ST[1<<K];

inline void calc(const int &x) {
    va[x][0] = 0;
    for (int i = 0; i < k; ++i) {
        va[x][0] -= a[x][i];
    }
    for (int i = 1, j; i < 1<<k; ++i) {
        j = i&-i;
        va[x][i] = va[x][i^j]+2*a[x][lg2[j]];
    }
}

inline void update(const int &x) {
    for (int i = 0; i < 1<<k; ++i) {
        ST[i].modify(x, va[x][i]);
    }
}

signed main()
{
    for (int i = 1; i < K; ++i) lg2[1<<i] = i;
    scanf("%d %d", &n, &k);
    for (int i = 0; i < 1<<k; ++i) ST[i].build(n);
    for (int i = 1; i <= n; ++i) {
        for (int j = 0; j < k; ++j) {
            scanf("%d", &a[i][j]);
        }
        calc(i);
        update(i);
    }
    scanf("%d", &q);
    int op, x, l, r, res;
    while (q--) {
        scanf("%d", &op);
        if (op == 1) {
            scanf("%d", &x);
            for (int i = 0; i < k; ++i) {
                scanf("%d", &a[x][i]);
            }
            calc(x);
            update(x);
        } else {
            scanf("%d %d", &l, &r);
            res = -INF;
            for (int i = 0, j, base = (1<<k)-1; i <= base; ++i) {
                j = base^i;
                res = max(res, ST[i].query(l, r)+ST[j].query(l, r));
            }
            printf("%d\n", res);
        }
    }
    return 0;
}
```

{% endspoiler %}