---
title: 帕秋莉的手环
date: 2018-10-07 17:30:00
categories:
  - Luogu
tags:
  - 数学
  - 斐波那契
  - 矩阵快速幂
mathjax: true
---

#[Luogu  P4910 帕秋莉的手环](https://www.luogu.org/problemnew/show/P4910)

来自比赛[东方国庆大赛](https://www.luogu.org/contestnew/show/11029) A 题

推荐[某人的题解](https://www.luogu.org/blog/812-xiao-wen/solution-p4910)

## 思路

大致题目所求的就是 黑白相间的珠子**环**

其中把白色当成间隔的话, 间隔的连续白色的数目不得超过2

(虽然题目没看出这个关系,但是根据样例可以得出)

我们不妨设白色状态为 0, 黑色状态为 1

~~忽略中间想了很久的过程~~

如果按线性递推的来

当前如果是白色, 那么前一个必定是heise

当前的如果是黑色, 那么前一个可以是黑色,也可以是白色

我们设 $f[i][0]$ 是第 i 个珠子为白色的方案数

$f[i][1]$ 是第 i 个珠子为黑色的方案数

那么 $f[i][0] = f[i-1][1]$

$f[i][1] = f[i-1][0]+f[i-1][1]$

最后我们还得考虑**环**的情况

也就是首尾的情况

显然如果首尾都是白色, 是不可行的

那么我们就在 $f[n][0]+f[n][1]$ 的基础上减去首尾是白色的方案

如果按照递推的做法,出现首尾是白色的情况必定是当第二个和倒数第二个是黑的

那么除去首尾和第二倒数第二的

剩下 n-4 个满足要求的方案

此时不用再考虑这 n-4 个首尾是否为白色

就是 $f[n-4][0]+f[n-4][1]$

最终结果就是 $f[n][0]+f[n][1]-f[n-4][0]-f[n-4][1]$

如果你稍稍观察或手玩一下数据就会发现

这个是**斐波那契数列**

|$i$|1|2|3|4|5|
|-|-|-|-|-|-|
|$f[i][0]$|1|1|2|3|5|
|$f[i][1]$|1|2|3|5|8|
|sum|2|3|5|8|13|
|ans|1|3|4|7|11|

所以结果是 $Fibo(n+2)-Fibo(n-2)$

---

简单的斐波那契可以线性推导

我是记得有一个通项公式

$a_n = 1/\sqrt{5}[\frac{(1+\sqrt{5})^n}{2}+\frac{(1-\sqrt{5})^n}{2}]$

但是这个根号...处理不了

就是忘记了斐波那契还可以用**矩阵快速幂**算

---

~~还有前几个数据就特判吧~~


## 代码
暴力 $O(n)$
```cpp
#include <bits/stdc++.h>

using namespace std;

const int Maxn = 1e6+7;
const int MOD = 1e9+7;

int T, n;
int f[Maxn];

inline void init()
{
    f[0] = f[1] = 1;
    for(int i = 2; i < Maxn; ++i)
        f[i] = (f[i-1]+f[i-2])%MOD;
}

int main()
{
    init();
    scanf("%d", &T);
    while(T--)
    {
        scanf("%d", &n);
        if(n == 1) puts("1");
        else if(n == 2) puts("3");
        else if(n == 3) puts("4");
        else if(n == 4) puts("7");
        else printf("%d\n", (f[n+1]-f[n-3]+MOD)%MOD);
    }
    return 0;
}
```
矩阵快速幂 $O(\log n)$
```cpp
#include <bits/stdc++.h>

using namespace std;

const int MOD = 1e9+7;

int T;
long long n;

struct Match
{
    long long m[3][3];
    Match(){ memset(m, 0, sizeof m); }
    inline void init()
    {
        m[1][1] = m[2][2] = 1;
        m[1][2] = m[2][1] = 0;
    }
    inline void Fibo_init()
    {
        m[2][2] = 0;
        m[1][1] = m[1][2] = m[2][1] = 1;
    }

    Match operator * (const Match &b) const
    {
        Match res;
        for(int i = 1; i <= 2; ++i)
            for(int j = 1; j <= 2; ++j)
            {
                long long &cur = res.m[i][j];
                for(int k = 1; k <= 2; ++k)
                    cur = (cur+m[i][k]*b.m[k][j])%MOD;
            }
        return res;
    }
} a;

inline Match qpow(Match a, long long p)
{
    Match res;
    res.init();
    while(p)
    {
        if(p&1) res = res*a;
        p >>= 1;
        a = a*a;
    }
    return res;
}

inline int Fibo(long long x)
{
    a.Fibo_init();
    a = qpow(a, x);
    return (int)a.m[1][2];
}

int main()
{
    scanf("%d", &T);
    while(T--)
    {
        scanf("%lld", &n);
        if(n == 1) puts("1");
        else if(n == 2) puts("3");
        else printf("%d\n", (Fibo(n+2)-Fibo(n-2)+MOD)%MOD);
    }
    return 0;
}
```