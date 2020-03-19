---
title: CF1243D. 0-1 MST
date: 2019-11-11 20:15:00
categories:
  - Codeforces
tags:
  - 最小生成树
  - 数学

mathjax: true
---

## [题面](https://codeforces.com/contest/1243/problem/D)

## 思路

一看就是最小生成树,可是边好多啊

零边肯定先随便连,再连 1 的边

先删点,再跑最小生成树

删度最小的点,(边最少)

就是保留这个点,把这点有零边相连的点都删掉

假设最坏的情况是度都一样大

$n \times du \leq m$

$du = m/n$

剩下的点即为这个点的度,最坏情况也不超过 $10^3$ 级别

那么就可以跑最小生成树了

问题在于删点,删点其实相当于用 0 的代价把这些点都连上了

问题删了一波点,这波被删的点的 0 边所连点也要删去 (不妨递归?)

由于后边的删点只需判断 还没删的点 有没有和删的点有 1 边

第二波复杂度最大为 $(n-du)\times du == (n-m/n)\times m/n \leq 10^5$

所以可行

## 代码
```cpp
#include <bits/stdc++.h>
// #define DEBUG

using namespace std;

const int N = 1e5+7;

int n, m;
int flag[N];
vector<int> e[N];
queue<int> del;
set<int> p;
map<pair<int, int>, int> mp;

struct DSU
{
    int fa[N];
    void init(int sz) { for (int i = 0; i <= sz; ++i) fa[i] = i; }
    int get(int s) { return s == fa[s] ? s : fa[s] = get(fa[s]); }
    int& operator [] (int i) { return fa[get(i)]; }
    bool connect(int x, int y) {
        int fx = get(x), fy = get(y);
        if (fx == fy) return false;
        fa[fx] = fy; return true;
    }
} dsu;

#ifdef DEBUG
inline void print(vector<int> &v) {
    for (int i : v) cout << i << " ";
    cout << endl;
}
#endif

int main()
{
    cin >> n >> m;
    dsu.init(n);
    for (int i = 1, a, b; i <= m; ++i) {
        cin >> a >> b;
        e[a].push_back(b);
        e[b].push_back(a);
        mp.insert({{a, b}, 1});
        mp.insert({{b, a}, 1});
    }
    // rebuild map
    int mini = 1;
    for (int i = 2; i <= n; ++i) {
        if (e[i].size() < e[mini].size())
            mini = i;
    }
    // del
    flag[mini] = 1;
    for (int i : e[mini]) {
        p.insert(i);
        flag[i] = 1;
    }
    for (int i = 1; i <= n; ++i) {
        if (!flag[i])
            del.push(i);
    }
    vector<int> tmp;
    while (del.size()) {
        int i = del.front();
        del.pop();
        tmp.clear();
        for (int j : p) {
            if (mp.count({i, j})) continue;
            tmp.emplace_back(j);
        }
        for (int j : tmp) {
            flag[j] = 0;
            p.erase(j);
            del.push(j);
        }
    }
#ifdef DEBUG
    print(p);
#endif
    // build tree
    p.insert(mini);
    int cnt = 0, res = 0;
    for (int i : p) {
        for (int j : p) {
            if (i == j || mp.count({i, j})) continue;
            if (dsu.connect(i, j)) ++cnt;
        }
    }
    if (cnt >= (int)p.size()-1) {
        cout << 0 << endl;
        return 0;
    }
    for (int i : p) {
        for (int j : p) {
            if (i == j || !mp.count({i, j})) continue;
            if (dsu.connect(i, j)) {
                ++res;
                if (++cnt >= (int)p.size()-1) {
                    cout << res << endl;
                    return 0;
                }
            }
        }
    }
    return 0;
}
```