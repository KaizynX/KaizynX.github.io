---
title: 日常
date: 2018-10-06 16:10:00
categories:
  - Luogu
tags:
  - 数学
  - 排列组合
  - 错排
  - 乘法逆元
mathjax: true
---

# [Luogu  T44250 日常](https://www.luogu.org/problemnew/show/T44250)

来自比赛 [你的名字。 信息学奥林匹克](https://www.luogu.org/contestnew/show/10292) B题

本题官方[题解](https://www.luogu.org/blog/Taki/solution-daily)

## 思路

题目指向很明确

你只是需要点亮技能树而已

我们核心要求得是 n 个数中 k 个数错排的方案数

显然就是 从 n 个数中选出 k 个来错排

假设 $sf[k]$ 是 k 个数错排的方案数

那么结果就是 $C_n^{k} \times sf[k]$

排列组合的公式不必多说

但是直接求得复杂度达到 $O(n^2)$

### 排列组合优化

  参考[P3811 【模板】乘法逆元 题解](https://www.luogu.org/problemnew/solution/P3811)

1. 我们可以用前缀和处理出 $s[k] = k!$
  
    $C_n^k = \frac{n!}{k! \times (n-k)!}$

    $C_n^i = s[i] \times s[k]^{-1} \times s[n-k]^{-1}$ (乘法逆元)

    复杂度取决于你求乘法逆元的方法

    费马小定理 $O(n \log n)$

    解不定方程 欧几里得 不明...
  
2. 观察发现我们要求得排列组合下标都是 $n$

    那么只要求出 $C_n^{i} ,i \in (0, n)$

    那么我们可以采用另一种线性做法

    $C_n^{k-1} = \frac{n!}{(k-1)! \times (n-k+1)!}$

    $C_n^k = \frac{n!}{k! \times (n-k)!}$

    所以 $C_n^k = C_n^{k+1} \times \frac{n-k+1}{k}$

    进而再用乘法逆元

    $C_n^k = C_n^{k+1} \times (n-k+1) \times k^{-1}$

    这时候需要的乘法逆元范围就是 $(1, n)$

    所以可以采用线性递推的方法求乘法逆元

    最终复杂度优化为 $O(n)$

    ps:乘法逆元线性递推有点迷啊

### 错排

不知道的可以参考资料[洛谷日报 #52 [Planet6174]小学生都能看懂的错排问题解析](https://www.luogu.org/blog/P6174/post-cuo-pai)

我们可以考虑错排的递推公式

$sf[k] = (k-1) \times (sf[k-1] + sf[k-2])$

线性可求得

## 代码
加入了快读快写优化,因而显得冗长
```cpp
#include <bits/stdc++.h>

using namespace std;

const int Maxn = 1e6+7;
const int MOD = 993244853;

int n, ansi;
long long C[Maxn], inv[Maxn], sf[Maxn], ansv;

template <typename T> inline void read(T &);
void write(int);

inline void init()
{
	sf[0] = 1; sf[1] = 0; sf[2] = 1;
	for(int i = 3; i <= n; ++i)
		sf[i] = (sf[i-1]+sf[i-2])%MOD*(i-1)%MOD;

	inv[1] = 1;
	for(int i = 2; i <= n; ++i)
		inv[i]= (MOD-MOD/i)*inv[MOD%i]%MOD;

	C[0] = 1;
	for(int i = 1; i <= n; ++i)
		C[i] = C[i-1]*(n-i+1)%MOD*inv[i]%MOD;
}

int main()
{
	read(n);
	init();
	for(int i = 0, c; i <= n; ++i)
	{
		read(c);
		long long cur = sf[i]*C[i]%MOD*c%MOD;
		if(cur > ansv)
		{
			ansi = i;
			ansv = cur;
		}
	}
	write((int)ansv); putchar('\n');
	for(int i = 1; i <= n-ansi; ++i)
		write(i), putchar(' ');
	for(int i = n-ansi+1, flag = 1; i <= n-3*(ansi&1); ++i, flag = -flag)
		write(i+flag), putchar(' ');
	if(ansi&1)
	{
		write(n-1); putchar(' ');
		write(n);   putchar(' ');
		write(n-2);
	}
	putchar('\n');
	return 0;
}

template <typename T> inline void read(T &x)
{
	char c; T tag = 1;
	while(!isdigit((c=getchar()))) if(c == '-') tag = -1;
	x = c-'0';
	while(isdigit((c=getchar()))) x = (x<<1)+(x<<3) + c-'0';
	x *= tag;
}

void write(int x)
{
	if(x>9) write(x/10);
	putchar(x%10+'0');
}
```