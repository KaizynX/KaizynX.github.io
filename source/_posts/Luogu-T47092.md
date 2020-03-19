---
title: 作业
date: 2018-10-02 15:50:00
categories:
  - Luogu
tags:
  - DP
  - 状压DP
  - 位运算
mathjax: true
---
# [Luogu T47092 作业](https://www.luogu.org/problemnew/show/T47092)

来自比赛[ARFA 教堂的第一次洗礼](https://www.luogu.org/problemnew/show/T47092)

## 思路

看着数据范围,大概就是 O(2^n) 暴力枚举了

一开始就想着怎么二进制骚操作,但一些地方就是不知道怎么处理

最后还是先打了一遍暴力看看

暴力就是用 dfs

大致如下

```cpp
// cur 用二进制表示当前选了哪些, last表示上一个选的
void dfs(int cur, int last)
{
    for(int i = 0; i < n; ++i)
    {
        // this one had not been chosen and satisfy the request of a[]
        if((cur&(1<<i)) == 0 && a[i] >= a[last])
        {
            int cnt = 0;
            // 处理 b[] 的条件
            for(int j = last+1; j < i; ++j)
                if((cur&(1<<j)) == 0) ++cnt;
            if(cnt > b[last]) continue;

            if(++dp[cur|(1<<i)] >= MOD) dp[cur|(1<<i)] %= MOD;
            dfs(cur|(1<<i), i);
        }
    }
}
```

上面代码中处理 b 的条件时候写错了

因为不一定 上一个选的小于现在选的

这个小错误曾困扰了我通往满分

然后事实上这样的暴力已经可以拿 70~90 分了

（我拿了70分,因为上述错误WA了2个,T了一个.但是根据作者说,他卡了3个)

再看看二进制枚举时候的难点吧

总体思路是

枚举当前做了的作业数量x

枚举做了 x 份作业的方案(究竟是哪 x 份)

找到下一份满足要求的作业,更新状态

*语无伦次*

看赛场上的丑陋代码

```cpp
#include <cmath>
#include <cstdio>
#include <iostream>

using namespace std;

const int Maxn = 19;
const int MOD = 4921057;


// dp[i][j] 表示状态 i(二进制)下最后一个选了 j 的方案数
int n, a[Maxn], b[Maxn], dp[1<<Maxn][Maxn], m[1<<Maxn];

int main()
{
    // 输入
    scanf("%d", &n);
    for(int i = 0; i < n; ++i) scanf("%d", a+i);
    for(int i = 0; i < n; ++i) scanf("%d", b+i);
    // 求最大值
    // 注释 ①
    for(int i = 1; i < (1<<n); ++i)
        for(int j = 0; j < n; ++j)
            if(i&(1<<j)) { m[i] = max(a[j], m[i^(1<<j)]); break; }

    // 初始化,第一份作业写了啥
    for(int i = 0; i < n; ++i) dp[1<<i][i] = 1;
    // 枚举写了几份作业
    for(int i = 1; i < (1<<(n-1)); i = i<<1|1)
    {
        int cur = i, x, y;
        while(cur < (1<<n))
        {
            // 选择下一个要写的作业
            for(int j = 0; j < n; ++j)
                // 第 j 份作业没写并且满足 a[] 递增
                // 因为递增,后选的 a[] 一定比之前选的都大
                if(!(cur&(1<<j)) && a[j] >= m[cur])
                    // 找当前(cur)最后一个选的
                    for(int k = 0; k < n; ++k)
                        // 因为递增,最后一个选的一定是当前(cur)最大的
                        if(cur&(1<<k) && a[k] == m[cur])
                        {
                            // 判断是否满足 b[] 的要求
                            int cnt = 0;
                            if(k < j)
                                for(int p = k+1; p < j; ++p)
                                {
                                    if(!(cur&(1<<p))) ++cnt;
                                }
                            else 
                                for(int p = j+1; p < k; ++p)
                                {
                                    if(!(cur&(1<<p))) ++cnt;
                                }
                            // 状态转移
                            if(cnt <= b[k])
                                dp[cur|(1<<j)][j] = (dp[cur][k]+dp[cur|(1<<j)][j])%MOD;
                        }
            // 写了这[几]份作业的下一种方案(这[几]种,那[几]种)
            // 注释 ②
            x = cur&-cur;
            y = cur+x;
            cur = ((cur&~y)/x>>1)|y;
        }
    }
    // 最后只要满足要求,可以是任意一个作为 最后选的
    int ans = 0;
    for(int i = 0; i < n; ++i)
        if(a[i] == m[(1<<n)-1])
            ans = (ans+dp[(1<<n)-1][i])%MOD;
    printf("%d\n", ans);
    return 0;
}
```

详细的解释一下代码

注释 ①

```cpp
    for(int i = 1; i < (1<<n); ++i)
        for(int j = 0; j < n; ++j)
            if(i&(1<<j)) 
            {
                m[i] = max(a[j], m[i^(1<<j)]);
                break;
            }
```
这里是处理最大值

因为我们排出的 a[] 是单调递增的,所以求出最大值有利于判断那个啥的

当前做了的作业是 i (二进制表示)

随便找一个做了的作业 `i&(1<<j) == 1`

`i^(1<<j)` 就是去掉这份作业 (~~基础的~~二进制骚操作)

然后这个状态的最大值就是 `max(a[j], i去掉j的最大值)`

可以~~显而易~~证明

因为 i 是从小到大枚举的, 但 i 少了一个 1 之后的状态

一定是之前已经处理过的,所以转移没有问题

当然我现在想到一种不用那么麻烦的想法

那就是直接去掉 `lowbit`

这样省去了一重枚举,复杂度降低了一维 ~~对整体复杂度还是无影响~~

注释 ②

这可能是最骚的操作了

二进制的一种骚操作

用来枚举集合中 选固定个数的 所有子集

*借鉴自 《挑战程序设计竞赛(第二版)》 P156 专栏 集合的整数表示*

枚举 集合 中所包含的所有 大小为 k 的子集 的方法

```cpp
int comb = (1 << k) - 1;
while (comb < 1 << n) {
    int x = comb & - comb,
        y = comb + x;
    comb = ((comb & ~y) / x >> 1) | y;
}
```

思路大致如下:

1. 求出最低为的 1 开始的连续的 1 的区间 (`0100 1110` -> `0000 1110`)
2. 将这一区间全部变为 0, 并且区间左侧的那个 0 变为 1 (`0100 1110` -> `0101 0000`)
3. 将第(1)步里取出的区间右移, 直到剩下的 1 的个数减少了 1 个 (`0000 1110` -> `0000 0011`)
4. 将第(2)步和第(3)步的结果按位取或 (`0101 0000 | 0000 0011` -> `0101 0011`)

解释代码

`x = comb & - comb` 是 lowbit, 也就是最低位上的 1

`y = comb + x` 最低位加上1,最低位进位

那么假如最后是连续的1,就会一直进位,进位至末尾全部连续的1 

就是将 comb 从最低为的 1 开始的连续的 1 都置 0 了

y 取反再和 comb 按位与 就得到最低为 1 开始的连续区间

`/ x` 是个很神奇的操作

这个操作使得 y 的 1 右移到最低位

再 `>> 1` 就完成了步骤 3

然后就没然后了