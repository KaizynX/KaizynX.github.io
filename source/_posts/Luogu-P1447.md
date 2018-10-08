---
title: 能量采集
date: 2018-10-08 20:30:00
categories:
  - NOI
tags:
  - 容斥原理
  - 最大公约数
mathjax: true
---

# [Luogu P1447 [NOI2010]能量采集](https://www.luogu.org/problemnew/show/P1447)

## 思路

~~参考自[题解](https://www.luogu.org/problemnew/solution/P1447)~~

分析某个点的连接线段上有几个植物

设这个点的坐标是 $(i, j)$, 答案就是 $\gcd (i, j)$

采用离散化思想, 把 $(i, j)$ 离散成 $(\frac{i}{\gcd}, \frac{j}{\gcd})$

不要再深入解释了吧... 方案就是 $i \div \frac{i}{\gcd} = \gcd$

总结果是 $\sum\limits_{i=1}^{n} \sum\limits_{j=1}^{m} \gcd (i,j)$

然后暴力枚举, 80分 get...

---

我们设 $f[x]$ 是 gcd 为 x 的 (i, j) 的对数, 这个不好直接求

设 $g[x]$ 是包含 x 公因数的 (i, j) 对数

$g[x]$ 很好求, $g[x] = \frac{n}{x} \times \frac{m}{x}$

就是选择 x 倍数的长和 x 倍数的宽, 乘法定理...

两者什么关系呢, $g[x] = f[x] + f[2x] + f[3x] + ...$

所以根据容斥原理, $f[x] = g[x] - f[2x] - f[3x] - ...$

所以我们就可以逆着推出 $f[x]$

---

### 时间复杂度

$\sum\limits_{i=1}^{N} \frac{N}{i}$

不清楚具体多少, $10^5$ 大致对应 $10^7$

介于$O(\sqrt{N})$ 和 $O(\log N)$ 之间,勉强水过...

---

## 代码
```cpp
#include <bits/stdc++.h>

using namespace std;

const int Maxn = 1e5+7;

int n, m;
long long ans, f[Maxn];

int main()
{
    cin >> n >> m;
    for(int i = min(n, m); i; --i)
    {
        f[i] = 1ll*(n/i)*(m/i);
        for(long long j = i+i; j <= min(n, m); j += i)
            f[i] -= f[j];
        ans += f[i]*i;
    }
    cout << ans*2-1ll*n*m << endl;
    return 0;
}
```
---

## 结尾

其实本题的正确做法应该是 **莫比乌斯反演**,**狄利克雷卷积** 之类加上玄学推导数论题

但是我不会不会不会啊 ~~就这种解释看得懂~~