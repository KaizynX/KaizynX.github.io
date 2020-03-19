---
title: AtCoder-Triangular-Relationship
date: 2018-09-12 19:30:00
categories:
  - AtCoder
tags: 
  - 数学
 
mathjax: true
---

# AtCoder ABC 108 [C - Triangular Relationship][1] 

## 题意

给出N，K，用1~N组成三元组(a, b, c)

使得a+b, b+c, a+c都是K的倍数

数据范围1≤N, K≤2e5

## 思路

如果K为奇数, a, b, c为K的倍数

如果K为偶数, a, b, c为K/2的倍数


**简单证明**:

设a = na*K+x(na为自然数)

因为 a+b 是K的倍数

所以 b = nb*K-x(nb为自然数)

又因为 a+c 是K的倍数

所以 c = nc*K-x(nc为自然数)

又因为 b+c 也是K的倍数

所以 b+c = (nb+nc)*K - 2*x

即 2*x 是K的倍数

综上所述:

如果K为奇数, x是K的倍数

如果K为偶数, x是K/2的倍数

结论:

如果K为奇数, a, b, c为K的倍数

如果K为偶数, a, b, c为K/2的倍数

**证毕**

不妨设 a $\geq$ b $\geq$ c

1. 当 a == b == c 时 只有一种排列组合
2. 当 a == b > c 时 有$C_3^1$种排列组合
3. 当 a > b == c 时 有$C_3^1$种排列组合
4. 当 a > b > c 时 有$A_3^3$种排列组合
   
如果我们按 a $\geq$ b $\geq$ c 的顺序找下去

对于 a 取不同的值,容易证明这些排列组合不会重复

**tip**: 显然也可以看出K==1时候有N^3种，会爆int

---

1. 超级暴力O(n^3)
    
    枚举每种a, b, c的情况
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
            }
        }
    }
    printf("%lld\n", ans);
    return 0;
}
```
---
2. 简单优化至O(n^2)
   
    当确定a, b的值时, 就已经可以确定c的值了

    如果K为奇数, b 取 nb*K

    那么c 就是 K ~ nb*K

    - 当 b == c 时 三种

    - 当 b > c 时， 即 c 在 K ~ (nb-1)*K 时，每类有6种

        用一个简单的求和公式就可以求得总数为

        $\frac { ( 1+(nb-1) ) \times ( nb-1 ) }{2} = \frac { nb \times (nb-1) } {2}$

    代码不在累赘

---
3. 进一步计算得到O(n)
   
   当 a 确定时, 即可计算出b, c的取值的方法总数

   当 a 取 na*K

   - 当 a == b == c 时, 一种
   - 当 a == b 时 c 取值 K ~ (na-1)*K, 故有3 * (na-1)种
   - 当 b == c 时b,c取值 K ~ (na-1)*K, 也有3 * (na-1)种
   - 当 a > b > c 时, 每种6个排列组合
  
        当 b = (na-1)*K, c 有 na-2 种

        当 b = (na-2)*K, c 有 na-3 种

        ...

        当 b = 2*K, c 有 1 种

      就有$6*\sum_{i=2}^{na-1}{i-1}$ 种

      所以只需用一个数组保存一下这个前缀和

    当时在比赛中就只想到这种地步了, 毕竟可以O(n)过了

    数据也才10^5 (虽然有点奇怪怎么不是10^6)

    然后我就把大部分时间花在处理K为奇偶数的问题上

    还写了一个很骚皮的滚动数组

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
 聪明的你不难发现 $\sum_{i=2}^{na-1}{i-1}$ 是可以轻而易举求和的
 
 $=\sum_{i=1}^{na-2}{i} = \frac{(nb-1) \times (nb-2)}{2}$

 这连O(n)的辅助空间的省去了
 


---

4. 数论奥秘之O(1)
   
   以下就是比赛结束后我看见的大佬的代码

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
        cout << cub((n+k)/k) + cub(n/k) << endl;
    }
    return 0;
}
```

我琢磨了好几天终于搞明白了

就是在O(n)的基础上再数~~玄~~学推导

当 a 取 na*K

   - 当 a == b == c 时, 一种
   - 当 a == b 时 c 取值 K ~ (na-1)*K, 故有3 * (na-1)种
   - 当 b == c 时b,c取值 K ~ (na-1)*K, 也有3 * (na-1)种
   - 当 a > b > c 时, 每种6个排列组合
  
        当 b = (na-1)*K, c 有 na-2 种

        当 b = (na-2)*K, c 有 na-3 种

        ...

        当 b = 2*K, c 有 1 种

      就有$6 \times \sum_{i=2}^{na-1}{i-1}$ 种

      $= 6 \times \sum_{i=1}^{na-2}{i}$
      
      $= 6 \times \frac{(nb-1)*(nb-2)}{2}$

      $= 3 \times (nb-1) \times (nb-2)$

综上: 

设$F{(i)}$是 a 取 i*K 时

$F(i) = 1+6 \times (na-1)+3 \times (nb-1)\times (nb-2)$

$= 3 \times {i^2} - 3 \times i + 1$

所以:

先设 a 的取值可以有 K ~ X*K

$\sum_{i=1}^{X}F(i)$

$= 3 \times \sum_{i=1}^{X}{i^2}+3 \times \sum_{i=1}^{X}{i}+3 \times \sum_{i=1}^{X}{1}$

关于$\sum_{i=1}^{X}{i^2}$其实我并不会, 百度可以查到的

$\sum_{i=1}^{n}{i^2}=\frac{n(n+1)(2n+1)}{6}$

所以$\sum_{i=1}^{X}F(i)$

$=一系列的化简略略略$

$=X^3$

多么惊喜,多么意外

最后分类讨论

- 当 K 为奇数

  显然有 X = ⌊$\frac{N}{K}$⌋

  即 a = $K, 2K, 3K...⌊\frac{N}{K}⌋*K$

- 当 K 为偶数
  
  a = $\frac{1}{2}K, K,\frac{3}{2}K...⌊\frac{N}{\frac{1}{2}K}⌋*K$

  我们可以分为 $K,2K...$ 和 $\frac{1}{2}K,\frac{3}{2}K...$

  前一类有 ⌊$\frac{N}{K}$⌋个, 后一类就有 ⌊$\frac{N+K}{K}$⌋

  后一类怎么出来的?

  假设我们把 $\frac{1}{2}K,\frac{3}{2}K...$ 乘上2

  就是 $K,3K,5K...$

  就是 $(0+1)K, (2+1)K, (4+1)K...$

  再把2除回去...

  大致是这个意思吧...

**证毕!**

--- 

*更新于2018-10-05*

## 正解

就在我得意地证明出了这一切

回头看了看官方的题解,瞬间爆炸

原文:


> K が奇数の時、 a,b,c を K で割ったあまりはすべて 0 である必要があります。 K が偶数の時、 a,b,c をK で割ったあまりはすべて 0 であるか、あるいはすべて K/2 である必要があります。このような組の個数は、 N 以下で K で割って 0 あまるものの個数と K/2 あまるものの個数から求めることができるので、この問題を解くことができました。


就是这么短,标程都懒得给了

意思大致是:

1. 当 K 是奇数时

    根据我上面的推理

    a, b, c 可以选的是 K, 2K, 3K ...

    每个有 $\frac{N}{K}$ 种选择

    所以一共就有  $(\frac{N}{K})^3$ 种

2. 当 K 是偶数的时候

    同理

    注意此时是要分开算的

    K 的整数倍还是有 $\lfloor \frac{N}{K} \rfloor$ 种选择

    K 的整数加半个倍的有 (这里想到一个简单理解的)

    因为一共有 $\lfloor \frac{N}{K/2} \rfloor$

    所以这个就是 $\lfloor \frac{N}{K/2} \rfloor - \lfloor \frac{N}{K} \rfloor$

### 再现超短代码
```cpp
#include <bits/stdc++.h>
using namespace std;
int n, k;
inline long long calc(int x) { return 1ll*x*x*x; }
int main()
{
    cin >> n >> k;
    if(k&1) cout << calc(n/k) << endl;
    else cout << calc(n/k)+calc(n/(k/2)-n/k) << endl;
    return 0;
}
```

  [1]: https://beta.atcoder.jp/contests/abc108/tasks/arc102_a