---
title: ABC176F Brave CHAIN
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2020-08-23 18:00:00
mathjax: true
categories:
  - AtCoder
tags:
  - DP
description: 特殊转移的DP题
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/Atcoder-ABC176F/cover.jpg
---
# 题面

[链接](https://atcoder.jp/contests/abc176/tasks/abc176_f)

给 $3N$ 张牌,值依次是 $A_i$ ,执行 $N-1$ 次如下操作

可以将前五张任意排序,删掉最左边三张,如果删掉的三张值相同,获得 1 分

最后剩下三张,如果值相同,获得 1 分

数据范围:

- $1 \leq N \leq 2000$
- $1 \leq A_i \leq N$

# 思路

每次操作相关的卡牌只有上一轮剩下的两张,以及这一轮新加入的三张

考虑数据范围,应该是把上一轮的两张作为 dp 的状态,不难得出 $O(n^3)$ 简单 dp 吧(略

那么如果再稍加思考,就会发现状态转移并不需要对所有状态更新,开始分类讨论

设 $dp[i][j]$ 是上一轮剩下一张 $i$, 一张 $j$ 的最大得分

设新加入的三张分别为 $a[i], a[i+1], a[i+2]$

- $a[i] == a[i+1] == a[i+2]$

  这时候如果我们删除这三张,即可使所有状态的 dp 值加 1

  如果我们删掉其中两张或一张,再用上轮留下的相同值凑成1分删去,其实是等价的

  但如果我们不删除这三张凑成1分,而留下其中几张和后面的凑

  显然直接删这三张更优,是贪心的(稍加思考可得

  所以一旦出现这种情况,只考虑删掉这三张即可,那么全部状态加1就可以用额外的标记记录

- $a[i] == a[i+1] || a[i+1] == a[i+2]$

  此时只要前一轮中一张相同的值即可凑得 1 分

  不妨设 $a[i] == a[i+1]$ 那么有 $dp[a[i+2]][k]=dp[a[i]][k]+1$

- 新一轮的三个数保留其中两个,不得分
- 新一轮的三个数保留其中一个,不得分
- 新一轮的三个数全部删去,不得分,此时状态全不变

注意:状态转移时要用上一轮的状态值转移,为防止当前轮的影响,应先记录需要用到的 dp 值

# 代码

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
const int N = 2e3+7;

int n;
int a[N*3];
int dp[N][N], mv[N], curmv[N], curdp[N];

inline void update(int x, int y, int z) {
  if (x > y) swap(x, y);
  if (z <= dp[x][y]) return;
  dp[x][y] = z;
  mv[x] = max(mv[x], z);
  mv[y] = max(mv[y], z);
}

inline void solve() {
  cin >> n;
  for (int i = 1; i <= 3*n; ++i) cin >> a[i];
  memset(dp, 0x9f, sizeof dp);
  memset(mv, 0x9f, sizeof mv);
  update(a[1], a[2], 0);
  int base = 0;
  for (int i = 3; i+2 <= 3*n; i += 3) {
    if (a[i] == a[i+1] && a[i] == a[i+2]) {
      ++base;
      continue;
    }
    int b[3], maxv = 0;
    sort(a+i, a+i+3);
    if (a[i+1] == a[i+2]) swap(a[i], a[i+2]); // fuck there
    for (int j = 1; j <= n; ++j) {
      curdp[j] = dp[min(a[i], j)][max(a[i], j)];
      curmv[j] = mv[j];
      maxv = max(maxv, mv[j]);
    }
    for (int j = 0; j < 3; ++j) b[j] = dp[a[i+j]][a[i+j]];
    // spj
    if (a[i] == a[i+1]) for (int j = 1; j <= n; ++j) { // fuck here
      update(a[i+2], j, curdp[j]+1); // dp[a[i]][j] -> dp[a[i+2]][j]
    }
    update(a[i+1], a[i+2], b[0]+1); // get three a[i]
    update(a[i], a[i+2], b[1]+1); // get three a[i+1]
    update(a[i], a[i+1], b[2]+1); // get three a[i+2]
    // two of three rest
    update(a[i+1], a[i+2], maxv);
    update(a[i], a[i+2], maxv);
    update(a[i], a[i+1], maxv);
    // one of three rest
    for (int j = 1; j <= n; ++j) {
      update(a[i], j, curmv[j]);
      update(a[i+1], j, curmv[j]);
      update(a[i+2], j, curmv[j]);
    }
  }
  int res = dp[a[3*n]][a[3*n]]+1;
  for (int i = 1; i <= n; ++i) res = max(res, mv[i]);
  cout << res+base << endl;
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