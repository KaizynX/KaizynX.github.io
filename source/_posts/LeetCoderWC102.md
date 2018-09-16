---
title: LeetCode Weekly Contest 102
date: 2018-09-16 11:30:00
tags:
  - LeetCode
  - 数学
---

# 905. [按奇偶校验排序数组](https://leetcode-cn.com/contest/weekly-contest-102/problems/sort-array-by-parity)

给定一个非负整数数组 A，返回一个由 A 的所有偶数元素组成的数组，后面跟 A 的所有奇数元素。

你可以返回满足此条件的任何数组作为答案。

## 数据范围：
1. 1 <= A.length <= 5000

2. 0 <= A[i] <= 5000

## 思路

根本不用思考

```cpp
class Solution {
public:
    vector<int> sortArrayByParity(vector<int>& A) {
        vector<int> tmp0, tmp1;
        for(auto i : A)
        {
            if(i&1) tmp1.emplace_back(i);
            else tmp0.emplace_back(i);
        }
        tmp0.insert(tmp0.end(), tmp1.begin(), tmp1.end());
        return tmp0;
    }
};
```

---
# 904. [水果成篮](https://leetcode-cn.com/contest/weekly-contest-102/problems/fruit-into-baskets)

在一排树中，第 i 棵树产生 tree[i] 型的水果。

你可以从你选择的任何树开始，然后重复执行以下步骤：

1. 把这棵树上的水果放进你的篮子里。如果你做不到，就停下来。
2. 移动到当前树右侧的下一棵树。如果右边没有树，就停下来。

请注意，在选择一颗树后，你没有任何选择：你必须执行步骤 1，然后执行步骤 2，然后返回步骤 1，然后执行步骤 2，依此类推，直至停止。

你有两个篮子，每个篮子可以携带任何数量的水果，但你希望每个篮子只携带一种类型的水果。

用这个程序你能收集的水果总量是多少？

## 数据范围

1. 1 <= tree.length <= 40000
2. 0 <= tree[i] < tree.length

## 思路

一眼看穿:这不是某年NOIP初赛的完善程序吗?

就是找一段连续的子序列,子序列里面只有两种值

求这样子序列最长有多长

~~题面全是废话~~

看代码,如果看不懂那就是连NOIP初赛水准都没有

```cpp
class Solution {
public:
    int totalFruit(vector<int>& tree) {
        int ans = 0, fr1 = tree[0], fr2 = -1, ct1 = 1, ct2 = 1, l = 0, r = 1;
        while(r < tree.size() && tree[r] == fr1) ct1++, r++;
        if(r == tree.size()) return ct1;
        fr2 = tree[r++];
        while(r < tree.size() && tree[r] == fr2) ct2++, r++;
        ans = ct1+ct2;
        if(r == tree.size()) return ans;
        
        while(r < tree.size())
        {
            if(tree[r] == fr1)
            {
                ++ct1;
                ans = max(ans, ct1+ct2);
            }
            else if(tree[r] == fr2)
            {
                ++ct2;
                ans = max(ans, ct1+ct2);
            }
            else
            {
                int &delt = tree[r-1] == fr1 ? ct2 : ct1;
                int &delf = tree[r-1] == fr1 ? fr2 : fr1;
                while(delt > 0)
                {
                    if(tree[l] == fr1) ct1--;
                    else ct2--;
                    l++;
                }
                delf = tree[r];
                delt = 1;
            }
            ++r;
        }
        return ans;
    }
};
```
---
# 907. [子数组的最小值之和](https://leetcode-cn.com/contest/weekly-contest-102/problems/sum-of-subarray-minimums/)


给定一个整数数组 A，找到 min(B) 的总和，其中 B 的范围为 A 的每个（连续）子数组。

由于答案可能很大，因此返回答案模 10^9 + 7。

## 数据范围

1. 1 <= A <= 30000
2. 1 <= A[i] <= 30000

## 思路

在最后一刻(比赛结束了)做了出来T_T

这个从暴力开始推吧

1. 超级暴力O(n^3)
   
```cpp
int fa(vector<int> &A)
{
    int res = 0;
    for(int l = 0; l < A.size(); ++l)
    {
        for(int r = l; r < A.size(); ++r)
        {
            int minv = 30001;
            for(int k = l; k <= r; ++k)
            {
                minv = min(minv, A[k]);
            }
            res += minv;
        }
    }
    return res;
}
```

2. 暴力优化O(n^2)

    发现当计算某个区间[l, r]时

    [l, r]的最小值就是min{[l, r-1]最小值, a[r]}

```cpp
int fa(vector<int> &A)
{
    int res = 0, minv;
    for(int l = 0; l < A.size(); ++l)
    {
        minv = A[l];
        for(int r = l; r < A.size(); ++r)
        {
            minv = min(minv, A[r]);
            res += minv;
        }
    }
    return res;
}
```

3. 盲目思考

    这也是我一开始根本放弃暴力而采取的思维方式

    设A[i]是数组中最小的值

    那么包含A[i]的子序列里的最小值都是A[i]

    所以`res+=A[i]*包含A[i]的子序列个数`

    然后我想包括A[i]的子序列个数怎么求

    如果以A[i]为左端点的子序列,有
    ```

    i
    i, i+1
    i, i+1, i+2
    ...
    i, i+1, i+2, ... A[A.length()-1]
    ```

    就有`(A.length()-1)-i+1`个

    如果以A[i]为右端点

    同理得有`i-0+1`个

    再根据排列组合乘法原理

    加上A[i]是序列中间某一点得时候

    总共有`((A.length()-1)-i+1)*(i-0+1)`个

    综上:`res+=A[i]*((A.length()-1)-i+1)*(i-0+1)`

    ~~然后第二小的emmmmm...~~

    *以上是启蒙思考...*

    总之重要得得出区间内子序列个数得求法

    后来我就想到对于任意一个A[i]

    以它为最小值得区间i往左找到最左边不小于它得位置

    右边也一样找到最右边不小于它得位置

    设l <= i <= r

    使得[l,r]区间内A[i]为最小值

    也即A[l-1] < A[i], A[r+1] < A[i]

    所以就有`res+=A[i]*((i-l+1)*(r-i+1))`

    这真的是完美~~无缺~~!

    但是谨慎得我仔细一想:
    
    如果存在A[i] == A[j]就会有重复的

    *好吧,这也是启蒙思路...*

    然后我就卡如何去重好久了

    于是回归了暴力的本质,想去试探O(30000^2)的边缘

    妥妥的TLE,~~还发现上面的代码都忘了取模~~

    暴力着暴力着就出思路了

4. 应该是O(n)的解法

    同样建立在寻找以A[i]为最小值的区间上

    去重的方法,找到上文所示的l, r,微调

    A[l-1] < A[i], A[r+1] **<=** A[i]

    就是把重复的那部分包含到左边值一样的点

    ~~一时语塞,不知该怎么解释,先搁着~~

    至于时间复杂度

    每个元素不是都要向左向右找

    难道不是O(n^2)?

    不,如果A[i]是最小值,那么就会找完全部

    如果A[i]是最大值,那么只会找左右各1个位置就结束了

    虽然没能数学证明,但直觉论证

    平均下来的时间复杂的应该为O(k*n)

    这个常数k,嗯...我觉得是2~3

    但是我们还说过两个值相同的情况

    如果全部都一样,还是退化成了O(n^2)

    不过这只是算法的最坏情况

    平均情况还是O(n)得

    题目数据A[i]得大小和个数范围一样

    让人会误以为A[i]都不相同,其实没有这么说

    反正我交上去是过了


```cpp
class Solution {
public:
    int sumSubarrayMins(vector<int>& A) {
        int res = 0, mo = 1e9+7, minv;
        for(int i = 0; i < A.size(); ++i)
        {
            int l = i, r = i;
            while(--l >= 0 && A[l] > A[i]);
            while(++r < A.size() && A[r] >= A[i]);
            ++l; --r;
            res = (res+A[i]*(i-l+1)*(r-i+1))%mo;
        }
        return res;
    }
};
```

ps:后来我测试了一下所谓得最差情况得数据

600ms惊了,难道这只是我的直觉出了问题吗