---
title: 子序列宽度之和
date: 2018-09-29 11:30:00
categories:
  - LeetCode
tags:
  - 数学

mathjax: true
---
# [LeetCode 891 子序列宽度之和](https://leetcode-cn.com/problems/sum-of-subsequence-widths/description/)

## 题面

给定一个整数数组 A ，考虑 A 的所有非空子序列。

对于任意序列 S ，设 S 的宽度是 S 的最大元素和最小元素的差。

返回 A 的所有子序列的宽度之和。

由于答案可能非常大，请返回答案模 10^9+7。

## 示例

### 输入

    [2,1,3]

### 输出

    6

### 解释

子序列为 [1]，[2]，[3]，[2,1]，[2,3]，[1,3]，[2,1,3]

相应的宽度是 0，0，0，1，1，2，2

这些宽度之和是 6

## 数据范围

1 <= A.length <= 20000

1 <= A[i] <= 20000

## 思路

### $O(n^2)$算法

显然暴力枚举是不可能的,这辈子都不可能的

首先从样例可以看出,所谓的子序列是自由组合的

所以先排序一下有助于我们思考

决定一个子序列宽度的是最大值和最小值

假如我们从这个,现在排序过的序列中任意找出几个

那么起决定作用的一定是首尾两个

所以我们的取法是先取首尾 $a_l, a_r$

再从首尾间的数字中选任意个加入序列

这些序列的宽度均为 $a_r - a_l$

那么我们要取这对的首尾能构成几个序列

显然首尾间还有 $r-l-1$ 个元素

那么就是排列组合问题了

设 $k = r-l-1$

结果就是 $C_k^0+C_k^1+C_k^2...+C_k^k$

$\sum\limits_{i=0}^{k} C_k^i = 2^k$

所以这样的序列宽度和就是 $2^k\times (a_r-a_l)$

那么能够想到的就是枚举首尾求结果了

而求 $2^k$ 复杂度是 $O(k)$

用快速幂也是 $O(\log k)$

直接算可能会 $O(n^2 \log n)$

为何不开个数组 $O(n)$

最后复杂度是 $O(n^2)$

还是妥妥地 TLE

虽然抱着侥幸的心理

不过 $10^4$ 这样的数据范围真不三不四的样子

### $O(n^2)$代码
```cpp
class Solution {
public:
    const int MOD = 1e9+7;
    long long pow2[20005];
    int sumSubseqWidths(vector<int>& A) {
        sort(A.begin(), A.end());
        // init
        pow2[0] = 1;
        for(int i = 1; i <= A.size(); ++i)
            if((pow2[i] = pow2[i-1]<<1) > MOD)
                pow2[i] %= MOD;
        // init
        long long res = 0;
        for(int i = 0; i < A.size()-1; ++i)
            for(int j = i+1; j < A.size(); ++j)
                res = (res+pow2[j-i-1]*(A[j]-A[i]))%MOD;
        return res;
    }
};
```
---

### $O(n)$

直觉说肯定能优化到 $O(n)$

因为 $a_i$ 是不确定的

但是 $2^k$ 是确定的

于是我就看看乘以 $2^k$ 的到底是那些

假设 $a$ 有 $n$ 项

- $k = 0$

    $(a_n-a_{n-1})+(a_{n-1}-a_{n-2})+...+(a_3-a_2)+(a_2-a_1)$

- $k = 1$

    $(a_n-a_{n-2})+(a_{n-1}-a_{n-3})+...+(a_4-a_2)+(a_3-a_1)$

- $k = 2$

    $(a_n-a_{n-5})+(a_{n-1}-a_{n-4})+...+(a_5-a_2)+(a_4-a_1)$

......

所以是 $2^k\times \sum\limits_{i=1+(k+1)}^{n}{a_i} - \sum\limits_{i=1}^{n-(k+1)}{a_i}$

显然可以用前缀和来解决

最后结果就是

$\sum\limits_{k=1}^{n-2}(2^k\times \sum\limits_{i=k+2}^{n}{a_i} - \sum\limits_{i=1}^{n-k-1}{a_i})$

空间复杂度 $O(n)$

### $O(n)$代码

```cpp
class Solution {
public:
    const int MOD = 1e9+7;
    long long s[20007];
    int sumSubseqWidths(vector<int>& A) {
        sort(A.begin(), A.end());
        int n = A.size();
        long long pow2 = 1, res = 0;
        for(int i = 1; i <= n; ++i)
            s[i] = s[i-1]+A[i-1];
        for(int i = 0; i <= n-2; ++i)
        {
            res = (res+(s[n]-s[i+1]-s[n-i-1])%MOD*pow2)%MOD;
            pow2 = (pow2<<1)%MOD;
        }
        return res;
    }
};
```
---
### 官方算法

这是官方的答案

大致是 $\sum a_r-\sum a_l$

定下来的是 $a_i$, 找它应该是加上或减去 2 的几次方

[官方题解链接](https://leetcode-cn.com/problems/sum-of-subsequence-widths/solution/)

最后结果是

$\sum\limits_{i=0}^{n-1}(2^i-2^{N-i-1})\times a_i$

- 如果牺牲空间换时间​,预处理 $2^k$

    时间复杂度 $O(n)$

    空间复杂度 $O(n)$

- 如果牺牲时间换空间​,快速幂求 $2^k$

    时间复杂度 $O(n\log n)$

    空间复杂度 $O(1)$

### 官方没有C++代码

```java
// java
class Solution {
    public int sumSubseqWidths(int[] A) {
        int MOD = 1_000_000_007;
        int N = A.length;
        Arrays.sort(A);

        long[] pow2 = new long[N];
        pow2[0] = 1;
        for (int i = 1; i < N; ++i)
            pow2[i] = pow2[i-1] * 2 % MOD;

        long ans = 0;
        for (int i = 0; i < N; ++i)
            ans = (ans + (pow2[i] - pow2[N-1-i]) * A[i]) % MOD;

        return (int) ans;
    }
}
```