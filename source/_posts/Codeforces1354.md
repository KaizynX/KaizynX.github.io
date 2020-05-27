---
title: Educational Codeforces Round 87 (Rated for Div. 2)
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2020-05-17 20:30:00
mathjax: true
categories:
  - Codeforces
tags:
description: CF1354题解(ABCDEF)
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/Codeforces1354/cover.jpg 
---
# 写在前头

应该是鄙人CF史上最好的一次成绩(如果没有fst逃

[链接](https://codeforces.com/contest/1354)

# A - Alarm Clock

简单模拟?

特判一下两个特殊情况

1. 一开始就睡爽了,不用补交

2. $c \leq d$ 怎么补都肾虚


其他算没补一次 $c-d$ 共要补 $\lceil\frac{a-b}{c-d}\rceil$ (向上取整

{% spoiler "代码" %}
```cpp
#include <bits/stdc++.h>

// #define DEBUG

using namespace std;

const double eps = 1e-7;
const double PI = acos(-1);
typedef pair<int, int> pii;
const int MOD = 998244353;
const int INF = 0x3f3f3f3f;

int a, b, c, d;

inline void solve() {
  cin >> a >> b >> c >> d;
  if (b >= a) return void(cout << b << endl);
  if (c <= d) return void(cout << -1 << endl);
  int e = c-d;
  long long res = (a-b+e-1ll)/e*c+b;
  cout << res << endl;
}

signed main() {
  ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
  int T = 1;
  cin >> T;
  for (int t = 1; t <= T; ++t) {
    solve();
  }
  return 0;
}
```

{% endspoiler %}

# B - Ternary String

求最短的区间包含 1, 2, 3

设右边界为 $r$

设 $p[i]$ 是数字 $i$ 出现的最右边位置

则此时的左边界 $l=min({p[1], p[2], p[3]})$

emmm 不多说了

{% spoiler "代码" %}
```cpp
#include <bits/stdc++.h>

// #define DEBUG

using namespace std;

const double eps = 1e-7;
const double PI = acos(-1);
typedef pair<int, int> pii;
const int MOD = 998244353;
const int INF = 0x3f3f3f3f;
const int N = 2e5+7;

string s;
int p[5];

inline void solve() {
  cin >> s;
  memset(p, -1, sizeof p);
  int n = s.size();
  int res = 0;
  for (int i = 0; i < n; ++i) {
    p[s[i]-'0'] = i;
    if (p[1] == -1 || p[2] == -1 || p[3] == -1) continue;
    int cur = i-min({p[1], p[2], p[3]})+1;
    res = res ? min(res, cur) : cur;
  }
  cout << res << endl;
}

signed main() {
  ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
  int T = 1;
  cin >> T;
  for (int t = 1; t <= T; ++t) {
    solve();
  }
  return 0;
}
```

{% endspoiler %}

# C - (Not So) Simple Polygon Embedding

真就是纯数学几何题

当 $n$ 为偶数

<img src="https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/Codeforces1354/C1.jpg" data-action="zoom">

由几何对称美可知(呸)如图多边形就这卡在正方形里面

多边形总有四条边刚好拄在正方形边的中间

$\theta = \frac{\pi}{n}$

$h = \frac{0.5}{\tan\frac{\theta}{2}}$

正方形边长 $=2\times h$

*其实我代码里取的角度不是图上的那个角度,不过这么简单问题不大*

当 $n$ 为奇数

根据哦我的上帝啊几何的对称美(明明是瞎转了好久才看出来的)可知

<img src="https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/Codeforces1354/C2.jpg" data-action="zoom">

多边形按照如图所示的那个对称轴放置

正方形对角线 $2\times h$

正方形边长 $\frac{2\times h}{\sqrt{2}}$

$\beta = 45°$

$l = \frac{0.5}{\sin\frac{\theta}{2}}$

显然 $h$ 的长度和 $l$ 有关

首先知道了这个多边形,我们的正方形怎么得到

我们要用 $a$ 这条边(这条边的斜率是确定的), 去**切** $l$ 边的顶点

所以枚举所有顶点,计算不同 $\theta,l$ 情况下形成不同切边 $a$ 围成正方形 $h$ 的最大值

求最大值是因为要保证所有顶点都在正方形内

又由于哦这该死的对称美我们只需要枚举右上角(第一象限)的顶点即可

怎么求,纯数学几何

$\begin{cases}
2 \cos\theta \cdot h\cdot l = h^2+l^2-a^2 \\\\
2 \cos\beta \cdot h\cdot a = h^2+a^2-l^2 \\\\
\end{cases}$

两式相减得 $h\cdot(\cos \theta \cdot l-\cos \beta \cdot a) = l^2-a^2$

两式相加得 $h\cdot(\cos \theta \cdot l+\cos \beta \cdot a) = h^2$

由相加式得 $h = \cos \theta \cdot l+\cos \beta \cdot a$

代入相减式得 $(\cos \theta \cdot l)^2+(\cos \beta \cdot a)^2=l^2-a^2$

解得 $a = \sqrt{\frac{(1-\cos^2\theta)}{(1-\cos^2\beta)}} \cdot l$

此时就可以解得 $h$ 了

不过注意这里的 $\theta$

第 $i$ 个顶点的 $\theta=\frac{i\cdot\pi}{n}$


{% spoiler "代码" %}
```cpp
#include <bits/stdc++.h>

// #define DEBUG

using namespace std;

const double eps = 1e-7;
const double PI = acos(-1);
typedef pair<int, int> pii;
const int MOD = 998244353;
const int INF = 0x3f3f3f3f;
const int N = 4e2+7;

int n;

inline void solve() {
  double res = 0;
  // cin >> n;
  scanf("%d", &n);
  if (n%2 == 0) {
    res = tan((n-1)*PI/n/2);
  } else {
    double h, theta, l, a, cost;
    l = 0.5/sin(PI/n/2);
    for (int i = 1; i <= n/2; ++i) {
      theta = PI/n*i;
      cost = cos(theta);
      a = sqrt(2*(1-cost*cost))*l;
      h = cost*l+a/sqrt(2);
      res = max(res, h);
    }
    res = 2*res/sqrt(2);
  }
  printf("%.9f\n", res);
}

signed main() {
  // ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
  int T = 1;
  // cin >> T;
  scanf("%d", &T);
  for (int t = 1; t <= T; ++t) {
    solve();
  }
  return 0;
}
```

{% endspoiler %}

# D - Multiset

看题第一感觉平板电视红黑树?(可惜我不会也不知道能不能

此题有内存限制(意义不明

我采用树状数组+二分做法,时间复杂度 $O(n\log^2n)$ (坚信伟大的cf神秘力量)

使用 cin 1341 ms, 吓得我改了 scanf 748 ms,加上手抖罚时 +2

具体就是类似桶排?记录每个值的数字个数

查询的时候二分,某个值之前的数字个数就是排名

应该不难理解?

{% spoiler "代码" %}
```cpp
#include <bits/stdc++.h>

// #define DEBUG

using namespace std;

const double eps = 1e-7;
const double PI = acos(-1);
typedef pair<int, int> pii;
const int MOD = 998244353;
const int INF = 0x3f3f3f3f;
const int N = 1e6+7;

template <typename T>
struct BinaryIndexedTree
{
    int n;
    T tr[N<<1];
    BinaryIndexedTree() { memset(tr, 0, sizeof tr); }
    void init(const int &_n) { n = _n; clear(); }
    void clear() { for (int i = 1; i <= n; ++i) tr[i] = 0; }
    void add(const int &x, const T &v) { for (int i = x ; i <= n; i += i&-i) tr[i] += v; }
    void add(const int &x, const int &y, const T &v) { add(x, v); add(y+1, -v); }
    T query(const int &x) { T res = 0; for (int i = x ; i; i -= i&-i) res += tr[i]; return res; }
    T query(const int &x, const int &y) { return query(y)-query(x-1); }
};

int n, q;
BinaryIndexedTree<int> bit;
int t[N];

inline void solve() {
  // cin >> n >> q;
  scanf("%d %d", &n, &q);
  bit.init(n);
  for (int i = 1, a; i <= n; ++i) {
    // cin >> a;
    scanf("%d", &a);
    bit.add(a, 1);
  }
  for (int i = 1, k, l, r, mid; i <= q; ++i) {
    // cin >> k;
    scanf("%d", &k);
    if (k > 0) {
      bit.add(k, 1);
    } else {
      k = -k;
      l = 1; r = n;
      while (l < r) {
        mid = (l+r)>>1;
        if (bit.query(mid) >= k) r = mid;
        else l = mid+1;
      }
      bit.add(l, -1);
    }
  }
  // if (bit.query(n) == 0) return void(cout << 0 << endl);
  if (bit.query(n) == 0) return void(puts("0"));
  for (int i = 1; i <= n; ++i) {
    if (bit.query(i) > 0) {
      printf("%d\n", i);
      return;
    }
  }
}

signed main() {
  // ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
  int T = 1;
  // cin >> T;
  for (int t = 1; t <= T; ++t) {
    solve();
  }
  return 0;
}
```

{% endspoiler %}

# E - Graph Coloring

能与 1 相邻的有 2

能与 2 相邻的有 1, 3

能与 3 相邻的有 2

不难发现, 1, 3 可以看成一种颜色

于是问题转化为对图**奇偶染色**(黑白染色)

我们先黑白染色,然后有两种情况 白的赋 2 或者黑的赋 2, 要满足赋 2 的点数为 $n2$

(为什么考虑 2 ? 因为 2 特殊啊, 当然把 1,3 看成一个整体也可以

但是又有一个严重的问题,这个图不一定连通,每个子图都要考虑是黑还是白赋 2

我们用背包来解决这个问题

设 $dp[i][j]$ 是给前 $i$ 个子图黑或白赋 2,是否恰好有 $j$ 个点被赋值 2

然后我们可以 $O(n^2)$ 转移出来,也可以根据 $dp$ 数组倒推回去得到具体方案

(这什么操作啊,我叫不出具体名词)

总之这题大致思路到此应该清晰了,但是细节还是很多很繁琐,详情见代码(溜

另,不满足的情况就是: 无法黑白染色 或 无法恰好有 $n2$ 个点赋 2

{% spoiler "代码" %}
```cpp
#include <bits/stdc++.h>

// #define DEBUG

using namespace std;

const double eps = 1e-7;
const double PI = acos(-1);
typedef pair<int, int> pii;
const int MOD = 998244353;
const int INF = 0x3f3f3f3f;
const int N = 5e3+7;

struct DSU
{
    int fa[N];
    void init(int sz) { for (int i = 0; i <= sz; ++i) fa[i] = i; }
    int get(int s) { return s == fa[s] ? s : fa[s] = get(fa[s]); }
    int& operator [] (int i) { return fa[get(i)]; }
    bool merge(int x, int y) {
        int fx = get(x), fy = get(y);
        if (fx == fy) return false;
        fa[fx] = fy; return true;
    }
};

int n, m, t;
int n1, n2, n3;
DSU dsu;
int col[N], cnt[N][2];
int dp[N][N];
int mp[N], pt[N], paint[N];
int res[N];
vector<int> e[N];
queue<int> q;
vector<int> odd;

void bfs(const int &s) {
  q.push(s);
  col[s] = 0;
  while (q.size()) {
    int u = q.front();
    q.pop();
    for (const int &v : e[u]) {
      if (col[v] == -1) {
        col[v] = col[u]^1;
        q.push(v);
        dsu.merge(u, v);
      } else {
        if (col[u] == col[v]) {
          cout << "NO" << endl;
          exit(0);
        }
      }
    }
  }
}

inline void solve() {
  memset(col, -1, sizeof col);
  cin >> n >> m;
  cin >> n1 >> n2 >> n3;
  for (int i = 1, u, v; i <= m; ++i) {
    cin >> u >> v;
    e[u].emplace_back(v);
    e[v].emplace_back(u);
  }
  dsu.init(n);
  for (int i = 1; i <= n; ++i) {
    if (col[i] == -1) bfs(i);
  }
  for (int i = 1; i <= n; ++i) {
    ++cnt[dsu[i]][col[i]];
    if (dsu[i] == i) {
      pt[++t] = i;
      mp[i] = t;
    }
  }
  dp[0][0] = 1;
  for (int i = 1; i <= t; ++i) {
    for (int j = cnt[pt[i]][0]; j <= n; ++j) {
      dp[i][j] |= dp[i-1][j-cnt[pt[i]][0]];
    }
    for (int j = cnt[pt[i]][1]; j <= n; ++j) {
      dp[i][j] |= dp[i-1][j-cnt[pt[i]][1]];
    }
  }
  if (!dp[t][n2]) return void(cout << "NO" << endl);
  for (int i = t, rest = n2; i; --i) {
    if (rest-cnt[pt[i]][0] >= 0 && dp[i-1][rest-cnt[pt[i]][0]]) {
      rest -= cnt[pt[i]][0];
      paint[pt[i]] = 0;
    } else {
      rest -= cnt[pt[i]][1];
      paint[pt[i]] = 1;
    }
  }
  for (int i = 1; i <= n; ++i) {
    if (col[i] == paint[dsu[i]]) res[i] = 2;
    else odd.emplace_back(i);
  }
  for (int i = 0; i < (int)odd.size(); ++i) {
    res[odd[i]] = (i < n1 ? 1 : 3);
  }
  cout << "YES" << endl;
  for (int i = 1; i <= n; ++i) cout << res[i];
  cout << endl;
}

signed main() {
  ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
  int T = 1;
  // cin >> T;
  for (int t = 1; t <= T; ++t) {
    solve();
  }
  return 0;
}
```

{% endspoiler %}

# F - Summoning Minions

首先要观察思考手玩得出:

假设我们选好了前 $k-1$ 个

因为 $b \geq 0$ 所以对于剩下的全都选一下再删掉(最后一个不删)

为了使结果最优

前 $k-1$ 个要按 $b$ 的升序选

最后留下的(没有删掉的)应该是除掉前 $k-1$ 剩下的当中 $a$ 最大的

想到这里,那么我们要枚举前 $k-1$ 个分别是多少吗?

不,枚举最后一个是哪个就好了

然后我们用动态规划找前 $k-1$ 个的选法

设 $dp[i][j]$ 是在前 $i$ 个中选了 $j$ 个的最大“价值”

我们还需要定义一个所谓的**"价值"**来衡量一个选法的优秀与否

不妨考虑最后的值 (这里的 $(a_i,b_i)$ 表示第 $i$ 个被选的属性)

$=\sum\limits_{i=1}^{k}{a_i+(i-1)\cdot b_i}+\sum\limits_{没选的}{k\cdot b}$ 

$=\sum\limits_{全部}{k\cdot b}+\sum\limits_{i=1}^{k}{a_i-(k-i)\cdot b_i}$

因此我们可以把 $\sum\limits_{i=1}^{k}{a_i-(k-i)\cdot b_i}$ 作为衡量函数(即当作$dp$转移的时候的值)

同 E 题,我们可以倒推出到底选了哪几个,那么方案就出来了

时间复杂度 $O(T\cdot n^3)$

详情同见代码(溜

{% spoiler "代码" %}
```cpp
#include <bits/stdc++.h>

// #define DEBUG

using namespace std;

const double eps = 1e-7;
const double PI = acos(-1);
typedef pair<int, int> pii;
const int MOD = 998244353;
const int INF = 0x3f3f3f3f;
const int N = 80;

struct Node {
  int a, b, id;
  friend bool operator <(const Node &lhs, const Node &rhs) {
    return lhs.b < rhs.b;
  }
  friend istream& operator >>(istream &is, Node &nd) {
    return is >> nd.a >> nd.b;
  }
};

int n, k;
Node c[N], d[N];
int dp[N][N], val[N], vis[N];
vector<int> seq;
// dp[i][j] choose j's [1, i]
// val[i] the val choose i as last

inline int work() {
  memcpy(d+1, c+1, sizeof(Node)*n);
  sort(d+1, d+n);
  fill(dp[0], dp[0]+n*N, -INF);
  dp[0][0] = 0;
  for (int i = 1; i < n; ++i) {
    memcpy(dp[i], dp[i-1], sizeof(int)*k);
    for (int j = 1; j < k; ++j) {
      dp[i][j] = max(dp[i][j], dp[i-1][j-1]+d[i].a-d[i].b*(k-j));
    }
  }
  return dp[n-1][k-1]+d[n].a;
}

inline void solve() {
  cin >> n >> k;
  for (int i = 1; i <= n; ++i) cin >> c[i], c[i].id = i;

  int last = 1;
  for (int i = 1; i <= n; ++i) {
    // choose c[i] as last
    swap(c[i], c[n]);
    val[i] = work();
    if (val[i] > val[last]) last = i;
    swap(c[i], c[n]);
  }

  swap(c[last], c[n]);
  work();
  vector<int>().swap(seq);
  memset(vis+1, 0, sizeof(int)*n);
  vis[d[n].id] = 1;
  for (int i = n-1, j = k-1; i && j; --i) {
    if (dp[i][j] == dp[i-1][j-1]+d[i].a-d[i].b*(k-j)) {
      vis[d[i].id] = 1;
      seq.emplace_back(d[i].id);
      --j;
    }
  }
  reverse(seq.begin(), seq.end());
  cout << 2*n-k << endl;
  for (auto &i : seq) cout << i << " ";
  for (int i = 1; i <= n; ++i) if (!vis[i]) {
    cout << i << " " << -i << " ";
  }
  cout << d[n].id << endl;
#ifdef DEBUG
  for (int i = 1; i <= n; ++i) val[last] += c[i].b*(k-1);
  cout << "val = " << val[last] << endl;
#endif
}

signed main() {
  ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
  int T = 1;
  cin >> T;
  for (int t = 1; t <= T; ++t) {
    solve();
  }
  return 0;
}
```

{% endspoiler %}