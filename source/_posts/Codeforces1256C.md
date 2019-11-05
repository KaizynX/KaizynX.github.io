---
title: CF1256C. Platforms Jumping
date: 2019-11-05 19:20:00
categories:
  - Codeforces
tags:
  - 数学
  - 贪心

---
## [题面](https://codeforces.com/contest/1256/problem/C)

## 思路

易证:如果跳到某块任意长度木板,一定可以通过有限步数达到右端

不妨采取这样的贪心策略,每次跨河(没木板的地方)用最大的步数(d)

每次跳到木板的左端点,然后到右端点,再跳 d

做法如下

先设左右的木板并排在右端

如果跳一步到河里了,就把最左边的木板移到前面

懒得说了,上代码

## 代码
```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1e3+7;

int n, m, d;
int c[N], res[N];

int main()
{
    cin >> n >> m >> d;
    int sum = 0;
    for (int i = 1; i <= m; ++i) {
        cin >> c[i];
        sum += c[i];
    }
    int id = 1, p = d;
    for ( ; id <= m && p < n-sum+1; ++id) {
        for (int j = 0; j < c[id]; ++j) {
            res[p+j] = id;
        }
        sum -= c[id];
        p += c[id]-1+d;
    }
    if (p < n-sum+1) {
        cout << "NO" << endl;
        return 0;
    }
    for (int i = m, j = n; i >= id; --i) {
        for (int k = 0; k < c[i]; ++k) {
            res[j--] = i;
        }
    }
    cout << "YES" << endl;
    for (int i = 1; i <= n; ++i) {
        cout << res[i] << " \n"[i==n];
    }
    return 0;
}
```