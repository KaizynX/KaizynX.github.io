---
title: 递归改迭代
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2020-04-21 14:00:00
mathjax: true
categories:
  - 技术
tags:
  - 递归
  - 常数优化
description: switch大法好
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/recursion_to_iteration/cover.jpg 
---
# 序言

不得不承认递归是个神奇且强大的东西

但它却也存在致命的缺陷:递归层数太大会**爆栈**

另一方面递归开销也大于循环,因面临相关方面(时空受限)问题,于此寻找递归改迭代方法

# 正文

思路应该很明了?

就是自己模拟一个栈,记录函数运行的信息

栈是先进后出的数据结构,符合递归的要求

本试图在某度上寻找,结果都是无关紧要的尾递归改迭代?

下面就以 SCC|Tarjan 算法为例(其实我只会了这个)

## 例题汉诺塔

*20210406 update*

在做程序设计课程实践作业时有一题是用非递归实现汉诺塔

我当时就震惊了,递归改迭代这么高级的技术都能写吗

我并不知正解,也许是栈之类,反正我改迭代了,其实这个比下面的例子简单

自行看代码,懒得写解析了,可以直接看下面的例子

```cpp
void hanio(int n, int t1 = 1, int t2 = 2, int t3 = 3) {
  if (n < 1) return;
  hanio(n-1, t1, t3, t2);
  cout << "from " << t1 << " to " << t3 << '\n';
  hanio(n-1, t2, t1, t3);
}
```

```cpp
void hanio(int sz) {
  stack<int> sn, st1, st2, st3, ss;
  int n, t1, t2, t3, state;
  function<void()> push = [&]() {
    sn.push(n);
    st1.push(t1);
    st2.push(t2);
    st3.push(t3);
    ss.push(state);
  };
  function<void()> top = [&]() {
    n = sn.top();
    t1 = st1.top();
    t2 = st2.top();
    t3 = st3.top();
    state = ss.top();
  };
  function<void()> pop = [&]() {
    sn.pop();
    st1.pop();
    st2.pop();
    st3.pop();
    ss.pop();
  };
  n = sz; t1 = 1; t2 = 2; t3 = 3; state = 0;
  push();
  while (sn.size()) {
    top();
    pop();
    switch (state) {
    case 0:
      if (n < 1) continue;
      state = 1;
      push();
      n -= 1; swap(t2, t3); state = 0;
      push();
    continue;
    case 1:
      cout << "from " << t1 << " to " << t3 << '\n';
      n -= 1; swap(t1, t2); state = 0;
      push();
    }
  }
}
```

## 例题Tarjan

[[USACO06JAN]The Cow Prom S](https://www.luogu.com.cn/problem/P2863)

题目描述:

有一个 n 个点，m 条边的有向图，请求出这个图点数大于 1 的强联通分量个数。

## 递归版
接下来将根据这份代码来改成非递归版本

{% spoiler "代码" %}
```cpp
#include <bits/stdc++.h>

// #define DEBUG

using namespace std;

const int N = 1e4+7;
const int MOD = 998244353;
const int INF = 0x3f3f3f3f;
const double eps = 1e-7;
const double PI = acos(-1);
typedef pair<int, int> pii;

int n, m;
vector<int> e[N];
int cnt[N];

int _dfn, _col, _top;
int dfn[N], low[N], ins[N], col[N], sta[N];

void tarjan(const int &u)
{
    dfn[u] = low[u] = ++_dfn;
    ins[u] = 1;
    sta[++_top] = u;
    for (int i = 0; i < (int)e[u].size(); ++i) {
        int v = e[u][i];
        if (!dfn[v]) {
            tarjan(v);
            low[u] = min(low[u], low[v]);
        } else if (ins[v]) {
            low[u] = min(low[u], low[v]);
        }
    }
    if (dfn[u] == low[u]) {
        ++_col;
        do {
            col[sta[_top]] = _col;
            ins[sta[_top]] = 0;
        } while (sta[_top--] != u);
    }
}

signed main()
{
    cin >> n >> m;
    for (int i = 1, u, v; i <= m; ++i) {
        cin >> u >> v;
        e[u].emplace_back(v);
    }
    for (int i = 1; i <= n; ++i) {
        if (!dfn[i]) tarjan(i);
        ++cnt[col[i]];
        #ifdef DEBUG
        cout << col[i] << " \n"[i==n];
        #endif
    }
    int res = 0;
    for (int i = 1; i <= _col; ++i) {
        res += cnt[i] > 1;
    }
    cout << res << endl;
    return 0;
}
```

{% endspoiler %}

## 非递归版本
先贴代码
```cpp
int _dfn, _col, _top;
int dfn[N], low[N], ins[N], col[N], sta[N];

struct Stack_Node {                                         // 1
    int u;
    int edge_info;
    int state;
} tarjan_stack[N];

void tarjan(const int &u) {
    static int stack_top;
    stack_top = 0;                                          // 2
    tarjan_stack[++stack_top] = {u, 0, 0};
    while (stack_top) {
        bool flag = false;                                  // 3
        int &u = tarjan_stack[stack_top].u;                 // 4
        auto &i = tarjan_stack[stack_top].edge_info;
        switch (tarjan_stack[stack_top].state) {
        case 0 :
        tarjan_stack[stack_top].state = 1;                  // 5
        flag = false;
            dfn[u] = low[u] = ++_dfn;
            ins[u] = 1;
            sta[++_top] = u;
            for (; i < (int)e[u].size(); ++i) {
#define v e[u][i]                                           // 6
                if (!dfn[v]) {
                    tarjan_stack[++stack_top] = {v, 0, 0};  // 7
        flag = true;
                    break;
        case 1 :                                            // 8
        flag = false;
                    low[u] = min(low[u], low[v]);
                } else if (ins[v]) {
                    low[u] = min(low[u], low[v]);
                }
#undef v
            }
        if (flag) break;                                     // 9
            if (dfn[u] == low[u]) {
                ++_col;
                do {
                    col[sta[_top]] = _col;
                    ins[sta[_top]] = 0;
                } while (sta[_top--] != u);
            }
        --stack_top;
        }
    }
}
```
以上代码如果只看三层缩进的代码,其实就几乎是递归版本的代码

一层缩进是模拟栈

二层缩进就是神奇的 switch 大法控制递归状态了(重点)

记住一个特性,如果 switch 没有 break, 那么匹配项(case) 之后的代码会一直执行下去,直到 break 或 switch 末尾为止

1. 定义了个结构体用来存储函数运行的信息

   依次存了当前的结点 u

   循环到的边的信息 edge_info(本文存储的是边的序号,也可以存迭代器等等)

   函数的递归状态 state(后面详细讲)

2. 因为 tarjan 函数不只执行一次,记得清空栈(虽然不清空一般没事)

   然后就是原本第一层的函数,将它入栈,开始模拟栈

3. 这个 flag 标记很重要(重点)后面详细讲

4. 取出栈里的信息,得到当前函数的信息

   注意我在这里采用了**引用**的形式,因为函数一般执行那么函数信息就会改变

   比如你遍历了一条边,该轮到下一条边了,栈里存储的信息也要对于改变,用引用就省去了记录

5. 修改状态,下面讲

6. 注意这里使用了宏替换,也是方便起见,为什么不能像原来一样呢?

   因为如果你直接跳转到了 case 1, 这个 v 就是未定义的

   不过方便的同时这个宏定义也非常危险(宏定义代码块内不得出现字符 v )

   当然最保险的办法还是老老实实写 e[u][i]

7. 重点来了,递归部分,我们模拟栈,那么应该入栈(开辟下一层函数)

   然后我们应该暂时跳出当前函数,这里通过 flag 来实现

   这里真就是 switch 的神奇了.注意这里的 break, 跳出的是 for 循环(不是 case)

   也就是说我们来到了 **9** 的位置(通过简单的 gdb 调试易得)

   通过 break,我们就推出了 switch 也就相当于推出了当前函数

8. 注意到 **5**, 这一层函数递归状态改为了 1, 如果没有进入下一层(**7**)

   就会继续执行下去没有影响
   
   但如果有下一层递归,我们知道下一层(**7**)执行完毕后要回到 **8**, state 的记录满足了这一条件,同时不要忘记把 flag 改回去

9.  如 **7** 所述


如果不明白想一下所有情况

1. if(!dfn[v]) 进入下一层递归,在 **7** 处入栈,跳出switch,下一层 while, 此时栈顶就是新加的函数

2. if(dfn[v]) 跳到 elseif 正常执行

3. 执行完了回退到上一层递归函数,会进入 case 1, 循环往复


如果还不明白呢,建议单步调试一下看一下过程(毕竟我只会口胡

# 总结

据此可以类比到任何形式递归改迭代了吧?(反正我不行(逃

总的来说并不是很好写吧(递归不香吗

如果不是对时间空间有限制的话(汇编手动扩栈不香吗

# 参考资料

[github Yangff/dfs-benchmark](https://github.com/Yangff/dfs-benchmark)