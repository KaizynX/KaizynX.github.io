---
title: AtCoder-ABC108
date: 2018-09-01 21:49:29
categories:
  - AtCoder
tags:
  - 数学

mathjax: true
---

今日九月一日晚，正是一年一度~~灾难~~开学日
北京时间晚上八点整，正是一年一度的央视大型~~邪教~~教育片《开学第一课》的放映时间
同时也是重要的AtCoder比赛开始的时间
于是我~~毫不犹豫~~踌躇三分，最后~~开开心心~~忍痛割爱的抛弃了开学第一课，投入了ABC的怀抱

----------

[A - Pair][1]
大水题
**题意**：给定整数N，求1~N中奇数和偶数的配对个数
**思路**：如果N为偶数，奇数和偶数都为 N/2 个
如果N为奇数，奇数有(N-1)/2个，即⌊N/2⌋个，偶数有(N+1)/2，即⌈N/2⌉个
合并可得答案即为 ⌊N/2⌋*⌈N/2⌉
由于C++ 的 int 向0取整的特性
显然可得
```cpp
#include <cstdio>
#include <iostream>

using namespace std;

int k;

int main()
{
    scanf("%d", &k);
    int a = k/2, b = (k+1)/2;
    printf("%d\n", a*b);
    return 0;
}
```
----------
[B - Ruined Square][2] 
水题
**题意**：坐标轴上逆时针的正方形，四个点给出前两个求后两个
**思路**：画一下就出来了，懒得数学推导，反正跟着一个样例的特殊的数字敲关系就出来了
题目也许不水，被我水过了……
```cpp
#include <cstdio>
#include <iostream>

using namespace std;

int x1, x2, x3, x4, y1, y2, y3, y4;

int main()
{
    scanf("%d%d%d%d", &x1, &y1, &x2, &y2);
    int a = x1-x2, b = y1-y2;
    x3 = x2+b; y3 = y2-a;
    x4 = x1+b; y4 = y1-a;
    printf("%d %d %d %d\n", x3, y3, x4, y4);
    return 0;
}
```
**tip**：发现题目给得数据都是第二个点在第一个点右上方，怀疑有坑
我把数据反过来输入，发现并无大碍

----------
[C - Triangular Relationship][3] 
有点意思
C题也就是ARC第一题了，如果去看ARC排名会发现一个有趣得现象
七百多人参加，六十人写出三题
从326到722都只写了第一题，就是C题
想起上周参加ARC只写出一题的我，这次被迫来打ABC了哈哈

**题意：**给出N，K，用1~N组成三元组(a, b, c)
使得a+b, b+c, a+c都是K的倍数
数据范围1≤N,K≤2e5
**思路**：看到数据范围就想着骚皮的做法了
首先我想到假设三个数字一样，设为i, i, i,则显然成立
如果把某个数字减去K(前提是能减),那么也显然还是成立
那么要枚举这些情况，先定最大的为i，然后递减的枚举
还想到如果K是偶数可以切一半怎样怎样的……
最终没能做出来哈哈
于是回归根本，写了个暴力的O(n^3)
假设三个数单调递减
发现三个数如果相同就一种排列
两个相同就3种排列
全不一样就有A(3,3)=6种排列
**tip**: 显然也可以看出K==1时候有N^3种，会爆int
```cpp
#include <cstdio>
#include <iostream>

using namespace std;

const int Maxn = 2e5+7;

int n, k, add;
long long ans;

int main()
{
    scanf("%d%d", &n, &k);
    add = k&1 ? k : k/2;
    for(int i = add; i <= n; i += add)
    {
        for(int j = i; j > 0; j -= k)
        {
            for(int j2 = j; j2 > 0; j2 -= k)
            {
                if(i == j && j == j2) ans++;
                else if(i == j || j == j2 || i == j2) ans += 3;
                else ans += 6;
                printf("%d %d %d %lld\n", i, j, j2, ans);
            }
        }
    }
    printf("%lld\n", ans);
    return 0;
}
```
然后就可以进一步得出一些关系了
根据数据范围，决定采用O(n)得算法
对于递减得三个数i, j, j2

 1. 如果三个数相等只有一种
 2. 如果两个数相等要么 i == j 要么 j == j2 其中 i == j 得时候，j2 的选择有 i/k-(i%k == 0)种
    ①如果 j2 是 k 的倍数，那么 j2 可选 k, 2k, 3k ... i 有 i/k-1 种，-1是因为第一个为 0 不能取
    ②如果j2 = n*k+k/2 那么就有 i/k种了 于是我聪明机智的得出了以上的式子 i/k-(i%k == 0)
 3. 如果三个数都不相等，假设递减，每种情况6种排列，但是有几对 j, j2呢
    如果 j == nk，j2就有 (n-1)k, (n-2)k ... k
    如果 j == (n-1)k, j2 就有 (n-2)k, (n-3)k ... k
    显然就是之前求得的 i/k-(i%k == 0) 式子的和了，我用 s[] 保存，就可以O(1)求出了
    但如果 j == nk+k/2 ，情况不同，但也类似，只是两种情况 s[] 要分开存
    赛场上我就凭借过人的机智想出了骚皮的滚动数组，详情见代码

```cpp
#include <cstdio>
#include <iostream>

using namespace std;

const int Maxn = 2e5+7;

int n, k, cnt, add;
long long ans;
long long s[2][Maxn];

int main()
{
    scanf("%d%d", &n, &k);
    if(k == 1)
    {
        printf("%lld\n", 1ll*n*n*n);
        return 0;
    }
    add = k&1 ? k : k/2;
    for(int i = add, cnt = 1; i <= n; i += add, ++cnt)
    {
        // i i i
        ans++;
        // i i j && i j j
        ans += (i/k-(i%k == 0))*3*2;
        s[0][cnt] = s[0][cnt-1];
        s[1][cnt] = s[1][cnt-1];
        s[i%k == 0][cnt] += i/k-(i%k == 0);
        // i j j2
        ans += s[i%k == 0][cnt-1]*6;
    }
    printf("%lld\n", ans);
    return 0;
}
```
然后就A了，前两题用时20分钟，第三题想了一小时！
最后剩下不到20分钟，最后一题就没写了~~(就算有时间也写不来)~~
当时C题就这么草草过了，其实还有一些地方可以再推导
赛后看了大佬的代码，颇为震惊，↓
```cpp
#include <iostream>

using namespace std;

long long cub(int x) {
    return 1ll * x * x * x;
}

int main() {
    int n, k;
    cin >> n >> k;
    if (k & 1) {
        cout << cub(n/k) << endl;
    } else {
        k /= 2;
        cout << cub((n+k)/k/2) + cub(n/k/2) << endl;
    }
    return 0;
}
```
有待思考


----------
最后取得了第 300 名整的成绩，rating 74 --> 309
别管我为什么第一次才 74 ...

  [1]: https://beta.atcoder.jp/contests/abc108/tasks/abc108_a
  [2]: https://beta.atcoder.jp/contests/abc108/tasks/abc108_b
  [3]: https://beta.atcoder.jp/contests/abc108/tasks/arc102_a