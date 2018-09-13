---
title: NOIP普及组2013 小朋友的数字
date: 2018-08-31 21:33:36
tags:
  - NOIP
  - DP
---

## 题意

~~这题目描述真是有点为难我胖虎啊~~

有$n$个人,每个人手上有一个数字$a_i$(可正可负)

规定每个人的$b_i$为 他前面(包括他)连续若干人(至少一人)的a的和的最大值

即$b_k=max\sum\limits_{i=l}^ra_i(1\leq l\leq r \leq k)$

~~怎么编了个公式看上去更不懂了~~

再规定$m_i$

1. $m_1=b_1$

2. 当$i > 1$
   
   $m_i$是他前面(不包括他)的**某一个人**的$b_i+m_i$的最大值

    即$m_k=max(m_i+b_i)(1 \leq i < k)$

求$m$的最大值取模$p$

数据范围: $1\leq n \leq 10^6, 1 \leq p \leq 10^9, abs(a_i) \leq 10^9$

## 思路

$b_i$其实很好求得啦

就是求i之前的**连续区间最大和**的问题(是这么叫的吧?)

可以在O(n)求得,详情**略**

再看$m_i$ 是前面最大的某一个,因为只有一个

所以可以用记录下来,每次更新,也是O(n)

所以思路也是十分清晰明了

再分析一下数据范围:

假设是最坏的情况

$b_n=\sum\limits_{i=1}^{n}{a_i}\leq10^6*10^9=10^{15}$

因而可以用long long记录

这里有一个误区,我本来认为

既然$m_k=max(m_i+b_i)(1 \leq i < k)$

$b_i$在long long范围内,那么$m_i$也ojbk了

先上代码吧(80分,还有20分爆long long)

```cpp
#include <cstdio>
#include <iostream>

using namespace std;

const int Maxn = 1e6+7;
const long long INF = 1e16;

int n, p;
int a[Maxn];
long long b[Maxn], m[Maxn];
long long cur, maxsum, ans = -INF;

int main()
{
    scanf("%d%d", &n, &p);
    for(int i = 1; i <= n; ++i)
        scanf("%d", a+i);
    m[1] = maxsum = b[1] = cur = a[1];
    if(cur < 0) cur = 0;
    for(int i = 2; i <= n; ++i)
    {
        cur += a[i];
        b[i] = maxsum = max(maxsum, cur);
        if(cur < 0) cur = 0;
    }
    for(int i = 2; i <= n; ++i)
    {
        m[i] = ans = max(ans, m[i-1]+b[i-1]);
    }
    ans = max(ans, m[1]);
    printf("%d\n", ans%p);
    return 0;
}
```
---

我们进一步分析

**证明**: $b_i$是单调递增的

对于$b_i$就表示截取$[1, i]$的数组a的最大连续区间和

由于分治思想~~什么~~包容性

当$j=i+1$时$b_j=max(b_i, b_i+a_j,a_j)$

即$j>i$有$b_j=max(b_i ,[i+1, j]$的某个区间和, $[1, j]$的某个区间和$)$

可以得出当$j>i$时$b_j \geq b_i$

即$b_i$是单调递增的

**证毕**

**再证明**: $m_i$也是是单调递增的

设$i < j,k=j+1$并且$m_j = m_i+b_i$

即设$m_i+b_i$是$[1,j-1]$中最大的一项

当计算$m_k$时,求$[1,k-1]$即$[1,j]$中最大的

如果$m_j+b_j > m_i+b_i$则选$j$

反之因为$i$是$[1,j-1]$最大的,所哟选$i$

而又因为$m_j = m_i+b_i$

所以只要$b_j>0$选$j$反之选$i$

再如果选了$i$那么$m_k=m_i+b_i=m_j$

如果选的是$j$那么$m_k=m_j+b_j=m_i+b_i+b_j$

此时$b_j>0$所以$m_k>m_j=m_i+b_i$

所以也可以说明$j>i$时$m_j \geq m_i$

即$m_i$是单调递增的

**证毕**

终于经过一番玄学分析,题目才变成标签所示的DP

$m_i=max(m_{i-1}, m_{i-1}+b_i)$

显然$[2,n]$中$m_n$是最大的

但是$m_1$是个特判

我们有$m_1*2=m_2\leq m_3\leq ...\leq m_n$

如果$m_1>0$那么就是一直大下去

问题就在当$m_1<0$时

$m_n \geq m_2 = m_1+b_1=2*b_1$

不难发现当存在$\sum b_i>abs(b_1)(2\leq i < n)$

答案就是$m_n$否则为$m_1$

有因为$m_1=b_1$所以$m_1$在long long范围内

因此这个判断可以在不爆long long的情况下进行

由于之前怕要比较大小关系之类的不敢轻易取模

现在看来没事了

~~其实不如直接上高精~~

讲是讲得很详细了,代码丑点不为过吧

这里用某些骚皮的操作把a数组和m数组省掉了

b也可以用滚动数组,不过懒

```cpp
#include <cstdio>
#include <iostream>

using namespace std;

const int Maxn = 1e6+7;

int n, p, a, flag;
long long b[Maxn], m, cur, maxsum, maxsumb;

int main()
{
	scanf("%d%d%d", &n, &p, &a);
	maxsum = b[1] = cur = a;
	if(a > 0) flag = 1;
	if(cur < 0) cur = 0;
	for(int i = 2; i <= n; ++i)
	{
		scanf("%d", &a);
		cur += a;
		b[i] = maxsum = max(maxsum, cur);
		if(cur < 0) cur = 0;
		if(i == 2) m = 2*b[1];
		else if(b[i-1] > 0)
		{
			m = (m+b[i-1])%p;
			if(!flag && (maxsumb += b[i-1]) >= -b[1]) flag = 1;
		}
	}
	if(flag) printf("%lld\n", m);
	else printf("%lld\n", b[1]%p);
	return 0;
}
```