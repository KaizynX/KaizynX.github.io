---
title: 矿脉开采
date: 2018-10-01 21:46:00
categories:
  - Luogu
tags:
  - 暴力优化
  - 搜索
mathjax: true
---

# [Luogu T46446 矿脉开采](https://www.luogu.org/problemnew/show/T46446)

来自洛谷比赛 [ETO的公开赛](https://www.luogu.org/contestnew/show/10817) 的 A 题

## 思路

看这个数据范围像暴力啊

交了个 $O(2^n)$ 的算法(难道这不是正确算法?)

妥妥 T 了一个点,故意的吧

虽然只有一个点,但是出题人恶心得设成了

*这道题是“签到”题，故开启计分策略，通过所有测试点方可得分（大佬其实可以跳过）*

这几分对于蒟蒻的我来说怎么放得下!!!

再看这个数据范围 46

如果减去一半,那么 $O(2^n)$ 就可以了

这个技巧还是应该有碰到过的

就是把 n 分成两份

分别进行 $O(2^n)$ 算法,再合起来

问题就在于怎么合起来了

其实就是分成两份

每一份求出可以平凑出的利润

遍历某一份中的利润

假设当前这份中有个利润为 $v_i$

那么在另外一份中找到最大的那个 $\leq S - v_i$ 的

这样保证两者结合最接近 $S$

这个过程可以用排序再二分优化

所以总的复杂度就是 $O(2^{\frac{N}{2}} \log 2^{\frac{N}{2}})$

还有一个优化就是 过程中出现 $\geq S$ 的值直接舍去

## 代码

```cpp
#include <bits/stdc++.h>

using namespace std;

const int Maxn = 50;
const int Maxm = (1<<23)+1;

int n, tot;
long long S, a[Maxn], ans;
long long b[2][Maxm];

void solve(int cur, int tail, long long sum, long long *arr)
{
    if(sum == S)
    {
        cout << S << endl;
        exit(0);
    }
    if(cur > tail)
    {
        ans = max(ans, sum);
        return;
    }
    if(sum+a[cur] <= S)
    {
        arr[++arr[0]] = sum+a[cur];
        solve(cur+1, tail, sum+a[cur], arr);
    }
    solve(cur+1, tail, sum, arr);
}

int main()
{
    cin >> S >> n;
    for(int i = 1, v, w; i <= n; ++i)
    {
        cin >> w >> v;
        a[++tot] = w;
        for(int i = 2; i <= v; ++i)
            a[tot] *= w;
        if(a[tot] > S) tot--;
        if(a[tot] == S)
        {
            cout << S << endl;
            return 0;
        }
    }
    solve(1, tot/2, 0, b[0]);
    solve(tot/2+1, tot, 0, b[1]);
    if(!b[0][0] || !b[1][0])
    {
        cout << ans << endl;
        return 0;
    }
    sort(b[0]+1, b[0]+b[0][0]+1);
    sort(b[1]+1, b[1]+b[1][0]+1);
    b[0][0] = unique(b[0]+1, b[0]+b[0][0]+1)-(b[0]+1);
    b[1][0] = unique(b[1]+1, b[1]+b[1][0]+1)-(b[1]+1);
    for(int i = 1; i <= b[0][0]; ++i)
    {
        // b[1][pos] > S-b[0][i]
        // b[1][pos-1] <= S-b[0][i]
        // b[0][i]+b[1][pos-1] <= S
        int pos = upper_bound(b[1]+1, b[1]+b[1][0]+1, S-b[0][i])-b[1];
        if(pos > 1) ans = max(ans, b[0][i]+b[1][pos-1]);
    }
    cout << ans << endl;
    return 0;
}
`cpp