---
title: 烹调方案
date: 2018-10-18 16:30:00
categories:
  - Luogu
tags:
  - 膜法
  - 数学

mathjax: true
---

# [Luogu P2261 [CQOI2007]余数求和](https://www.luogu.org/problemnew/show/P2261)

## 思路

### 暴力

~~用来验证答案~~ 60 分,满卒

```cpp
#include <bits/stdc++.h>
using namespace std;
int n;
int main()
{
	scanf("%d%d", &n, &k);
	for(int i = 1; i <= n; ++i)
		ans += k%i;
	printf("%lld\n", ans);
	return 0;
}
```

### 正经

反正咋看看推不出什么玄学 O(1) 公式

先换一个不是那么直接的求膜法

$a \% p = a - \lfloor \frac{a}{p} \rfloor \times p$

所以答案可以先粗略看做(先不考虑n, k的大小关系)

$\sum\limits_{i=1}^{n}(k-\lfloor \frac{k}{i} \rfloor \times i)$

$= n\times k - \sum\limits_{i=1}^{k} \lfloor \frac{k}{i} \rfloor \times i$

我们考虑优化后面那个式子

稍微借鉴了 [Luogu P4863 JerryC Loves Driving](https://www.luogu.org/problemnew/show/P4863) 的思维方式

因为 $i$ 是每个不同的, 而 $\lfloor \frac{k}{i} \rfloor$ 是可能相同的

不妨找到一个范围使得 $\lfloor \frac{k}{i} \rfloor = x (i \in [l, r]), x$ 是整数,定值

因为 $i$ 肯定是连续的,那么这些的和就是 $x \times \frac{(l+r)\times (r-l+1)}{2}$ (求和公式)

再看怎么找范围,范围满足

$\lfloor \frac{k}{i} \rfloor = x$ ,也就是 $x \leq \lfloor \frac{k}{i} \rfloor < x+1$

那么~~稍加~~思考就可以得出范围是 $[ \lfloor \frac{k}{x+1} \rfloor +1, \lfloor \frac{k}{x} \rfloor ]$

不过虽然我们看似好厉害好厉害地玄学推理了一大推

实际复杂度任然堪忧,暴力是枚举 $i, O(n)$ ,而这里枚举 $x, O(k)$

其实一样...

---

然后借鉴 [Luogu T46446 矿脉开采](https://www.luogu.org/problemnew/show/T46446) 的降低数据大小的思路

我们不妨分别枚举 $i$ 和 $x$

**把 $k$ 分为 $\sqrt{k}$ 的两份**

复杂度为 $O(\sqrt{k})$

其实应该先想到这个大框架再去想到上面细致的处理

但我思路就是先玄学,再暴力,最后正规...

---

大体思路已经建成,还有很多细节要处理

1. 首先是数据分割的问题
    
    我想到 $k$ 是否是完全平方数,那么 $\sqrt{k}$ 分哪边

    其实没什么大纠结的,~~只是我比较蠢~~

    关系是大致这样的 $k\div i = x$

    假如我们先把 $[1, \sqrt{k}]$ 分给 $i$

    那么 $k \div i \geq \lfloor \frac{k}{\sqrt{k}} \rfloor$

    所以 $x <\lfloor \frac{k}{\sqrt{k}} \rfloor$

    所以就是这么分啦, 设$edge = \sqrt{k}$

    $i \in [1, edge]$

    $x \in [1, k \div edge)$

2. 就是开头忽略的 $n, k$ 的大小关系

    首先很明显的就是 当 $n > k$ 时

    $k \% n = k$,直接加上去就好了

    另外 $n$ 过小就会影响到 $i$ 的取值范围进而又影响 $x$

    $i$ 的取值范围很好调整, 调整为 $i \in [1, \min(n,edge)]$

    那么 $x$ 怎么调整,难道要玄学数学再推理一波?

    其实~~聪明的~~我发现只要对枚举 $x$ 过程中的 $l, r$ 再做限制即可

3. 小心爆 int

---

## 代码

```cpp
#include <bits/stdc++.h>

using namespace std;

int n, k;
long long ans;

int main()
{
    scanf("%d%d", &n, &k);
    ans = 1ll*n*k;
    int edge = sqrt(k);
    // k/i (i[1, edge]) >= floor(k/edge)
    for(int i = 1; i <= min(n, edge); ++i)
        ans -= k/i*i;
    // floor(k/j) = i, i < floor(k/edge)
    for(int i = 1, l, r; i < k/edge; ++i)
    {
        // floor(k/j) == i (j <= n)
        // j [floor(k/(i+1))+1, floor(k/i)]
        l = k/(i+1)+1; r = k/i; 
        r = min(r, n);
        if(l > r) continue;
        ans -= 1ll*(r-l+1)*(l+r)/2*i;
    }
    printf("%lld\n", ans);
    return 0;
}
```