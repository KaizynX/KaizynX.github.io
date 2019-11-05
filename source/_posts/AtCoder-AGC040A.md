---
title: AGC040A - ><
date: 2019-11-05 19:50:00
categories:
  - AtCoder
tags:
  - 数学

mathjax: true
---
## [题面](https://atcoder.jp/contests/agc040/tasks/agc040_a)

## 思路

如果存在一个上升的序列

$a_i < a_{i+1} < ... < a_j$

不妨令他们为 $0, 1, 2 ...$

反过来下降序列也相同

但最终结果要同时满足上面两种方法,故取 $\max$

## 代码
```cpp
#include <bits/stdc++.h>
#define DEBUG

using namespace std;

const int N = 5e5+7;
const int INF = 1e9;

string str;
int a[N];

int main()
{
    cin >> str;
    int n = str.length()+1;
    long long sum = 0;
    for (int i = 0, cnt = 0; i < n; ++i) {
        a[i] = max(a[i], cnt);
        if (i < (int)str.length() && str[i] == '<') {
            ++cnt;
        } else {
            cnt = 0;
        }
    }
    for (int i = str.length()-1, cnt = 0; i >= -1; --i) {
        a[i+1] = max(a[i+1], cnt);
        if (i >= 0 && str[i] == '>') {
            ++cnt;
        } else {
            cnt = 0;
        }
    }
    for (int i = 0; i < n; ++i) {
        sum += a[i];
    }
    cout << sum << endl;
    return 0;
}
```