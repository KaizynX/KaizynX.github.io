---
title: 检查「好数组」
date: 2019-11-10 19:00:00
categories:
  - LeetCode
tags:
  - 扩展欧几里得
  - 数学

mathjax: true
---

## [题面](https://leetcode-cn.com/contest/weekly-contest-161/problems/check-if-it-is-a-good-array/)

## 思路

由**扩展欧几里得**可得

$ax+by=c$

其中 $c$ 是 $\gcd(a,b)$ 的倍数

要想得到 $1$ ,则要 $a, b$ 互质

$a, b$ 可以是数组中任意两个,也可以是任意个通过**变换**得到

或者说我们找到 $k_{p_1}\times a_{p_1}+k_{p_2}\times a_{p_2}+.. = z_1$

$k_{q_1}\times a_{q_1}+k_{q_2}\times a_{q_2}+.. = z_q$

使得 $\gcd(z_1, z_2) == 1$ 即可

假设数组为 $a_1, a_2,...a_n$

其中 $a_i, a_j$ 能组成 $\gcd(a_i, a_j)$ 的倍数的数

再有 $a_k, a_l$ 能组成 $\gcd(a_k, a_l)$ 的倍数的数

其中 $i$ 可能等于 $j, k, l$ 等等

那么这四个数又可以组成 $\gcd(\gcd(a_i, a_j),\gcd(a_k,a_l))$ 倍数的数

所以所有的数能组成 $\gcd\limits_{1 \leq i \leq n} a_i$ 的倍数的数

因为不断做 $\gcd$ 只会使结果越来越小,也就是能组成的数的范围越来越大

最后判断所有数的最大公约数是否为 $1$ 即可




## 代码
```cpp
class Solution {
public:
    bool isGoodArray(vector<int>& nums) {
        int g = 0;
        for (int i : nums) g = __gcd(i, g);
        return g == 1;
    }
};
```