---
title: Luogu P1417 烹调方案
date: 2018-08-31 21:33:36
tags:
  - DP
  - Luogu
---

**摘自tinylic的题解**:

现在考虑相邻的两个物品x,y。假设现在已经耗费p的时间，那么分别列出先做x,y的代价：

①a[x]-(p+c[x])*b[x]+a[y]-(p+c[x]+c[y])*by

②a[y]-(p+c[y])*b[y]+a[x]-(p+c[y]+c[x])*bx

==>

①a[x]-(p+c[x])*b[x]+a[y]-(p+c[x]+c[y])*b[y]

②a[x]-(p+c[x]+c[y])*b[x]+a[y]-(p+c[y])*b[y]

消去相同的项后得

①c[x]*b[y]

②c[y]*b[x]

所以

对这两个式子化简，得到①＞②的条件是c[x]*b[y]<c[y]*b[x]

发现只要满足这个条件的物品对(x,y)，x在y前的代价永远更优


------------
再然后很多人都错在了没开long long上

其实本来也没有这个坑,踩的人多了就成了坑

对于某个食物i,在时刻t完成的价值是ai-t*bi

显然,当**t>ai/bi**时,价值<0

而选择不做这个食物所获的价值为0

所以对每个食物进行背包的状态更新时,我们只需要枚举的下标范围缩小到**`[ ci, min(T, ⌊ai/bi⌋) ]`** (ai/bi向下取整)

转移方程``dp[j] = max(dp[j], dp[j-ci]+ai-j*bi)``

此时`j*bi <= (ai/bi)bi = ai <= 100000`

就不会爆了

~~(以上纯属搞笑)~~

再者还有一个爆int的地方是排序时ci*bi

本人亲身体验这里不会爆(逃)


------------
下面就是理论上不完美但是能AC还附魔一点优化的代码

```cpp
#include <cstdio>
#include <iostream>
#include <algorithm>

using namespace std;

const int Maxn = 60;
const int Maxt = 1e5+7;

int T, n, res;
int dp[Maxt];

struct Food
{
	int a, b, c;
	bool operator < (const Food &nex) const
	{
		return b*nex.c > c*nex.b;
	}
} f[Maxn];

int main()
{
	scanf("%d%d", &T, &n);
	for(int i = 1; i <= n; ++i) scanf("%d", &f[i].a);
	for(int i = 1; i <= n; ++i) scanf("%d", &f[i].b);
	for(int i = 1; i <= n; ++i) scanf("%d", &f[i].c);
	sort(f+1, f+n+1);
	for(int i = 1; i <= n; ++i)
		for(int j = min(T, f[i].a/f[i].b); j >= f[i].c; --j)
			dp[j] = max(dp[j], dp[j-f[i].c]+f[i].a-j*f[i].b);
	for(int i = 1; i <= T; ++i) res = max(res, dp[i]);
	printf("%d\n", res);
	return 0;
}

```
