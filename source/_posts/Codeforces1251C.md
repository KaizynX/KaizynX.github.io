---
title: CF1251C. Minimize The Integer
date: 2019-10-29 14:35:00
categories:
  - Codeforces
tags:
  - 数学
  - 排序

mathjax: true
---

## [题面](https://codeforces.com/contest/1251/problem/C)

## 思路

发现奇数之间相对位置不变

偶数之间相对位置也不变

以上易证

奇偶数之间可以随便变

比如有序列 

`1010`->`1001`->`0101`->`0011`

如果一个偶数想换到一个奇数前面,

只要前面是奇数就可以一直向前

当碰到偶数停止,那么先让前面的偶数以相同方式前进

这个你想换的偶数就可以继续前进了

最后用归并思想把奇数堆偶数堆合并即可

## 代码

```cpp

int t, n;
int a[2][N], cnt[2];
char s[N];
 
int main()
{
    scanf("%d", &t);
    while (t--) {
        scanf("%s", s);
        n = strlen(s);
        cnt[0] = cnt[1] = 0;
 
        for (int i = 0, cur; i < n; ++i) {
            cur = s[i]-'0';
            a[cur&1][++cnt[cur&1]] = cur;
        }
        int i = 1, j = 1;
        while (i <= cnt[0] && j <= cnt[1]) {
            if (a[0][i] < a[1][j]) printf("%d", a[0][i++]);
            else printf("%d", a[1][j++]);
        }
        while (i <= cnt[0]) printf("%d", a[0][i++]);
        while (j <= cnt[1]) printf("%d", a[1][j++]);
        putchar('\n');
    }
    return 0;
}
```