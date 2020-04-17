---
title: 网络流
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2020-03-26 10:00:00
mathjax: true
categories:
  - ACM
tags:
  - 网络流
description: 网络流|费用流学习笔记
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/network_flows/cover.gif
---
# 序

引用hjt巨佬的语录，网络流主要在建图

<img src="https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/network_flows/hjt001.png" data-action="zoom">
<img src="https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/network_flows/hjt002.png" data-action="zoom">
<img src="https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/network_flows/hjt003.png" data-action="zoom">

# 一些概念

部分转载自[hjt巨佬博客](https://www.cnblogs.com/axiomofchoice/p/template.html#网络流)

- $c(u,v)$ 为 $u$ 到 $v$ 的容量，$f(u,v)$ 为 $u$ 到 $v$ 的流量，$f(u,v)<c(u,v)$
- $c[X,Y]$ 为 $X$ 到 $Y$ 的容量和，不包括 $Y$ 到 $X$ 的容量；$f(X,Y)$ 为 $X$ 到 $Y$ 的流量和，要减去 $Y$ 到 $X$ 的流量
- 费用流（最小费用最大流）：保证最大流后的最小费用
割：割 $[S,T]$ 是点集的一个分割且 $S$ 包含源点，$T$ 包含汇点，称 $f(S,T)$ 为割的净流，$c[S,T]$ 为割的容量
- 最大流最小割定理：最大流即最小割容量
- 求最小割：在最大流残量网络中，令源点可达的点集为 $S$，其余的为 $T$ 即可（但是满流边不一定都在 $S,T$ 之间）
- 简单割：割集中所有的边，都与s或t相连接。
- 最小点权覆盖集：在二分图中，对于每条边，两个端点至少选一个，求所选取的点最小权值和。
- 最大点权独立集：在二分图中，对于每条边，两个端点至多选一条边，求所选取的点的最大权值和。
- 最小点权覆盖集=最小割，最大点权独立集=总权值-最小点权覆盖集

# 网络流模板

见[模板](https://kaizynx.github.io/2018/11/04/template/#网络流)

# 实战

## 二分图匹配

设二分图一边为左，一边为右

从 s 向左边每个点连权为 1 的边 (保证左边每个最多被匹配一次)

左右之间的边权为 1 (其实任意)

从右边每个点向 t 连权为 1 的边 (保证右边每个最多被匹配一次)

### [loj6000「网络流 24 题」搭配飞行员](https://loj.ac/problem/6000)
#### 代码
{% spoiler "代码" %}
```cpp
int n, m;
ISAP<int> isap;

inline void solve() {
    cin >> n >> m;
    int s = 0, t = n + 1;
    isap.init(n + 1);
    for (int i = 1; i <= m; ++i) isap.add_edge(s, i, 1);
    for (int i = m + 1; i <= n; ++i) isap.add_edge(i, t, 1);
    for (int a, b; cin >> a >> b;) isap.add_edge(a, b, 1);
    cout << isap.work(s, t) << endl;
}

signed main() {
    ios::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);
    int testcase = 1;
    // cin >> testcase;
    for (int i = 1; i <= testcase; ++i) {
        solve();
    }
    return 0;
}
```
{% endspoiler %}

## 最大流

### [hdu3572 Task Schedule](http://acm.hdu.edu.cn/showproblem.php?pid=3572)

#### 题意
要完成 n 个任务，每个任务需要在某个时间段 [s, e] 执行 p 时间(不一定连续),同一时间可以同时执行 m 个任务， 求是否能完成所有任务

#### 分析
我们把时间看作流量，每个时间点一开始有 m 个流量

一个任务完成的条件就是是否有 p 流量流经

所以从每个任务往 T 连一条边权为 p 的边，判断最大流是否为 所有任务 p 之和

对于每个任务 [s, e] 中每个时间都可以给这个任务 1 点流量

#### 建图 
S 向每个时间点连边 权为 m

每个 [s, e] 向任务连边 权为1

每个任务向 T 连边 权为 p

#### 代码
{% spoiler "代码" %}
```cpp
ISAP<int> isap;

inline bool solve()
{
    int n, m;
    static int vis[N];
    scanf("%d %d", &n ,&m);
    int src = 0, dst = 500+n+1;
    memset(vis, 0, sizeof(int)*(500+n+3));
    isap.init(500+n+1);
    int sum = 0;
    for (int i = 1, p, s, e; i <= n; ++i) {
        scanf("%d %d %d", &p, &s, &e);
        isap.add_edge(src, 500+i, p);
        sum += p;
        for (int j = s; j <= e; ++j) {
            isap.add_edge(500+i, j, 1);
            vis[j] = 1;
        }
    }
    for (int i = 1; i <= 500; ++i) if (vis[i]) {
        isap.add_edge(i, dst, m);
    }
    return isap.work(0, dst) == sum;
}

int main()
{
    int T = 1;
    scanf("%d", &T);
    for (int i = 1; i <= T; ++i) {
        printf("Case %d: %s\n\n", i, solve() ? "Yes" : "No");
    }
    return 0;
}
```
{% endspoiler %}

### [hdoj2883 kebab](http://acm.hdu.edu.cn/showproblem.php?pid=2883)

#### 分析
一样的题型，只不过需要离散化，把时间点合成时间段

#### 代码
{% spoiler "代码" %}
```cpp
struct Node
{
    int s, n, e, t;
    friend istream& operator >> (istream &is, Node &nd) {
        return is >> nd.s >> nd.n >> nd.e >> nd.t;
    }
};

int n, m;
Node a[N];
vector<int> d;
ISAP<int> isap;

inline bool solve()
{
    d.clear();
    for (int i = 1; i <= n; ++i) {
        d.emplace_back(a[i].s);
        d.emplace_back(a[i].e);
    }
    sort(d.begin(), d.end());
    d.erase(unique(d.begin(), d.end()), d.end());
    int tm = d.size()-1, s = tm+n+1, t = tm+n+2;
    // time zone[i] =  [d[i-1], d[i]], i [1, tm]
    // custom [tm+1, tm+n]
    isap.init(tm+n+2);
    int sum = 0;
    for (int i = 1; i <= tm; ++i)
        isap.add_edge(s, i, (d[i]-d[i-1])*m);
    for (int i = 1; i <= n; ++i) {
        a[i].s = lower_bound(d.begin(), d.end(), a[i].s)-d.begin();
        a[i].e = lower_bound(d.begin(), d.end(), a[i].e)-d.begin();
        for (int j = a[i].s+1; j <= a[i].e; ++j)
            isap.add_edge(j, tm+i, INF);
            // isap.add_edge(j, tm+i, (d[j]-d[j-1])*min(m, a[i].n));
        isap.add_edge(tm+i, t, a[i].n*a[i].t);
        sum += a[i].n*a[i].t;
    }
    return isap.work(s, t) == sum;
}

signed main()
{
    ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
    d.reserve(N);
    while (cin >> n >> m) {
        for (int i = 1; i <= n; ++i) cin >> a[i];
        cout << (solve() ? "Yes" : "No") << endl;
    }
    return 0;
}
```
{% endspoiler %}

## 拆点

一般网络流对每条边有流量限制

遇到对点有流量限制,如结点 u 有限制 f

拆成两点一边 u->v 边权 f

### [hdoj2732 Leapin' Lizards](http://acm.hdu.edu.cn/showproblem.php?pid=2732)
#### 代码
{% spoiler "代码" %}
```cpp
int n, m, d, s, t;
char a[25][25], b[25][25];
ISAP<int> isap;

inline int mp(const int &x, const int &y) { return (x*m+y)<<1; }

inline void walk(const int &x, const int &y)
{
    int flag = 0;
    for (int i = -d, nx, ny; i <= d; ++i) {
        for (int j = abs(i)-d; j <= d-abs(i); ++j) {
            if (i == 0 && j == 0) continue;
            nx = x+i; ny = y+j;
            if (nx < 0 || ny < 0 || nx >= n || ny >= m) {
                if (!flag) isap.add_edge(mp(x, y)^1, t, INF), flag = 1;
                continue;
            }
            if (a[nx][ny] == '0') continue;
            isap.add_edge(mp(x, y)^1, mp(nx, ny), INF);
        }
    }
}

inline int solve()
{
    scanf("%d %d", &n, &d);
    for (int i = 0; i < n; ++i) scanf("%s", a[i]);
    for (int i = 0; i < n; ++i) scanf("%s", b[i]);

    int num = 0;
    m = strlen(a[0]);
    s = 2*n*m; t= 2*n*m+1;
    isap.init(2*n*m+2);
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < m; ++j) {
            if (a[i][j] != '0') isap.add_edge(mp(i, j), mp(i, j)^1, a[i][j]-'0');
            if (b[i][j] == 'L') isap.add_edge(s, mp(i, j), 1), ++num;
            walk(i, j);
        }
    }
    return num-isap.work(s, t);
}

signed main()
{
    int testcase = 1;
    scanf("%d", &testcase);
    for (int i = 1; i <= testcase; ++i) {
        int res = solve();
        printf("Case #%d: ", i);
        if (res == 0) puts("no lizard was left behind.");
        else if (res == 1) puts("1 lizard was left behind.");
        else printf("%d lizards were left behind.\n", res);
    }
    return 0;
}
```
{%endspoiler%}

### [codeforces 546E. Soldier and Traveling](https://codeforces.com/contest/546/problem/E)
#### 题意
给一张图，每个点有一些人，每个人只能最多移动 1 步

问能否使每个点的人数达到给定值
#### 思路
简单题,每个点拆成移动前和移动后
#### 代码
{% spoiler "代码" %}
```cpp
int n, m;
int res[N][N];
ISAP<int> isap;

signed main()
{
    ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
    cin >> n >> m;
    // turn pt i to i*2-1, i*2
    int s = 2*n+1, t = 2*n+2,
        suma = 0, sumb = 0;
    isap.init(t);
    for (int i = 1, a; i <= n; ++i) {
        cin >> a;
        suma += a;
        isap.add_edge(s, i*2-1, a);
        isap.add_edge(i*2-1, i*2, INF);
    }
    for (int i = 1, b; i <= n; ++i) {
        cin >> b;
        sumb += b;
        isap.add_edge(i*2, t, b);
    }
    for (int i = 1, u, v; i <= m; ++i) {
        cin >> u >> v;
        isap.add_edge(u*2-1, v*2, INF);
        isap.add_edge(v*2-1, u*2, INF);
    }
    if (suma != sumb || isap.work(s, t) != sumb) return cout << "NO\n", 0;
    for (int u = 1; u <= n; ++u) {
        for (int i = isap.fir[u*2], v; i != -1; i = isap.e[i].nex) {
            v = (isap.e[i].v+1)/2;
            res[v][u] = isap.e[i].w;
        }
    }
    cout << "YES\n";
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= n; ++j) {
            cout << res[i][j] << " \n"[j==n];
        }
    }
    return 0;
}
```

{%endspoiler%}

### [luoguP2153 [SDOI2009]晨跑](https://www.luogu.com.cn/problem/P2153)
#### 思路
费用流板子题,限制:路口只能走一次
#### 代码
{% spoiler "代码" %}
```cpp
int n, m;
ZKW_SPFA<int> zkw;

signed main()
{
    // pt i ==> i*2-1, i*2
    ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
    cin >> n >> m;
    zkw.init(n*2);
    for (int i = 2; i < n; ++i) zkw.add_edge(i*2-1, i*2, 1, 0);
    for (int i = 1, u, v, w; i <= m; ++i) {
        cin >> u >> v >> w;
        zkw.add_edge(u*2, v*2-1, 1, w);
    }
    pair<int, int> res = zkw.work(2, n*2-1);
    cout << res.first << " " << res.second << endl;
    return 0;
}
```

{%endspoiler%}

### [luoguP1251 餐巾计划问题](https://www.luogu.com.cn/problem/P1251)
#### 思路
把每天拆成早上(可用毛巾)晚上(待洗毛巾) // 详情见洛谷题解
#### 代码
{% spoiler "代码" %}
```cpp
int r[N];
ZKW_SPFA<long long> zkw;

signed main()
{
    // day i ==> i to wash | evening, i+n can use | morning
    ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
    int n, p, t1, m1, t2, m2;
    cin >> n;
    for (int i = 1; i <= n; ++i) cin >> r[i];
    cin >> p >> t1 >> m1 >> t2 >> m2;
    int s = 0, t = n*2+1;
    zkw.init(t);
    for (int i = 1; i <= n; ++i) {
        zkw.add_edge(s, i, r[i], 0);
        zkw.add_edge(i+n, t, r[i], 0);
        if (i+1 <= n) zkw.add_edge(i, i+1, INF, 0); // do not wash
        if (i+t1 <= n) zkw.add_edge(i, i+n+t1, INF, m1); // fast
        if (i+t2 <= n) zkw.add_edge(i, i+n+t2, INF, m2); // slow
        zkw.add_edge(s, i+n, INF, p); // buy new
    }
    cout << zkw.work(s, t).second << endl;
    return 0;
}
```

{%endspoiler%}

### 另见于 [有向无环图最小不相交路径覆盖](#有向无环图最小不相交路径覆盖)

## 拆边

动态费用

如果对于某条边，其费用是关于流量的一个函数，并且这个函数的斜率是单调增加的，我们就可以拆边，第x条费用设为𝑓(𝑥)−𝑓(𝑥−1)

### [hdoj3667 Transportation](http://acm.hdu.edu.cn/showproblem.php?pid=3667)
#### 题意
N个城市，M条边，你要从1号点运送k个货物到N号点。每条边(u,v,a,c)表示u->v有一条边容量为c，从这条边运送x个物品花费为 ，问最小花费为多少？

N <= 100, M <= 5000, c <= 5
#### 思路
对于每一条边，原本的容量为c，我们可以把它拆成c条边

这c条边的容量都为1，花费为1a,3a,5a,7a...(c<=5)

由于费用流会优先流花费较小的边，因此假设最终的流中我们的流量经过了前k小的边，花费就是1a+3a+..+2(k-1)a=ak^2，与题意相符
#### 代码
{% spoiler "代码" %}
```cpp
int n, m, k;
ZKW_SPFA<int> zkw;

signed main()
{
    ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
    while (cin >> n >> m >> k) {
        int s = 0, t = n;
        zkw.init(n);
        zkw.add_edge(s, 1, k, 0);
        for (int i = 1, u, v, a, c; i <= m; ++i) {
            cin >> u >> v >> a >> c;
            for (int j = 1; j <= c; ++j) {
                zkw.add_edge(u, v, 1, (2*j-1)*a);
            }
        }
        pair<int, int> res = zkw.work(s, t);
        cout << (res.first == k ? res.second : -1) << endl;
    }
    return 0;
}
```

{%endspoiler%}

## 虚点
其实没什么好讲的
### [luoguP1361 小M的作物](https://www.luogu.com.cn/problem/P1361)
#### 思路
考虑最小割

每个点向 S, T 连边，设 S 是种在 A, B 是种在 B, 通过最小割拆成两个图即为所求

对于每个组合，建立一个虚点往 A 连边权为额外收益的边，再往组合里的每个点连 INF 边

这样如果某个作物要种在 B (割掉与 A 连边),此时因为该点还通过组合与 A 有连边

所以如果不种在 A 此时组合的边必定被割掉

#### 代码
{% spoiler "代码" %}
```cpp
int n, m;
int a[N], b[N];
ISAP<int> isap;

signed main()
{
    ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
    cin >> n;
    for (int i = 1; i <= n; ++i) cin >> a[i];
    for (int i = 1; i <= n; ++i) cin >> b[i];
    cin >> m;
    // plant [1, n] combination n+i*2-1, n+i*2
    isap.init(n+m*2+2);
    int s = n+m*2+1, t = n+m*2+2, sum = 0;
    for (int i = 1, k, c1, c2; i <= m; ++i) {
        cin >> k >> c1 >> c2;
        isap.add_edge(s, n+i*2-1, c1);
        isap.add_edge(n+i*2, t, c2);
        sum += c1+c2;
        for (int j = 1, x; j <= k; ++j) {
            cin >> x;
            isap.add_edge(n+i*2-1, x, INF);
            isap.add_edge(x, n+i*2, INF);
        }
    }
    for (int i = 1; i <= n; ++i) {
        sum += a[i]+b[i];
        isap.add_edge(s, i, a[i]);
        isap.add_edge(i, t, b[i]);
    }
    cout << sum-isap.work(s, t) << endl;
    return 0;
}
```

{%endspoiler%}

## 最小点权覆盖和最大点权独立集
原二分图中的边(u,v)替换为容量为INF的有向边(u,v)，设立源点s和汇点t，将s和x集合中的点相连，容量为该点的权值；将y中的点同t相连，容量为该点的权值。求最小割
### [hdoj1565 方格取数(1)](http://acm.hdu.edu.cn/showproblem.php?pid=1565)
#### 题意
给你一个n*n的格子的棋盘，每个格子里面有一个非负数。从中取出若干个数，使得任意的两个数所在的格子没有公共边，就是说所取的数所在的2个格子不能相邻，并且取出的数的和最大。
#### 建图
黑白染色成二分图

S 连黑，权值为黑点权值

黑与相邻的白连，权值为 INF

白连 T, 权值白点权值
#### 分析
求最小割

那么就不可能割 INF 边，反证，有一方案，割掉所有 S 与黑的边，比 INF 小

所以每对点中连向源汇点边权最小的边被割断，整体来看，就是对于任意一对端点，都选了一个较小权值，得到我们要的结果。
#### 代码
{% spoiler "代码" %}
```cpp
int n;
int a[25][25];
ISAP<int> isap;

inline void solve()
{
    int sum = 0;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            cin >> a[i][j];
            sum += a[i][j];
        }
    }
    int s = n*n+1, t = n*n+2;
    isap.init(t);
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            if ((i+j)&1) {
                isap.add_edge(s, i*n+j+1, a[i][j]);
                for (int k = 0, nx, ny; k < 4; ++k) {
                    nx = i+dir[k];
                    ny = j+dir[k+1];
                    if (nx < 0 || ny < 0 || nx >= n || ny >= n) continue;
                    isap.add_edge(i*n+j+1, nx*n+ny+1, INF);
                }
            } else {
                isap.add_edge(i*n+j+1, t, a[i][j]);
            }
        }
    }
    cout << sum-isap.work(s, t) << endl;
}

signed main()
{
    ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
    while (cin >> n) solve();
    return 0;
}
```
{%endspoiler%}

### [hdoj3657 Game](http://acm.hdu.edu.cn/showproblem.php?pid=3657)
#### 题意
与上题类似，不同的是

1. 相邻的可以选，但要付出一定代价
2. 有些点必选


#### 建图
同上

1. 相邻两点的边权由 INF 改为代价，要么割掉其中一个，要么割掉两点连边表示两个都选
2. 这个点到源点(汇点)的边权改为 INF ,以防被割掉


#### 代码
{% spoiler "代码" %}
```cpp
int n, m, k;
int a[N][N], c[N][N];
Dinic<int> dinic;

inline int mp(const int &x, const int &y) { return (x-1)*m+y; }

inline void solve()
{
    int sum = 0;
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            cin >> a[i][j];
            sum += a[i][j];
            c[i][j] = a[i][j];
        }
    }
    for (int i = 1, x, y; i <= k; ++i) {
        cin >> x >> y;
        c[x][y] = INF;
    }
    int s = n*m+1, t = n*m+2;
    dinic.init(n*m+2);
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            if ((i+j)&1) {
                dinic.add_edge(s, mp(i, j), c[i][j]);
                for (int k = 0, x, y; k < 4; ++k) {
                    x = i+dir[k];
                    y = j+dir[k+1];
                    if (x < 1 || y < 1 || x > n || y > m) continue;
                    dinic.add_edge(mp(i, j), mp(x, y), 2*(a[i][j]&a[x][y]));
                }
            } else {
                dinic.add_edge(mp(i, j), t, c[i][j]);
            }
        }
    }
    cout << sum-dinic.work(s, t) << endl;
}
```cpp

{% endspoiler %}

## 最大权闭合图

[定义参考博客](https://www.cnblogs.com/dilthey/p/7565206.html)
[证明参考博客](https://blog.csdn.net/ii0789789789/article/details/81532319)

S 连正点，权值为点值

正连负，权值为INF

负点连 T，权值为点值绝对值

答案为所有正点权值和-最小割

### [loj6001	「网络流 24 题」太空飞行计划](https://loj.ac/problem/6001)

#### 分析
用Dninc求最大流的好处是便于我们输出，因为层数如果为0，那么显然该边容量大于0，这就说明这条边并没有流量流过，那么显然与它相连的实验或者器材没有被使用（因为使用过的话，边的容量会变为0）
#### 代码
{% spoiler "代码" %}
```cpp
int n, m;
string str;
stringstream ss;
Dinic<int> dinic;

inline void solve()
{
    getline(cin, str);
    ss.clear();
    ss << str;
    ss >> n >> m;
    int s = 0, t = n+m+1;
    dinic.init(t);
    int sum = 0;
    for (int i = 1, j, w; i <= n; ++i) {
        getline(cin, str);
        ss.clear();
        ss << str;
        ss >> w;
        sum += w;
        dinic.add_edge(s, i, w);
        while (ss >> j)
            dinic.add_edge(i, j+n, INF);
    }
    for (int i = 1, w; i <= m; ++i) {
        cin >> w;
        dinic.add_edge(i+n, t, w);
    }
    int res = sum-dinic.work(s, t);
    vector<int> choose;
    for (int i = 1; i <= n; ++i)
        if (dinic.dep[i]) choose.emplace_back(i);
    for (int i = 0, sz = choose.size(); i < sz; ++i)
        cout << choose[i] << " \n"[i==sz-1];
    choose.clear();
    for (int i = 1; i <= m; ++i)
        if (dinic.dep[i+n]) choose.emplace_back(i);
    for (int i = 0, sz = choose.size(); i < sz; ++i)
        cout << choose[i] << " \n"[i==sz-1];
    cout << res << endl;
}
```
{% endspoiler%}

## 有向无环图最小不相交路径覆盖
[参考博客](https://www.cnblogs.com/justPassBy/p/5369930.html)
### 定义
在一个有向图中，找出最少的路径，使得这些路径经过了所有的点。

最小不相交路径覆盖：每一条路径经过的顶点各不相同。

### 算法
把原图的每个点V拆成Vx和Vy两个点，如果有一条有向边A->B，那么就加边Ax−>By。这样就得到了一个二分图。那么最小路径覆盖=原图的结点数-新图的最大匹配数。

### 证明
一开始每个点都是独立的为一条路径，总共有n条不相交路径。我们每次在二分图里找一条匹配边就相当于把两条路径合成了一条路径，也就相当于路径数减少了1。所以找到了几条匹配边，路径数就减少了多少。所以有最小路径覆盖=原图的结点数-新图的最大匹配数。

### [loj6002「网络流 24 题」最小路径覆盖](https://loj.ac/problem/6002)

#### 分析
二分图匹配求方案

如果两个点被匹配了，那么这两个点之间的边就有流量，则剩余流量为0
#### 代码
{% spoiler "代码" %}
```cpp
int n, m;
int du[N], to[N];
ISAP<int> isap;

inline void solve()
{
    cin >> n >> m;
    // pt i ==> i*2-1 - i*2
    int s = 0, t = n*2+1;
    isap.init(t);
    for (int i = 1; i <= n; ++i) {
        isap.add_edge(s, i*2-1, 1);
        isap.add_edge(i*2, t, 1);
    }
    for (int i = 1, u, v; i <= m; ++i) {
        cin >> u >> v;
        isap.add_edge(u*2-1, v*2, 1);
    }
    int res = n-isap.work(s, t);
    for (int u = 1; u <= n*2; u += 2) {
        for (int i = isap.fir[u], v; i != -1; i = isap.e[i].nex) {
            v = isap.e[i].v;
            if (v == s || isap.e[i].w) continue;
            to[u/2+1] = v/2;
            ++du[v/2];
            break;
        }
    }
    for (int i = 1; i <= n; ++i) {
        if (du[i]) continue;
        for (int u = i; u; u = to[u])
            cout << u << " \n"[to[u]==0];
    }
    cout << res << endl;
}
```

{% endspoiler %}

### [loj6003「网络流 24 题」魔术球](https://loj.ac/problem/6003)

## 有向无环图最小可相交路径覆盖

### 定义
在一个有向图中，找出最少的路径，使得这些路径经过了所有的点。

最小可相交路径覆盖：每一条路径经过的顶点可以相同。

## 切糕模型

### [codeforces434 D. Nanami's Power Plant](https://codeforces.com/problemset/problem/434/D)
详情见文末郑学长pdf,有图有真相(逃
#### 题意
给N个二次方程f(x)=ax^2+bx+c,每个方程的x取值范围为[l, r],并给出M个限制条件(u, v, d),表示要求满足xu <= xv+d,问你所有函数值之和的最大值
#### 思路
<img src="https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/network_flows/cf434D.png" data-action="zoom">

最小割,建图略了.(注:大致思路如此，我的代码略有不同)

如果割了f1(l1+2),我们发现如果割f2(l2)的话S,T还是联通,所以(贪心)没必要割,只能割后面的，因此做到了限制条件

但周知最小割是最小值,那么求最大值就把边权改为负,如果是负数求出来最大流量可能是0(逃)所以边权再加上一个很大的值使之为正，最后减去即可
#### 代码
{% spoiler "代码" %}
```cpp
const int LIM = 1e6;

struct F {
    int a, b, c, l, r;
    int calc(const int &x) const {
        return LIM-(a*x*x+b*x+c);
    }
};

int n, m;
int sum[55];
F f[55];
ISAP<long long> isap;

inline int id(const int &i, const int &j) { return sum[i-1]+j-f[i].l; }

signed main()
{
    ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
    cin >> n >> m;
    for (int i = 1; i <= n; ++i) {
        cin >> f[i].a >> f[i].b >> f[i].c;
    }
    for (int i = 1; i <= n; ++i) {
        cin >> f[i].l >> f[i].r;
        sum[i] = sum[i-1]+(f[i].r-f[i].l+2);
    }
    int s = sum[n], t = sum[n]+1;
    isap.init(t+1); // start from 0
    for(int i = 1; i <= n; ++i) {
        isap.add_edge(s, id(i, f[i].l), INF);
        for (int j = f[i].l; j <= f[i].r; ++j) {
            isap.add_edge(id(i, j), id(i, j+1), f[i].calc(j));
        }
        isap.add_edge(id(i, f[i].r+1), t, INF);
    }
    // xu <= xv+d
    for (int i = 1, u, v, d; i <= m; ++i) {
        cin >> u >> v >> d;
        for (int j = f[u].l; j <= f[u].r; ++j) {
            if (j-d >= f[v].l && j-d <= f[v].r+1) {
                isap.add_edge(id(u, j), id(v, j-d), INF);
            }
        }
    }
    cout << (LIM*n-isap.work(s, t)) << endl;
    return 0;
}
```

{% endspoiler %}

## 二分

最大流：新加进一条边不会使最大流变小；

费用流：费用流中费用是单调的；

## 优化建图
### [POJ1149 PIGS](http://poj.org/problem?id=1149)
#### 题意
有N个顾客，M个猪圈，每个猪圈有若干头猪，在开始的时候猪圈都是关闭的，每个顾客有一些猪圈的钥匙，每个顾客可以买最多hi头猪；

顾客依次来买猪，当一个顾客打开一些猪圈并且买完猪之后，你可以调整这些开着门的猪圈中猪的数量，然后再关门，等下一个顾客；求最多能卖多少头猪；

n <= 100, m <= 1000
#### 思路
见文末郑学长pdf

## 未完待续

# 一些资料

[郑学长的讲座](https://github.com/KaizynX/cdn/tree/master/file/network_flows) 收获很多

[luogu网络流24题](https://www.luogu.com.cn/problem/list?tag=332)

[胡伯涛《最小割模型在信息学竞赛中的应用》](https://github.com/enkerewpo/OI-Public-Library/blob/master/IOI%E4%B8%AD%E5%9B%BD%E5%9B%BD%E5%AE%B6%E5%80%99%E9%80%89%E9%98%9F%E8%AE%BA%E6%96%871999-2019/2007/day2/7.%E8%83%A1%E4%BC%AF%E6%B6%9B%E3%80%8A%E6%9C%80%E5%B0%8F%E5%89%B2%E6%A8%A1%E5%9E%8B%E5%9C%A8%E4%BF%A1%E6%81%AF%E5%AD%A6%E7%AB%9E%E8%B5%9B%E4%B8%AD%E7%9A%84%E5%BA%94%E7%94%A8%E3%80%8B.pdf)

[LibreOJ网络流24题(不全)](https://loj.ac/problems/tag/30)

[codeforces网络流标签](https://codeforces.com/problemset?tags=flows)