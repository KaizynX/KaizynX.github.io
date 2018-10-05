---
title: 线索
date: 2018-10-05 13:30:00
categories:
  - Luogu
tags:
  - 线段树
  - 并查集
---

# [Luogu T44252 线索](https://www.luogu.org/problemnew/show/T44252)

来自比赛 [你的名字。 信息学奥林匹克](https://www.luogu.org/contest/show?tid=10292) A题

## 思路

一道新颖的区间修改

我是用 线段树+并查集 做的

总的来说不难

核心的一点是颜色修改时

后修改的要覆盖前修改的

也就是前修改的不能覆盖后修改的

这点线段树的 lazy tag 就不太好了

处理方法是 添加一个时间标记

只有当时间大于之前的时间时才修改

至于操作2的并查集,也不难

对于某个点的操作,就等价于对该点的父亲节点的操作

总之,就是代码有点长,思考+调试 共花去1小时

## 代码
```cpp
#include <bits/stdc++.h>

using namespace std;

const int Maxn = 1e5+7;

int n, m;
int fa[Maxn], sz[Maxn];

struct Color
{
    int c, t;
} a[Maxn];

struct Tree
{
    int l, r, c, t;
} tr[Maxn<<2];

inline int ls(int x) { return x<<1; }
inline int rs(int x) { return x<<1|1; }
int getf(int s) { return s == fa[s] ? s : fa[s] = getf(fa[s]); }

void connect(int l, int r, int x, int t)
{
    int fl = getf(l), fr = getf(r);
    if(fl != fr)
    {
        fa[fl] = fr;
        sz[fr] += sz[fl];
    }
    a[fr].c = x;
    a[fr].t = t;
}

void push_down(int i)
{
    int l = ls(i), r = rs(i);
    if(tr[l].t < tr[i].t)
    {
        tr[l].t = tr[i].t;
        tr[l].c = tr[i].c;
    }
    if(tr[r].t < tr[i].t)
    {
        tr[r].t = tr[i].t;
        tr[r].c = tr[i].c;
    }
}

void build_tree(int i, int l, int r)
{
    tr[i].l = l; tr[i].r = r;
    if(l == r) return;
    int mid = (l+r)>>1;
    build_tree(ls(i), l, mid);
    build_tree(rs(i), mid+1, r);
}

void update_tree(int i, int l, int r, int x, int t)
{
    if(l <= tr[i].l && r >= tr[i].r)
    {
        tr[i].c = x;
        tr[i].t = t;
        return;
    }
    push_down(i);
    int mid = (tr[i].l+tr[i].r)>>1;
    if(l <= mid) update_tree(ls(i), l, r, x, t);
    if(r > mid)  update_tree(rs(i), l, r, x, t);
}

void push_tree(int i)
{
    if(tr[i].l == tr[i].r)
    {
        int f = getf(tr[i].l);
        if(tr[i].t > a[f].t)
        {
            a[f].t = tr[i].t;
            a[f].c = tr[i].c;
        }
        return;
    }
    push_down(i);
    push_tree(ls(i));
    push_tree(rs(i));
}

int main()
{
    scanf("%d%d", &n, &m);
    // init
    build_tree(1, 1, n);
    for(int i = 1; i <= n; ++i) fa[i] = i, sz[i] = 1;

    for(int i = 1; i <= n; ++i) scanf("%d", &a[i].c);
    for(int i = 1, op, l, r, x; i <= m; ++i)
    {
        scanf("%d%d%d%d", &op, &l, &r, &x);
        if(op == 1) update_tree(1, l, r, x, i);
        else connect(l, r, x, i);
    }
    // calc the ans
    push_tree(1);
    for(int i = 1; i < n; ++i) printf("%d ", a[getf(i)].c);
    printf("%d\n", a[getf(n)].c);
    for(int i = 1; i < n; ++i) printf("%d ", sz[getf(i)]-1);
    printf("%d\n", sz[getf(n)]-1);
    return 0;
}
```