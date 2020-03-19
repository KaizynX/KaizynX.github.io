---
title: CF1256F. Equalizing Two Strings
date: 2019-11-05 19:40:00
categories:
  - Codeforces
tags:
  - 排序
  - 数学
  - 逆序对

mathjax: true
---
## [题面](https://codeforces.com/contest/1256/problem/F)

## 思路

进行长度为 2 的操作相当于交换相邻两个字符

首先,如果一个字符串有两个字符相同,先用两两交换使之相邻

那么这一字符串一直交换这两个相同的字符,就相当于不变

另一个字符串进行两两交换,则能变成任意排列,所以有限次操作总能相同

排除以上特殊情况后,剩下的最长长度为 26 (暴力???)

引理1:假设存在方法使两个字符串相同了,再进行无限次操作,只要两者选的区间相同,都相同

引理2:假设字符串 S1 能通过 n 次两两交换变成 字符串 S2,若 n 是 2 的倍数, 则满足题目要求

证明:对 S2 同一区间进行 2 的倍数次操作,不会改变

所以

那么我们不妨将两个都操作成升序序列,通过两两交换的方式,次数就是逆序对

如果两个字符串的次数差是 2 的倍数,就满足条件



## 代码
```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 2e5+7;

int q, n;
int cnt[2][26], tot[2];
string s[2];

int main()
{
    cin >> q;
    while (q--) {
        cin >> n;
        int flag = 0;
        memset(cnt, 0, sizeof cnt);
        for (int i = 0; i < 2; ++i) {
            cin >> s[i];
            for (int j = 0; j < n; ++j)
                ++cnt[i][s[i][j]-'a'];
        }
        flag = 1;
        for (int i = 0; i < 26; ++i) {
            if (cnt[0][i] != cnt[1][i]) {
                flag = 0;
                break;
            }
        }
        if (!flag) {
            cout << "NO" << endl;
            continue;
        }
        flag = 0;
        for (int i = 0; i < 26; ++i) {
            if (cnt[0][i] > 1 || cnt[1][i] > 1) {
                flag = 1;
                break;
            }
        }
        if (flag) {
            cout << "YES" << endl;
            continue;
        }

        memset(cnt, 0, sizeof cnt);
        tot[0] = tot[1] = 0;
        for (int i = 0; i < 2; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int k = s[i][j]-'a'+1; k < 26; ++k)
                    tot[i] += cnt[i][k];
                ++cnt[i][s[i][j]-'a'];
            }
        }
        cout << (tot[0]%2 == tot[1]%2 ? "YES" : "NO") << endl;
    }
    return 0;
}
```