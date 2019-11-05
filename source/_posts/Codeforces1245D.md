---
title: CF1245D. Shichikuji and Power Grid
date: 2019-11-05 18:40:00
categories:
  - Codeforces
tags:
  - 最小生成树
  - 并查集
---

## [题面](https://codeforces.com/contest/1245/problem/D)

## 思路

设电站为节点 0

把在 i 建设电站当成链接 0 和 i

就是简单的最小生成树问题

## 代码
```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 2e3+7;

struct Node
{
    int x, y;
} a[N];

struct Edge
{
    int i, j;
    long long w;
    Edge(){}
    Edge(int _i, int _j, long long _w) : i(_i), j(_j), w(_w) {}
    friend bool operator < (const Edge &x, const Edge &y) {
        return x.w < y.w;
    }
};

int n;
int c[N], k[N], fa[N], choose[N];
vector<Edge> v, ans;

inline int dis(int i, int j)
{
    return abs(a[i].x-a[j].x)+abs(a[i].y-a[j].y);
}

int getf(int x) {
    return fa[x] == x ? x : fa[x] = getf(fa[x]);
}

int main()
{
    cin >> n;
    for (int i = 1; i <= n; ++i) {
        cin >> a[i].x >> a[i].y;
    }
    for (int i = 1; i <= n; ++i) {
        cin >> c[i];
        v.push_back(Edge(0, i, c[i]));
    }
    for (int i = 1; i <= n; ++i) {
        cin >> k[i];
        for (int j = 1; j < i; ++j) {
            v.push_back(Edge(i, j, 1ll*(k[i]+k[j])*dis(i, j)));
        }
    }
    sort(v.begin(), v.end());
    for (int i = 0; i <= n; ++i) fa[i] = i;
    long long res = 0;
    int cntc = 0, cnte = 0, cnt = 0;
    for (auto e : v) {
        int fi = getf(e.i), fj = getf(e.j);
        if (fi == fj) continue;
        fa[fi] = fj;
        res += e.w;
        if (e.i == 0) {
            choose[e.j] = 1;
            ++cntc;
        } else {
            ans.push_back(e);
            ++cnte;
        }
        if (++cnt >= n) break;
    }
    cout << res << endl;
    cout << cntc << endl;
    for (int i = 1; i <= n; ++i) {
        if (choose[i]) cout << i << " ";
    }
    cout << endl;
    cout << cnte << endl;
    for (auto e : ans) {
        cout << e.i << " " << e.j << endl;
    }
    return 0;
}
```