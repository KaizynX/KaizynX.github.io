---
title: Codeforces Round 510 (Div. 2) 
date: 2018-09-18 13:04:00
tags: 
  - Codeforces
  - OI
  - 数学
mathjax: true
---

## 前言

这是一场惨痛的翻车案

# [A. Benches](http://codeforces.com/contest/1042/problem/A)

## 题意

有n个沙滩,第i个沙滩原本有ai个人

后来来了m个人,每人去往任意一个沙滩

最终所有人数最多的一个沙滩的人数记为k

求k的最小值和最大值

## 数据范围

1 <= n <= 100

1 <= m <= 10000

1 <= ai <= 100

## 思路

一眼看出的水题

最大的k就是m个人都去原本人最多的沙滩

$k_{max} = \max (ai) + m$

那么问题就是最少

我的第一想法就是均摊嘛

原本人少的多去几个,原本人多的少去几个

尽量让每个沙滩的人数平均

答案就是均摊量

设sum是ai的和,也就是原本沙滩上的总人数

`k = (sum+m)/n+((sum+m)%n != 0))`

能均摊就`(sum+m)/n`

不能均摊就是`(sum+m)/n+1`

**然后**

就被Hack了

看这样一组数据:

---
输入

    3
    2
    5 0 0

我的输出

    3 7

正确答案

    5 7

---

发现了并不一定能"均摊"

调整为`k = max(maxa, (sum+m)/n+((sum+m)%n != 0))`

正确代码如下

```cpp
#include <cstdio>
#include <iostream>

using namespace std;

int n, m, sum, maxa, a;

int main()
{
	scanf("%d%d", &n, &m);
	for(int i = 1; i <= n; ++i)
	{
		scanf("%d", &a);
		maxa = max(a, maxa);
		sum += a;
	}
	printf("%d %d\n", max(maxa, (sum+m)/n+((sum+m)%n != 0)), maxa+m);
	return 0;
}
```
---

# B. [Vitamins](http://codeforces.com/contest/1042/problem/B)

## 题意

商店卖n种饮料,每种饮料有一种或多种维生素(A,B,C),价格为ci

问得到A,B,C三种最少要多少钱

ps:输入保证只出现A,B,C且每种最多出现一次

## 数据范围

1 <= n <= 1000

1 <= ci <= 100000

## 思路

这个看着其实挺简单的呀

用头想都知道怎么办

可是想敲代码又愣住了

怎么敲

还是**暴力**

想要ABC,可以有一下凑法

	A+B+C
	AB+C
	AC+B
	BC+A
	ABC

假如A的饮料有k个

我们选的肯定是k个中价值最小的

所以只需要一个简单数组记录各种饮料的最低价格

再配凑出ABC

饮料的种类可分为以下几种

	A
	B
	C
	AB
	AC
	BC
	ABC

很暴力了

但这样有些太过麻烦了

于是想到了二进制优化

假如把A,B,C看成以下数字

	A == 1
	B == 2
	C == 4

这样形成的组合就不会有重复

那么1~7就能完美覆盖所有饮料的种类

以下是我的代码

```cpp
#include <cstdio>
#include <cstring>
#include <iostream>

using namespace std;

const int INF = 1e8+7;

int n, c[8];
string str;

// a 1 b 2 c 4
// ab 3 ac 5 bc 6 abc 7

int main()
{
	for(int i = 1; i <= 7; ++i) c[i] = INF;
	cin >> n;
	for(int i = 1, tmp, k; i <= n; ++i)
	{
		cin >> tmp >> str;
		k = 0;
		for(int i = 0; i < str.length(); ++i)
			k += 1<<(str[i]-'A');
		c[k] = min(c[k], tmp);
	}
	int res = c[7];
	res = min(res, c[1]+c[2]+c[4]);
	res = min(res, c[3]+c[4]);
	res = min(res, c[5]+c[2]);
	res = min(res, c[6]+c[1]);
	res = min(res, c[3]+c[5]);
	res = min(res, c[3]+c[6]);
	res = min(res, c[5]+c[6]);
	printf("%d\n", res == INF ? -1 : res);
	return 0;
}
```

没错,你们发现有坑点了吗

想组合出ABC其实还有3种方法

	AB+AC
	AB+BC
	AC+BC

而`AB+AC+BC`是显然不行的啦

就是因为忘了这三种我第一次提交错了

后来,没错,我的B题也被Hack了!

原因是我一开始INF定义为c的最大值1e5

如果有这样的饮料

	A 1e5
	B 1e5
	C 1e5

按照我的做法得出答案1e5

其实是3e5 唉

---

# C. [Array Product](http://codeforces.com/contest/1042/problem/C)

这题无疑让我最头疼

## 题意

给n个数字ai

操作1: 把aj = ai*aj,删去ai

操作2: 删去ai(只能进行一次)

n-1次操作后只剩1个数字

求使最后数字最大的方案

## 数据范围

2 <= n <= 2*10^5

-10^9 <= ai <= 10^9

## 思路

思路还算简单

以下进行分类讨论

1. 对于正数

	反正全部乘起来就对了,因为都>=1

	所以对于任意个数的正数不做考虑

2. 对于0(没有负数的情况)

	我们知道任何数乘0得到0

	所以如果有0,结果会是0小于正数

	因此我们选择删去0(操作2)

	而且无论任意数量的0

	都可以两两相乘(操作1)得到1个0

3. 对于负数(没有0)

	- 假如有偶数个负数

		因为负负得正

		视为正数,无脑相乘
	
	- 假如有奇数个负数

		全部相乘一定为负

		我第一次想法是两两相乘再删掉

		其实正确做法是:

		删去一个乘积就是正数

		那么删去哪个呢

		要使剩下偶数个乘起来最大

		就要剩下的负数的绝对值最大

		意思是删去绝对值最小的

		因为是负数比较

		等价于删去负数中值最大的

4. 如果同时存在0和负数

	这是我第三次提交了

	之前两次是考虑错了这种情况

	因为0可以看成一个0

	- 负数有偶数个
	
		相乘为正数,不特殊考虑

		故删去0
	
	- 负数有奇数个
  
		第一想法是乘0和乘负数

		嗯,删去负数

		于是乎我就错了2次了

		于是又仔细想想

		发现可以这样操作

		把0和多出的负数相乘得到0

		再删去这个0

		两全其美

我想这应该就是正确思路了

如上所示分类也挺麻烦的

因此代码也是相当暴力,敲的贼长,也贼烦

如果你们看了样例

那么在实践过程中还会发现

我们需要记录一下进行了几步,是否会超出n-1

这个超出特判是在进行操作2的时候

因为我们的方案像把0相乘到一个0再删去

如果数据全是0,那么最后结果就是相乘的一个0

并不需要删除操作

赛后我看AC代码,感觉也确实如此的思路~~吧~~

可我就是WA了

C题3WA滚粗(至此不知所错)

以下是我赛后稍加思考改进的代码

起码比赛时的超暴力好看些

注意:是不知所错的代码

```cpp
#include <cstdio>
#include <iostream>
#include <algorithm>

using namespace std;

const int Maxn = 2e5+7;

// 00 1+ 2-
int n, cnt[3], pos[3], step;
pair<int, int> a[Maxn];

int main()
{
	scanf("%d", &n);
	for(int i = 1; i <= n; ++i)
	{
		scanf("%d", &a[i].first);
		a[i].second = i;
		if(a[i].first == 0) cnt[0]++;
		else if(a[i].first > 0) cnt[1]++;
		else cnt[2]++;
	}
	sort(a+1, a+n+1);
	for(int i = 1; i <= n; ++i)
	{
		if(a[i].first == 0 && !pos[0]) pos[0] = i;
		else if(a[i].first > 0 && !pos[1]) pos[1] = i;
		else if(a[i].first < 0 && !pos[2]) pos[2] = i;
	}

	while(step < n-1 && cnt[0] > 1)
	{
		printf("1 %d %d\n", a[pos[0]].second, a[pos[0]+1].second);
		pos[0]++; cnt[0]--; step++;
	}
	// now cnt[0] <= 1
	if(step < n-1 && cnt[2]&1)
	{
		if(cnt[0])
		{
			printf("1 %d %d\n", a[pos[0]].second, a[pos[2]+cnt[2]-1].second);
			cnt[2]--; step++;
			if(step < n-1)
			{
				printf("2 %d\n", a[pos[0]].second);
				cnt[0]--; step++;
			}
		}
		else
		{
			printf("2 %d\n", a[pos[2]+cnt[2]-1].second);
			cnt[2]--; step++;
		}
	}
	// now cnt[2]&1 == 0
	if(step < n-1 && cnt[0])
	{
		printf("2 %d\n", a[pos[0]].second);
		cnt[0]--; step++;
	}
	// now cnt[0] == 0
	while(step < n-1 && cnt[2] > 1)
	{
		printf("1 %d %d\n", a[pos[2]].second, a[pos[2]+1].second);
		pos[2]++; cnt[2]--; step++;
	}
	// now cnt[2] <= 1
	while(step < n-1 && cnt[1] > 1)
	{
		printf("1 %d %d\n", a[pos[1]].second, a[pos[1]+1].second);
		pos[1]++; cnt[1]--; step++;
	}
	// now cnt[0] == 0, cnt[1] <= 1, cnt[2] <= 1
	if(step < n-1)
		printf("1 %d %d\n", a[pos[1]].second, a[pos[2]].second);
	return 0;
}
```

再以下是我**借鉴**AC代码的代码

```cpp
#include <stack>
#include <cstdio>
#include <iostream>

using namespace std;

const int Maxn = 2e5+7;
const int INF = 1e9+7;

int n, nega = -INF, negid, cnt, negn;
stack<int> s, zero;

int main()
{
	scanf("%d", &n);
	for(int i = 1, a; i <= n; ++i)
	{
		scanf("%d", &a);
		if(a == 0) zero.push(i);
		else
		{
			if(a < 0) negn++;
			if(a < 0 && a > nega)
			{
				nega = a;
				negid = i;
			}
			s.push(i);
		}
	}
	int tmp;
	while(zero.size() > 1)
	{
		tmp = zero.top(); zero.pop();
		printf("1 %d %d\n", tmp, zero.top());
		cnt++;
	}
	if(negn&1)
	{
		if(zero.size())
		{
			printf("1 %d %d\n", zero.top(), negid);
			cnt++;
			zero.pop();
		}
		if(cnt < n-1)
		{
			printf("2 %d\n", negid);
			cnt++;
		}
	}
	if(cnt < n-1 && zero.size())
	{
		printf("2 %d\n", zero.top());
		cnt++;
	}
	if(cnt >= n-1) return 0;
	while(s.size() > 1 && cnt < n-1)
	{
		tmp = s.top(); s.pop();
		if(negn&1 && tmp == negid)
		{
			tmp = s.top(); s.pop();
		}
		if(negn&1 && s.top() == negid)
		{
			s.pop();
		}
		printf("1 %d %d\n", tmp, s.top());
		cnt++;
	}
	return 0;
}
```

# D. [Petya and Array](http://codeforces.com/contest/1042/problem/D)

这题别看我写出来了,其实是在群里py来的

我因被Hack被大佬看重

他从我这寻找Hack点

我交易得到D题代码

实在是有些罪过啊

## 题意

有长度n的数组ai

求有多少个区间的区间和小于t

## 数据范围

1 <= n <= 200000

|t| <= 2*10^14

|ai| <= 10^9

## 思路

当时感到爆炸,而且精力还放在C题找错上

就~~抄~~了一遍代码交上去了

我们用sum[]表示前缀和

区间`[i+1,j],(i<j)`和就是`sum[j]-sum[i]`

我们固定右端点为j

求`sum[j]-sum[i] < t`

即`sum[j]-t < sum[i]`

即求多少个`i (0<i<j)`的sum满足上述条件

我们用树状数组tr[x]

记录sum小于x的个数

每次扩大右端点j后,更新tr[]

即加入sum[j]

然后对于每个右端点j

查询`> sum[j]-t`的个数即可

由于查询`>`并不方便

不妨查询`<= sum[j]-t`

求出不满足`< t`的的区间个数

显然可得区间总数为`n*(n+1)/2`

另外由于数据范围贼大

需采用离散化

最终代码如下

```cpp
#include <cstdio>
#include <iostream>
#include <algorithm>

using namespace std;

const int Maxn = 2e5+7;

int n, a[Maxn], tr[Maxn], tot;
long long t, ans, sum[Maxn], dis[Maxn];

inline void update(int x)
{
	for( ; x <= n; x += x&-x) tr[x]++;
}

inline int query(int x)
{
	int res = 0;
	for( ; x; x -= x&-x) res += tr[x];
	return res;
}

int main()
{
	scanf("%d%I64d", &n, &t);
	for(int i = 1; i <= n; ++i)
	{
		scanf("%d", a+i);
		dis[i] = sum[i] = sum[i-1]+a[i];
	}
	dis[n] = 0;
	sort(dis+1, dis+n+1);
	tot = unique(dis+1, dis+n+1)-(dis+1);
	for(int i = 1, cur; i <= n; ++i)
	{
		cur = lower_bound(dis+1, dis+tot+1, sum[i-1])-dis;
		update(cur);
		cur = upper_bound(dis+1, dis+tot+1, sum[i]-t)-(dis+1);
		ans += query(cur);
	}
	printf("%I64d\n", ((n+1ll)*n)/2-ans);
	return 0;
}
```
---

总之某种意义上说我是一题也没写出来

前两题的教训告诉我,不能太急

再者比赛16:05~18:05

光荣地牺牲了晚餐