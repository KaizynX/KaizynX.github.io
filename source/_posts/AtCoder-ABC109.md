---
title: AtCoder-ABC109
date: 2018-09-08 22:05:29
tags: AtCoder OI 数学
---

哈哈，又是每周一次的ABC
这周只有ABC没有ARC，因此题目不是很难
~~而我依然做不出来~~

----------
[A - ABC333][1] 
超级水


**题意**
给定A，B(1 <= A, B <= 3)，C在A，B之间(包含A, B)
求是否有A*B*C为奇数


**思路**
不妨设A < B
思路一： 打表法
因为数据很小，总共只有6种情况
(1, 1) (1, 2) (1, 3) (2, 2) (2, 3) (3, 3)
手算一下都可以，很快的


思路二：辨证的思维
因为在打表过程中发现自己打的和样例不符合
因祸得福，发现了此题的真谛
要求乘积为奇数，即乘积不是偶数
而如果乘积为偶数，那么乘积可以被2整除
因此三个数中不能出现偶数
也就是不能有2
因为A, B是确定的，两者间有2就肯定不行
至于C嘛，判到C就说明A，B是没有2了
C可选的为[A, B]，显然存在C==A或C==B
那么C就可以取到奇数
综上：只要A，B都不等于2，就是Yes， 否则为No
```cpp
#include <iostream>

using namespace std;

int a, b;

int main()
{
    cin >> a >> b;
    if(a == 2 || b == 2) cout << "No" << endl;
    else cout << "Yes" << endl;
    return 0;
}
```


----------
**[B - Shiritori][2]**
也是很水的

**题意**：
给N个单词,按顺序，要求前一个的末尾字符等于后一个首字符(即成语接龙),且这个单词之前没出现过

**思路**：
真的是没有一点水准
判断首尾很简单
判断重复哈希表，用map轻松搞定

```cpp
#include <map>
#include <iostream>

using namespace std;

const int Maxn = 107;

int n;
char last;
string str;
map<string, int> m;

int main()
{
    cin >> n >> str;
    m.insert(make_pair(str, 1));
    last = str[str.length()-1];
    for(int i = 1; i < n; ++i)
    {
        cin >> str;
        if(str[0] != last || m.count(str))
        {
            cout << "No" << endl;
            return 0;
        }
        m.insert(make_pair(str, 1));
        last = str[str.length()-1];
    }
    cout << "Yes" << endl;
    return 0;
}
```

----------

[C - Skip][3]
需要一点思考

**题意**
给N个坐标轴上的点Xi以及起点X
选择一个D，每次有两种移动方式
① X+D ② X-D
求D的最大值

**思路**
~~一开始还觉得是二分~~
根据样例可以找到一些规律的
因为每步走的长度都一样，我们可以采用离散化的思想，把D看成单位长度1
然后各个点之间的距离都离散一下
可以发现如果有x1, x2, x3从小到大，且都能到达
不存在这样的走法1-3-2, 也就是说要到3必须经过2，因为每步一样长
因此把起点X和各个点Xi放在一起排序，相邻两点的距离一定可以被D整除
那么就是gcd了
```cpp
#include <cstdio>
#include <iostream>
#include <algorithm>

using namespace std;

const int Maxn = 1e5+7;

int n, ans, x[Maxn];

int gcd(int a, int b)
{
    return b == 0 ? a : gcd(b, a%b);
}

int main()
{
    scanf("%d%d", &n, x);
    for(int i = 1; i <= n; ++i)
        scanf("%d", x+i);
    sort(x, x+n+1);
    ans = x[1]-x[0];
    for(int i = 2; i <= n; ++i)
        ans = gcd(ans, x[i]-x[i-1]);
    printf("%d\n", ans);
    return 0;
}

```

----------
D题没写出来，感觉挺简单可以写的，然而还是不会
我一直不服，0不也是偶数吗


----------
最后800名多点了，估计打ARC的都来ABC了，~~自我安慰~~

  [1]: https://beta.atcoder.jp/contests/abc109/tasks/abc109_a
  [2]: https://beta.atcoder.jp/contests/abc109/tasks/abc109_b
  [3]: https://abc109.contest.atcoder.jp/tasks/abc109_c