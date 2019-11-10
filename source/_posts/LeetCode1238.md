---
title: 循环码排列
date: 2019-10-29 13:30:00
categories:
  - LeetCode
tags:
  - 构造

mathjax: true
---
一道有趣得构造题
<!--more-->
[传送](https://leetcode-cn.com/problems/circular-permutation-in-binary-representation/)

## 题面

给你两个整数 n 和 start。你的任务是返回任意 (0,1,2,,...,2^n-1) 的排列 p，并且满足：

p[0] = start

p[i] 和 p[i+1] 的二进制表示形式只有一位不同

p[0] 和 p[2^n -1] 的二进制表示形式也只有一位不同
 

### 示例 1：

输入：n = 2, start = 

输出：[3,2,0,1]

解释：这个排列的二进制表示是 (11,10,00,01)
     所有的相邻元素都有一位是不同的，另一个有效的排列是 [3,1,0,2]

### 示例 2：

输出：n = 3, start = 2

输出：[2,6,7,5,4,0,1,3]

解释：这个排列的二进制表示是 (010,110,111,101,100,000,001,011)
 

### 提示：

1 <= n <= 16

0 <= start < 2^n


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/circular-permutation-in-binary-representation
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

假设 `start` 是 `0`, 由样例二易得

`000,001,011,010,110,111,101,100...`

且首尾可相接

于是我神奇得发现,除去第一位,后面得就是前面反过来来一遍

`0,1`

`00,01,11,10` 当前面多了个`1`,后面一位是`0,1`反着来

`110,111,101,100` 同样,把首位的`1`去掉,就是上面一行反过来

就这么神奇得由样例看出了构造法,具体我也不知道怎么回事反正

## 代码
```cpp
class Solution {
public:
    vector<int> circularPermutation(int n, int start) {
        vector<int> res, v;
        int p = 0;
        v.push_back(0);
        for (int i = 0; i < n; ++i) {
            for (int j = 1; j <= (1<<i); ++j) {
                int tmp = (1<<i)+v[(1<<i)-j];
                v.push_back(tmp);
                if (start == tmp) p = v.size()-1;
            }
        }
        for (int i = 0; i < v.size(); ++i) {
            res.push_back(v[(p+i)%v.size()]);
        }
        return res;
    }
};
```