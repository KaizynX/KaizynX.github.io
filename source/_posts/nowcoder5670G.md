---
title: 2020牛客暑期多校训练营(第五场)G
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2020-08-08 22:30:00
mathjax: true
categories:
  - ACM
tags:
  - 网络流
  - 虚树
  - 线段树优化建图
  - 树链剖分
description: 牛客多校一道好难的题
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/nowcoder5670G/cover.jpg
---

# 序

好难一道题呜呜,补题补了半个月

**前置知识点:**

- 二分
  
- 网络流
  
- 虚树
  
- 线段树优化建图
  
- 树链剖分

# 题意

给 $n$ 个结点的树 ( $n \leq 20000$)，每个结点给定一个颜色 $c$

对于每个结点 $u$ 任选一个 $d_u$

假设以 $u$ 为根的子树中共有 $k_u$ 个颜色为 $d_u$ 的结点

那么这个结点的值为 $k_u\times d_u$

求所有结点 值 的 $MEX$ (最小的 不在集合内的 非负整数)

# 题解

## 暴力

容易想到(赛场上想到)这个数据范围甚至可以 $O(n^2)$ 暴力求出每个结点各种颜色有几个

枚举每种颜色，然后树上跑 dfs 即可

然后暴力网络流建图,二分答案,如果能跑满流即满足条件

因为是二分图,时间复杂度 $O(n\sqrt{m} \log{n})$

唯一的缺陷就是空间复杂度,也即边数 $m = n^2$

## 正解

### 废话

官方给出了可行的方案是**线段树优化建图** (什么玩意给我整蒙了)

但学习一下发现所谓的线段树优化建图并不难(吧

但我还是不会嘛,官方给出的题解也是感觉很大一部分都没讲

参考了牛客上的一巨佬用户题解,然后又去学了**虚树**(我怎么什么都不会)

咕咕了差不多半个月,王老师队出了题解,我总算开始懂了

### 正文

定义 **值** 为上述 $k_u\times d_u$

对于每种颜色建立虚树,每个结点到他父亲的区间 **[结点, 父亲)** 左闭右开区间

这个区间内结点的子树中当前颜色结点的数量相同,即这些点可以连向一种值

(这里不要忘了虚树顶端到树根节点1的连边)

此处即可线段树优化建图,在寻找 **[结点, 父亲)** 时可以用树剖往上跳

### 复杂度分析

虚树 时间 $O(n\log n)$ 空间 $O(n)$ 虚树的点数总和是 $O(n)$

树剖往上眺每个结点 $O(\log n)$ 总的复杂度 $O(n\log n)$

线段树优化建图总每条边 $O(\log n)$ 总的 $O(n\log^2n)$

网络流点数 $O(n)$ 边数 $O(n\log^2n)$ 复杂度$O(玄学)$

### 优化

这里有个奇妙的操作,即在网络流图中,对于树上每条重链

该重链中所有父亲结点向儿子结点连一条边(不包括重链底部的最后一个点)

如果在寻找 **[结点, 父亲)** 时树剖向上跳跳过了一条重链顶端

就可以把你在这条重链上起跳点往对应的值连INF边即可

就不必对整个区间线段树优化建图

为什么呢?因为起跳点到重链顶端这个区间的点都可以流向对应值

因为父亲往儿子建边,起跳点上面的点就可以流下来到起跳点再到对应值

而该重链上起跳点下面的点则不会流到对应值

另外对于二分,可以跑残余网络

也即当前二分点 $mid$ 满足条件的话,就在残余网络上加边继续跑

如果不满足,就重新建图,或者题解的方法是保存之前网络流图,然后回退(详情见代码

那么二分的这个 $\log n$ 复杂度应该是能去掉

### 再分析复杂度

除了没有跳过重链顶端的那次,也就是最后一次,最多只会有一次

除了这次用线段树优化建图 $O(\log n)$ 其他连边边数都是 $O(1)$

一次树剖跳 $O(\log n)$ 所以每个区间连的边数就是 $O(\log n)$

网络流点数 $O(n)$ 边数 $O(n\log n)$ 复杂度 $O(能过)$

## 小声逼逼

我引以为傲的 $ISAP$ 算法竟然被 $Dinic$ 吊打了至少**20**倍

不知道为什么 $ISAP$ 就妥妥 $TLE$ 呗, $Dinic$ 就跑贼快

果然网络流只有 $O(能过)$ 和 $O(不能过)$

牛客巨佬用户题解有张图挺好的,建议看下,详情见文末链接

# 代码

{% spoiler "代码" %}
```cpp
#include <bits/stdc++.h>

using namespace std;

const int INF = 0x3f3f3f3f;
const int N = 2e4+7;
const int NN = (int)log2(N)+3;

struct Dinic {
  typedef int T;
  struct EDGE {
    int v, nex;
    T w;
    EDGE(const int &_v, const int &_nex, const T &_w) : v(_v), nex(_nex), w(_w) {}
  };
  vector<EDGE> e;
  int n, s, t;
  T maxflow;
  int fir[N*3], dep[N*3], cur[N*3];
  T work(const int &_s, const int &_t) {
    s = _s; t = _t;
    T flow;
    while (bfs())
      while ((flow = dfs(s, INF)))
        maxflow += flow;
    return maxflow;
  }
  void init(const int &_n) {
    n = _n;
    e.clear();
    maxflow = 0;
    memset(fir, -1, sizeof(int)*(n+3));
  }
  void add_edge(const int &u, const int &v, const T &w) {
    e.emplace_back(v, fir[u], w); fir[u] = e.size()-1;
    e.emplace_back(u, fir[v], 0); fir[v] = e.size()-1;
  }
  bool bfs() {
    queue<int> q;
    memset(dep, 0, sizeof(int)*(n+3));
    q.push(s);
    dep[s] = 1;
    for (int i = 0; i <= n; ++i) cur[i] = fir[i];
    while (q.size()) {
      int u = q.front();
      q.pop();
      for (int i = fir[u], v; i != -1; i = e[i].nex) {
        v = e[i].v;
        if (dep[v] || !e[i].w) continue;
        dep[v] = dep[u]+1;
        if (v == t) return true;
        q.push(v);
      }
    }
    return false;
  }
  T dfs(const int &u, const T &flow) {
    if (!flow || u == t) return flow;
    T rest = flow, now;
    for (int &i = cur[u], v; i != -1; i = e[i].nex) {
      v = e[i].v;
      if (dep[v] != dep[u]+1 || !e[i].w) continue;
      now = dfs(v, min(rest, e[i].w));
      if (!now) {
        dep[v] = 0;
      } else {
        e[i].w -= now;
        e[i^1].w += now;
        rest -= now;
        if (rest == flow) break;
      }
    }
    return flow-rest;
  }
};
Dinic dinic, dinic2;

struct SegmentTreeGarph {
  struct TreeNode {
    int l, r, ls, rs;
    int len() const { return r-l+1; }
  } tr[N<<1];
  int tot, root;
  void build(const int &n) {
    tot = n;
    for (int i = 1; i <= n; ++i) tr[i].l = tr[i].r = i;
    build(1, n, root);
  }
  void build(const int &l, const int &r, int &i) {
    if (l == r) return i = l, void();
    i = ++tot;
    tr[i].l = l; tr[i].r = r;
    int mid = (l+r)>>1;
    build(l, mid, tr[i].ls);
    build(mid+1, r, tr[i].rs);
    dinic.add_edge(tr[i].ls, i, INF);
    dinic.add_edge(tr[i].rs, i, INF);
  }
  void insert(const int &o, int l, int r) {
    if (l > r) swap(l, r);
    insert(o, l, r, root);
  }
  void insert(const int &o, const int &l, const int &r, const int &i) {
    if (tr[i].l >= l && tr[i].r <= r) return dinic.add_edge(i, o, INF);
    int mid = (tr[i].l+tr[i].r)>>1;
    if (l <= mid) insert(o, l, r, tr[i].ls);
    if (r >  mid) insert(o, l, r, tr[i].rs);
  }
} stg;

struct HLD {
  int dfn;
  int fa[N], d[N], num[N], son[N], id[N], tp[N];
  vector<int> *e;
  template <typename E>
  void build(E *_e, const int &rt = 1) {
    e = _e;
    fa[rt] = dfn = 0;
    dfs1(rt);
    dfs2(rt);
  }
  void dfs1(const int &u = 1) {
    d[u] = d[fa[u]]+1;
    num[u] = 1;
    son[u] = 0;
    for (const int &v : e[u]) if (v != fa[u]) {
      fa[v] = u;
      dfs1(v);
      num[u] += num[v];
      if (num[v] > num[son[u]]) son[u] = v;
    }
  }
  void dfs2(const int &u = 1) {
    tp[u] = son[fa[u]] == u ? tp[fa[u]] : u;
    id[u] = ++dfn;
    if (son[u]) dfs2(son[u]);
    for (const int &v : e[u]) if (v != son[u] && v != fa[u])
      dfs2(v);
  }
  int lca(int x, int y) {
    while (tp[x] != tp[y]) {
      if (d[tp[x]] < d[tp[y]]) swap(x, y);
      x = fa[tp[x]];
    }
    return d[x] < d[y] ? x : y;
  }
  void add_edge(int o, int x, int y) {
    if (d[x] < d[y]) swap(x, y); // x is son of y
    while (tp[x] != tp[y]) {
      dinic.add_edge(id[x], o, INF);
      x = fa[tp[x]];
    }
    if (x != y) stg.insert(o, id[y]+1, id[x]);
  }
} hld;

int n, C, last_mid, last_mid2;
int c[N], vfa[N];
vector<int> e[N], ve[N], col[N];

// VirtualTree begin
void virtual_tree_clear(const int &u = 1) {
  for (const int &v : ve[u]) virtual_tree_clear(v);
  vfa[u] = 0;
  ve[u].clear();
}

// build a VirtualTree return the root
int virtual_tree_build(vector<int> &vset) {
  // id ==> dfn rank, d ==> depth
  static int stk[N], top;
  static int *id = hld.id, *d = hld.d;
  sort(vset.begin(), vset.end(), [&](const int &x, const int &y) {
    return id[x] < id[y];
  });
  top = 0;
  int x, z;
  for (const int &v : vset) {
    if (top && (z = hld.lca(v, stk[top])) != stk[top]) {
      x = stk[top--];
      while (top && d[stk[top]] > d[z]) {
        ve[stk[top]].emplace_back(x);
        vfa[x] = stk[top];
        x = stk[top--];
      }
      ve[z].emplace_back(x);
      vfa[x] = z;
      if (!top || stk[top] != z) stk[++top] = z;
    }
    stk[++top] = v;
  }
  x = stk[top--];
  while (top) {
    ve[stk[top]].emplace_back(x);
    vfa[x] = stk[top];
    x = stk[top--];
  }
  return x;
}
// VirtualTree end

// (k*d) ==> 2*n+(k*d)
int build_stg(const int &u = 1) {
  int sum = c[u] == C;
  for (const int &v : ve[u])
    sum += build_stg(v);
  if (sum*C < n)
    hld.add_edge(2*n+sum*C, u, vfa[u]);
  return sum;
}

void build_network() {
  dinic.init(n*3+2);
  stg.build(n);
  assert(stg.tot < n*2);
  for (C = 1; C <= n; ++C) if (col[C].size()) {
    int rt = virtual_tree_build(col[C]);
    build_stg(rt);
    virtual_tree_clear(rt);
  }
  int st = n*3+1, ed = n*3+2;
  dinic.add_edge(2*n, ed, 1); // k*d == 0
  for (int i = 1; i <= n; ++i) {
    dinic.add_edge(st, i, 1);
    dinic.add_edge(i, 2*n, 1);
    // dinic.add_edge(2*n+i, ed, 0); // key
    if (hld.fa[i] && hld.tp[i] != i)
      dinic.add_edge(hld.fa[i], i, INF);
  }
}

bool check(const int &mid) {
  int st = n*3+1, ed = n*3+2;
  while (last_mid < mid) dinic.add_edge(2*n+(++last_mid), ed, 1);
  return dinic.work(st, ed) >= mid+1;
}

void save_graph() {
  dinic2 = dinic;
  last_mid2 = last_mid;
}

void get_graph() {
  dinic = dinic2;
  last_mid = last_mid2;
}

signed main() {
  ios::sync_with_stdio(false); cin.tie(nullptr); cout.tie(NULL);

  cin >> n;
  for (int i = 2, p; i <= n; ++i) {
    cin >> p;
    e[p].emplace_back(i);
  }
  for (int i = 1; i <= n; ++i) {
    cin >> c[i];
    col[c[i]].emplace_back(i);
  }

  hld.build(e);
  int l = 0, r = n-1, mid;
  last_mid = 0;
  build_network();
  save_graph();
  while (l < r) {
    mid = (l+r+1)>>1;
    if (check(mid)) {
      l = mid;
      save_graph();
    } else {
      r = mid-1;
      get_graph();
    }
  }
  cout << l+1 << endl;
  return 0;
}
```

{% endspoiler %}

# 参考资料

## 线段树优化建图

[网上某博客](https://www.cnblogs.com/light-house/p/11761163.html)

## 题解

官方题解

[BonVoyage 大爹队的题解](http://www.koule2333.top:3000/s/HyE3utYev#G---Greetings-Souvenir)

[牛客用户 XLor 的题解](https://blog.nowcoder.net/n/e1c88f8d9d8a49b5b4c13dd2e93b1d0e)
