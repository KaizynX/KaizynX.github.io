---
title: Oier们的幸运数字
date: 2018-09-13 10:17:00
tags:
  - Luogu
  - OI
  - 数学
  
mathjax: true
---

*[题目来源:洛谷比赛Oier们的开学祭](https://www.luogu.org/problemnew/show/T42830)*

~~*[转载来自题解](https://www.luogu.org/blog/Lixiuyu/Oiers-kai-xue-ji)*~~
## 题意

定义一个函数 $f(i)=i\times\sum_{j=1}^{i}[i\bmod j]$

即表示$f(i)$等于$i$乘以$i$的约数个数

给定$T$组$A,B$,判断$\sum_{i=A}^{B}f(i)$是奇数还是偶数

数据范围: $100\%$ 数据 $0\leq T \leq10^6, 1\leq A_i \leq B_i \leq 10^{18}$

## 思路

暴力分还是有的,至于拿满分...

其实我是不会写的,题解转载来的

---
首先我们可以把那两个式子写成这样:

$\sum_{i=A}^B\sum_{j|i} i$

其中$j|i$表示$j$为$i$的约数.

如果我们设$d(i)$表示i的约数个数,那么就是这样的式子:

$\sum_{i=A}^B i*d(i)$

<br>

1. **三十分做法**
   
    直接暴力枚举..

2. **五十分做法**
   
    可以先$N\sqrt{N}$ 
    来求出所有数的$i*d(i)$的值
    
    然后求一个前缀和,
    
    然后就可以O(1)处理每一个询问了..

3. **七十分做法**
   
    这里面的$d(i)$是一个很常见的积性函数
    
    所以可以用线性筛预处理出所有的$d(i)$d
    
    然后求出$i*d(i)$的前缀和,继续O(1)查询..

4. **满分做法**
   
    来推理一波..

    首先要让$\sum_{i=A}^B i*d(i)$

    $i∗d(i)$为奇数,我们在A~B中就需要有奇数个$i*d(i)$取值为奇数的值加起来

    然后要让$i∗d(i)$取值为奇数,则需要$i$和$d(i)$都为奇数

    如果要一个数的约数个数为奇数,显然需要是一个**完全平方数**
    
    因为约数都是成对出现的$(i$和$\sqrt{i})$
    
    所以只能构造出一对相同的数才能使约数个数为奇数

    所以使$i*d(i)$取值为奇数的数都是完全平方奇数
    
    现在问题就变成了求一个区间的完全平方奇数的个数是不是奇数了

    这个就把两个端点开个方,左端点向上取整,右端点向下取整
    
    然后算一下这中间的奇数有多少个就好了

```cpp
#include <cmath>
#include <cstdio>
#include <iostream>
#include <algorithm>

using namespace std;

int T;
long long A, B, a, b, res;

int main()
{
    scanf("%d", &T);
    while(T--)
    {
        scanf("%lld%lld", &A, &B);
        a = ceil(sqrt(A));
        b = floor(sqrt(B));
        if(a > b) res = 0;
        // the odd number between a and b
        else res = ((b-a+1)>>1)+(a&1 && b&1);
        puts(res&1 ? "Yes" : "No");
    }
    return 0;
}
```