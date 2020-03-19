---
title: CF1245C. Constanze's Machine
date: 2019-11-05 18:45:00
categories:
  - Codeforces
tags:
  - 数学
  - 斐波那契
---

## [题面](https://codeforces.com/contest/1245/problem/C)

## 思路

思路应该很简单,找到连续的`u`(或`n`),求出这么多个`u`(`n`)能有几种变换

把所有连续段的种数乘起来,显然嘛

那么怎么求有几种变换

通过手动计算前几个发现是斐波那契(逃

**大胆猜测,胡乱验证**

设`dp[n]`表示长度为`n`的序列的变换种数

假如在一个连续的加上一个

1.这个不和任意一个合并 `dp[n-1]`

2.这个和前一个合并`dp[n-2]`

所以`dp[n] = dp[n-1] + dp[n-2]`

## 代码
```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1e5+7;
const int MOD = 1e9+7;

template <int _MOD> struct Mint
{
    int v = 0;
    Mint() {}
    Mint(int _v) : v((_v%_MOD+_MOD)%_MOD) {}
    Mint(long long _v) : v(static_cast<int>((_v%_MOD+_MOD)%_MOD)) {}
    Mint operator = (const int _v) { this->v = _v; return *this; }
    Mint operator = (const long long _v) { this->v = static_cast<int>(_v%_MOD); return *this; }
    bool operator < (const Mint &b) const { return v < b.v; }
    bool operator > (const Mint &b) const { return v > b.v; }
    bool operator == (const Mint &b) const { return v == b.v; }
    bool operator != (const Mint &b) const { return v != b.v; }
    bool operator <= (const Mint &b) const { return v < b.v || v == b.v; }
    bool operator >= (const Mint &b) const { return v > b.v || v == b.v; }
    Mint operator + (const Mint &b) { return Mint(v+b.v); }
    Mint operator - (const Mint &b) { return Mint(v-b.v); }
    Mint operator * (const Mint &b) { return Mint(1ll*v*b.v); }
    Mint operator / (const Mint &b) { return Mint(b.inv()*v); }
    Mint operator += (const Mint &b) { return *this = *this+b; }
    Mint operator -= (const Mint &b) { return *this = *this-b; }
    Mint operator *= (const Mint &b) { return *this = *this*b; }
    Mint operator /= (const Mint &b) { return *this = *this/b; }
    Mint pow(int p) const {
        Mint res(1), x(*this);
        while (p) {
            if (p&1) res = res*x;
            x *= x;
            p >>= 1;
        }
        return res;
    }
    Mint inv() const { return pow(_MOD-2); }
    friend istream& operator >> (istream &is, Mint &mt) { return is >> mt.v; }
    friend ostream& operator << (ostream &os, const Mint &mt) { return os << mt.v; }
};
using mint = Mint<MOD>;

string str;
mint f[N];
mint res = 1;

int main()
{
    cin >> str;
    f[0] = 1; f[1] = 1; f[2] = 2; f[3] = 3;
    for (int i = 2; i < N; ++i) {
        f[i] = f[i-1]+f[i-2];
    }
    for (int i = 0; i < (int)str.length(); ++i) {
        if (str[i] == 'm' || str[i] == 'w') {
            res = 0;
            break;
        }
        if (str[i] != 'n' && str[i] != 'u') continue;
        char c = str[i];
        int cnt = 1;
        while (i+1 < (int)str.length() && str[i+1] == c) {
            ++i;
            ++cnt;
        }
        res *= f[cnt];
    }
    cout << res << endl;
    return 0;
}
```