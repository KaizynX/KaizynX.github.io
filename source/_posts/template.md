---
title: NOIP模板
date: 2018-11-05 07:00:00
categories:
  - NOIP
tags:
top: true
---
# NOIP2018模板
---
## 前言
```cpp
while(true) ++++++ ++++++ ++++++ RP;
```
<!--more-->
---
## 快读快写
```cpp
template <typename T> inline void read(T &x)
{
    char c; T tag = 1;
    while(!isdigit((c=getchar()))) if(c == '-') tag = -1;
    x = c-'0';
    while(isdigit((c=getchar()))) x = (x<<1)+(x<<3) + c-'0';
    x *= tag;
}

template <typename T> void write(T x)
{
    if(x < 0) x = -x, putchar('-');
    if(x > 9) write(x/10);
    putchar(x%10+'0');
}
```
```cpp
ios::sync_with_stdio(false);
cin.tie(NULL);
```
---
## [堆](https://www.luogu.org/problemnew/show/P3378)
```cpp
struct Heap
{
    static const int Maxn = 1e6+7;
    int sz, a[Maxn];
    Heap() { sz = 0; memset(a, 0, sizeof a); }
    inline bool cmp(int x, int y) { return x < y; } // 小根堆
    inline int size() { return sz; }
    inline bool empty() { return sz == 0; }
    inline int top() { return a[1]; }
    inline void push(int x) { a[++sz] = x; swift_up(sz); }
    inline void pop() { swap(a[1], a[sz--]); swift_down(1); }
    inline void swift_up(int p)
    {
        while(p > 1 && cmp(a[p], a[p>>1])) // a[p] < a[p<<1]
            swap(a[p], a[p>>1]), p >>= 1;
    }
    inline void swift_down(int p)
    {
        int l, r, s;
        while(true)
        {
            l = p<<1; r = p<<1|1;
            if(l > sz) break;
            if(r > sz || cmp(a[l], a[r])) s = l; // a[l] < a[r]
            else s = r;
            if(cmp(a[s], a[p])) // a[s] < a[p]
                swap(a[p], a[s]), p = s;
            else break;
        }
    }
};
```
---
## 字符串
### [manacher算法](https://www.luogu.org/problemnew/show/P3805)(回文字符串)
```cpp
inline int manacher(const char *str, char *buf, int *p)
{
    int str_len = strlen(str), buf_len = 2;
    buf[0] = buf[1] = '#';
    for(int i = 0; i < str_len; ++i)
        buf[buf_len++] = str[i], buf[buf_len++] = '#';

    int mx = 0, id, ans = 0;
    for(int i = 1; i < buf_len; ++i)
    {
        if(i <= mx) p[i] = min(p[id*2-i], mx-i);
        else p[i] = 1;
        while(buf[i-p[i]] == buf[i+p[i]]) p[i]++;
        if(i+p[i] > mx) mx = i+p[i], id = i;
        ans = max(ans, p[i]-1);
    }
    return ans;
}
```
### [KMP](https://www.luogu.org/problemnew/show/P3375)
```cpp
len_a = strlen(a+1);
len_b = strlen(b+1);
for(int i = 2, k = 0; i <= len_b; ++i)
{
    while(k && b[k+1] != b[i]) k = _next[k];
    if(b[k+1] == b[i]) ++k;
    _next[i] = k;
}
for(int i = 1, j = 0; i <= len_a; ++i)
{
    while(j && a[i] != b[j+1]) j = _next[j];
    if(b[j+1] == a[i]) ++j;
    if(j == len_b)
    {
        cout << i-len_b+1 << endl;
        j = _next[j];
    }
}
```
[大佬的KMP](https://blog.csdn.net/v_july_v/article/details/7041827)
```cpp
inline void GetNext(char *str, int *_nex)
{
    int len = strlen(str);
    int k = -1, j = 0;
    _nex[0] = -1;
    while (j < len-1) {
        if (k == -1 || str[k] == str[j]) {
            ++k; ++j;
            _nex[j] = (str[k] == str[j] ? _nex[k] : j);
        } else {
            k = _nex[k];
        }
    }
}

inline int KMP(char *s, char *p)
{
    int i = 0, j = 0;
    int lens = strlen(s), lenp = strlen(p);
    while (i < lens && j < lenp) {
        if(j == -1 || s[i] == p[j]) {
            ++i; ++j;
        } else {
            j = nex[j];
        }
    }
    return j == lenp ? i-j : -1;
}
```
### BM算法
### Sunday算法
### [扩展KMP(Z函数)](https://subetter.com/algorithm/extended-kmp-algorithm.html)
```cpp
inline void GetNext(char *s, int *_nex)
{
    int len = strlen(s);
    int a = 0, p = 0;
    _nex[0] = len;
    for (int i = 1; i < len; ++i) {
        if (i >= p || i+_nex[i-a] >= p) {
            if (i > p) p = i;
            while (p < len && s[p] == s[p-i]) ++p;
            a = i;
            _nex[i] = p-i;
        } else {
            _nex[i] = _nex[i-a];
        }
    }
}

inline void GetExtend(char *s, char *ss, int *_ext, int *_nex)
{
    int lens = strlen(s), lenss = strlen(ss);
    int a = 0, p = 0;
    for (int i = 0; i < lens; ++i) {
        if (i >= p || i+_nex[i-a] >= p) {
            if (i > p) p = i;
            while (p < lens && p-i < lenss && s[p] == ss[p-i]) ++p;
            a = i;
            _ext[i] = p-i;
        } else {
            _ext[i] = _nex[i-a];
        }
    }
}
```


### [字符串哈希](https://www.luogu.org/problemnew/show/P3370)
```cpp
inline unsigned long long _hash(const string &s)
{
    unsigned long long res = 0;
    for(int i = 0; i < s.length(); ++i)
        res = (res*Base+s[i])%Mod+Prime;
    return res;
}
```
---
## 快排
```cpp
void quick_sort(int l, int r)
{
    if(l >= r) return;
    swap(a[l], a[l+rand()%(r-l)]);
    int i = l, j = r, mid = a[l];
    while(i < j)
    {
        while(i < j && a[j] >= mid) --j;
        swap(a[i], a[j]);
        while(i < j && a[i] < mid) ++i;
        swap(a[i], a[j]);
    }
    quick_sort(l, i-1);
    quick_sort(i+1, r);
}
```
---
## 求第K大数
[HDOJ 2665](http://acm.hdu.edu.cn/showproblem.php?pid=2665)
[POJ 2104](http://poj.org/problem?id=2104)
```cpp
int kth_element(int l, int r, int k)
{
    if(l == r) return a[l];
    swap(a[l], a[l+rand()%(r-l)]);
    int mid = a[l], i = l, j = r;
    while(i < j)
    {
        while(i < j && a[j] >= mid) --j;
        swap(a[i], a[j]);
        while(i < j && a[i] < mid) ++i;
        swap(a[i], a[j]);
    }
    a[i] = mid;
    if(i == k) return mid;
    else if(i > k) return kth_element(l, i-1, k);
    else return kth_element(i+1, r, k);
}
```
**STL** (排序,无返回值)
```cpp
nth_element(a+1, a+k+1, a+n+1);
```
---
## [求逆序对(归并排序)](https://www.luogu.org/problemnew/show/P1908)
```cpp
void merge_sort(int l, int r)
{
    if(l == r) return;
    int mid = (l+r)>>1;
    merge_sort(l, mid);
    merge_sort(mid+1, r);
    int i = l, j = mid+1, k = l;
    while(k <= r)
    {
        if(j <= r && (i > mid || a[j] < a[i]))
        {
            ans += mid-i+1;
            b[k++] = a[j++];
        }
        else b[k++] = a[i++];
    }
    memcpy(a+l, b+l, sizeof(int)*(r-l+1));
}
```
---
## 树
### [树的重心](https://www.luogu.org/problemnew/show/P2986)
```cpp
void treedp(int cur, int fa)
{
    s[cur] = c[cur];
    for(int i = fir[cur]; i; i = nex[i])
    {
        if(e[i] == fa) continue;
        treedp(e[i], cur);
        s[cur] += s[e[i]];
        maxs[cur] = max(maxs[cur], s[e[i]]);
    }
    maxs[cur] = max(maxs[cur], sum-s[cur]);
}
```
### 二叉查找树
### [平衡树](https://www.luogu.org/problemnew/show/P3369)
#### [Splay](https://www.luogu.org/blog/user19027/solution-p3369)
```cpp
struct Splay
{
    #define root e[0].ch[1]
    typedef int T;
    struct node
    {
        T v = 0;
        int ch[2] = { 0, 0 };
        int fa = 0, sum = 0, cnt = 0;
    } e[N];
    int n;
    void update(int x) { e[x].sum = e[e[x].ch[0]].sum+e[e[x].ch[1]].sum+e[x].cnt; }
    int identify(int x) { return x == e[e[x].fa].ch[1]; }
    void connect(int x,int f,int son) { e[x].fa = f; e[f].ch[son] = x; }
    void rotate(int x) {
        int y = e[x].fa,
            r = e[y].fa,
            rson = identify(y),
            yson = identify(x),
            b = e[x].ch[yson^1];
        connect(b, y, yson);
        connect(y, x, yson^1);
        connect(x, r, rson);
        update(y); update(x);
    }
    void splay(int at,int to) {
        to = e[to].fa;
        int up;
        while((up = e[at].fa) != to) {
            if(e[up].fa != to)
                rotate(identify(up) == identify(at) ? up : at);
            rotate(at);
        }
    }
    int add_point(T v, int fa) {
        ++n; e[n].v = v; e[n].fa = fa; e[n].sum = e[n].cnt = 1;
        return n;
    }
    int find(T v) {
        int now = root, last = 0;
        while (now && e[now].v != v)
            last = now, now = e[now].ch[v > e[now].v];
        splay((now ? now : last), root);
        return now;
    }
    void insert(T v) {
        if (!root) { root = add_point(v, root); return; }
        int now = root, last = 0;
        while (now && e[now].v != v)
            last = now, now = e[now].ch[v > e[now].v];
        if (now) ++e[now].cnt;
        else now = e[last].ch[v > e[last].v] = add_point(v, last);
        splay(now, root);
    }
    void erase(T v) {
        int del = find(v);
        if (!del) return;
        if (e[del].cnt > 1) {
            --e[del].cnt;
            --e[del].sum;
        } else if (!e[del].ch[0]) {
            root = e[del].ch[1];
            e[root].fa = 0;
        } else {
            int oldroot = root;
            splay(nex(e[del].ch[0], 1), root);
            connect(e[oldroot].ch[1], root, 1);
            update(root);
        }
    }
    int rank(T v) { return e[e[find(v)].ch[0]].sum+1; }
    T atrank(int x) {
        if (x > e[root].sum) return -INF;
        int now = root;
        while (true) {
            if (x <= e[e[now].ch[0]].sum) now = e[now].ch[0];
            else if ((x -= e[e[now].ch[0]].sum) <= e[now].cnt) break;
            else x -= e[now].cnt, now = e[now].ch[1];
        }
        splay(now, root);
        return e[now].v;
    }
    // small 0, big 1
    int nex(int x, int opt) { while (e[x].ch[opt]) x = e[x].ch[opt]; return x; }
    T lower(T v, int opt) {
        insert(v);
        T res = e[nex(e[root].ch[opt], opt^1)].v;
        erase(v);
        return res;
    }
    #undef root
};
```
区间反转
```cpp
struct Splay
{
    typedef int T;
    struct node
    {
        T v = 0;
        int ch[2] = { 0, 0 };
        int fa = 0, sum = 0, cnt = 0, tag = 0;
    } e[N];
    int sz, &root = e[0].ch[1];
    void update(int x) { e[x].sum = e[e[x].ch[0]].sum+e[e[x].ch[1]].sum+e[x].cnt; }
    int identify(int x) { return x == e[e[x].fa].ch[1]; }
    void connect(int x,int f,int son) { e[x].fa = f; e[f].ch[son] = x; }
    void rotate(int x) {
        int y = e[x].fa,
            r = e[y].fa,
            rson = identify(y),
            yson = identify(x),
            b = e[x].ch[yson^1];
        connect(b, y, yson);
        connect(y, x, yson^1);
        connect(x, r, rson);
        update(y); update(x);
    }
    void splay(int at,int to = 0) {
        to = e[to].fa;
        int up;
        while((up = e[at].fa) != to) {
            if(e[up].fa != to)
                rotate(identify(up) == identify(at) ? up : at);
            rotate(at);
        }
    }
    int add_point(T v, int fa) {
        ++sz; e[sz].v = v; e[sz].fa = fa; e[sz].sum = e[sz].cnt = 1;
        return sz;
    }
    int find(int x) {
        if (x > e[root].sum) return -INF;
        int now = root;
        while (true) {
            push_down(now);
            if (x <= e[e[now].ch[0]].sum) now = e[now].ch[0];
            else if ((x -= e[e[now].ch[0]].sum) <= e[now].cnt) break;
            else x -= e[now].cnt, now = e[now].ch[1];
        }
        return now;
    }
    int build(int l, int r, int fa) {
        if (l > r) return 0;
        int mid = (l+r)>>1,
            now = add_point(mid, fa);
        e[now].ch[0] = build(l, mid-1, now);
        e[now].ch[1] = build(mid+1, r, now);
        update(now);
        return now;
    }
    void push_down(int x) {
        if (x && e[x].tag) {
            e[e[x].ch[0]].tag ^= 1;
            e[e[x].ch[1]].tag ^= 1;
            swap(e[x].ch[0], e[x].ch[1]);
            e[x].tag = 0;
        }
    }
    void reverse(int l, int r) {
        int pl = find(l-1+1), pr = find(r+1+1);
        splay(pl); splay(pr, pl);
        e[e[e[root].ch[1]].ch[0]].tag ^= 1;
    }
    void print_LMR(int x) {
        if (!x) return;
        push_down(x);
        print_LMR(e[x].ch[0]);
        if (e[x].v != 0 && e[x].v != n+1)
            write(a[e[x].v]), putchar(' ');
        print_LMR(e[x].ch[1]);
    }
} tree;
```
### [线段树](https://www.luogu.org/problemnew/show/P3372)
区间修改区间查询
```cpp
struct Tree
{
    int l, r;
    long long sum, add;
    Tree(){ sum = add = 0; }
} tr[Maxn<<2];

inline int ls(int x) { return x<<1; }
inline int rs(int x) { return x<<1|1; }
inline void push_up(int);
inline void push_down(int);
inline void build_tree(int, int, int);
inline void update_tree(int, int, int, long long);
inline long long query_tree(int, int, int);

inline void push_up(int i)
{
    tr[i].sum = tr[ls(i)].sum+tr[rs(i)].sum;
}

inline void push_down(int i)
{
    if(!tr[i].add) return;
    int len = tr[i].r-tr[i].l+1;
    tr[ls(i)].add += tr[i].add;
    tr[rs(i)].add += tr[i].add;
    tr[ls(i)].sum += tr[i].add*(len-len/2);
    tr[rs(i)].sum += tr[i].add*(len/2);
    tr[i].add = 0;
}

inline void build_tree(int i, int l, int r)
{
    tr[i].l = l; tr[i].r = r;
    if(l == r) { tr[i].sum = a[l]; return; }
    int mid = (l+r)>>1;
    build_tree(ls(i), l, mid);
    build_tree(rs(i), mid+1, r);
    push_up(i);
}

inline void update_tree(int i, int l, int r, long long k)
{
    if(tr[i].l >= l && tr[i].r <= r)
    {
        tr[i].sum += k*(tr[i].r-tr[i].l+1);
        tr[i].add += k;
        return;
    }
    push_down(i);
    int mid = (tr[i].l+tr[i].r)>>1;
    if(l <= mid) update_tree(ls(i), l, r, k);
    if(r > mid)  update_tree(rs(i), l, r, k);
    push_up(i);
}

inline long long query_tree(int i, int l, int r)
{
    if(tr[i].l >= l && tr[i].r <= r) return tr[i].sum;
    push_down(i);
    int mid = (tr[i].l+tr[i].r)>>1;
    long long res = 0;
    if(l <= mid) res += query_tree(ls(i), l, r);
    if(r > mid)  res += query_tree(rs(i), l, r);
    return res;
}
```
### ZKW线段树
---
### 树状数组
[单点修改区间查询](https://www.luogu.org/problemnew/show/P3374)
[区间修改单点查询](https://www.luogu.org/problemnew/show/P3368)
```cpp
struct BinaryIndexedTree
{
    // set your type
    typedef int T;
    T tr[MAXN];
    BinaryIndexedTree() { memset(tr, 0, sizeof tr); }
    inline void clear() { for (int i = 1; i <= n; ++i) tr[i] = 0; }
    inline void update(int x, T v) { for ( ; x <= n; x += x&-x) tr[x] += v; }
    inline void update(int x, int y, T v) { update(x, v); update(y+1, -v); }
    inline T query(int x) { T res = 0; for ( ; x; x -= x&-x) res += tr[x]; return res; }
    inline T query(int x, int y) { return query(y)-query(x-1); }
} BIT;
```
```cpp
// 单点修改 add(x, k);
// 区间修改 add(x, k); add(y+1, -k);
inline void add(int i, int k)
{
    for( ; i <= n; i += i & -i) tr[i] += k;
}
// 单点查询
inline int query(int x)
{
    int res = 0;
    for( ; x; x -= x & -x) res += tr[x];
    return res;
}
// 区间查询
inline int query(int x, int y)
{
    int resx = 0, resy = 0;
    for( ; y; y -= y & -y) resy += tr[y];
    for(--x; x; x -= x & -x) resx += tr[x];
    return resy - resx;
}
```
### [分块](http://hzwer.com/8053.html#comment-8306)
```cpp
struct FenKuai
{
    typedef long long T;
    int t; // 每组大小
    static const int NN = static_cast<int>(sqrt(N))+7;
    T a[N], sum[NN], add[NN];

    FenKuai()
    {
        memset(a, 0, sizeof a);
        memset(sum, 0, sizeof sum);
        memset(add, 0, sizeof add);
    }

    void init()
    {
        t = static_cast<int>(sqrt(n)+0.5);
        for (int i = 0; i < n; ++i) sum[i/t] += a[i];
    }

    void update(int x, T k) { a[x] += k; sum[x/t] += k; }
    void update(int x, int y, T k)
    {
        for ( ; x <= y && x%t; ++x) a[x] += k, sum[x/t] += k;
        for ( ; x+t-1 <= y; x += t) sum[x/t] += k*t, add[x/t] += k;
        for ( ; x <= y; ++x) a[x] += k, sum[x/t] += k;
    }

    T query(int x) { return a[x]+add[x/t]; }
    T query(int x, int y)
    {
        T res = 0;
        for ( ; x <= y && x%t; ++x) res += a[x]+add[x/t];
        for ( ; x+t-1 <= y; x += t) res += sum[x/t];
        for ( ; x <= y; ++x) res += a[x]+add[x/t];
        return res;
    }
} B;
```
```cpp
struct FenKuai
{
    typedef int T;
    int t; // 每组大小
    T a[N], b[N], add[N];

    FenKuai()
    {
        memset(a, 0, sizeof a);
        memset(b, 0, sizeof b);
        memset(add, 0, sizeof add);
    }

    void build(int x)
    {
        for (int i = x*t; i < min(x*t+t, n); ++i) b[i] = a[i];
        sort(b+x*t, b+min(x*t+t, n));
    }

    void init()
    {
        t = static_cast<int>(sqrt(n)+0.5);
        for (int i = 0; i*t < n; ++i) build(i);
    }

    void update(int x, int y, T c)
    {
        int i = x;
        for ( ; i <= y && i%t; ++i) a[i] += c;
        build(x/t);
        for ( ; i+t-1 <= y; i += t) add[i/t] += c;
        for ( ; i <= y; ++i) a[i] += c;
        build(y/t);
    }

    T query(int x, int y, long long c)
    {
        T res = 0; int i = x;
        for ( ; i <= y && i%t; ++i) res += (a[i]+add[i/t] < c*c);
        for ( ; i+t-1 <= y; i += t) res += lower_bound(b+i, b+i+t, c*c-add[i/t])-(b+i);
        for ( ; i <= y; ++i) res += (a[i]+add[i/t] < c*c);
        return res;
    }
} B;
```
```cpp
struct FenKuai
{
    typedef int T;
    int t, sz;
    static const int NN = static_cast<int>(sqrt(N))+7;
    T a[N];
    deque<int> q[NN];

    void init(int _n)
    {
        sz = _n;
        t = static_cast<int>(sqrt(sz*1.5)+0.5);
        for (int i = 0; i < sz; ++i) q[i/t].push_back(a[i]);
    }

    void update(int x, int k)
    {
        stack<int> tmp;
        for (int i = 0; i != x%t; ++i) {
            tmp.push(q[x/t].front());
            q[x/t].pop_front();
        }
        q[x/t].push_front(k);
        while (tmp.size()) {
            q[x/t].push_front(tmp.top());
            tmp.pop();
        }
        ++sz;
        if (sz/t == x/t) return;
        for (int i = x/t, val; i < sz/t; ++i) {
            val = q[i].back();
            q[i].pop_back();
            q[i+1].push_front(val);
        }
    }

    T query(int x)
    {
        stack<int> tmp;
        for (int i = 0; i != x%t; ++i) {
            tmp.push(q[x/t].front());
            q[x/t].pop_front();
        }
        int res = q[x/t].front();
        while (tmp.size()) {
            q[x/t].push_front(tmp.top());
            tmp.pop();
        }
        return res;
    }
} B;
```
---
## 矩阵
**矩阵乘法**
```cpp
struct Match
{
    long long m[Maxn][Maxn];
    Match(){ memset(m, 0, sizeof m); }
    inline void init() { for(int i = 0; i < Maxn; ++i) m[i][i] = 1; }
    Match operator * (const Match &b) const
    {
        Match res;
        for(int i = 1; i <= n; ++i)
            for(int j = 1; j <= n; ++j)
            {
                long long &cur = res.m[i][j];
                for(int k = 1; k <= n; ++k)
                    cur = (cur+m[i][k]*b.m[k][j])%MOD;
            }
        return res;
    }
}
```
**[矩阵快速幂](https://www.luogu.org/problemnew/show/P3390)** (略)

---
## [快速幂](https://www.luogu.org/problemnew/show/P1226)
```cpp
inline long long qpow(long long a, long long p, long long mo)
{
    if(p == 0) return 1 % mo;
    long long ans = 1;
    a %= mo;
    while(p)
    {
        if(p&1) ans = ans*a%mo;
        a = a*a%mo;
        p >>= 1;
    }
    return ans;
}
```
---
## 快速乘
```cpp
inline long long qmul(long long x, long long y, long long mo)
{
    long long res = 0;
    while (y) {
        if (y&1) res = (res+x)%mo;
        x = (x<<1)%mo;
        y >>= 1;
    }
    return res;
}

inline long long qmul(long long x, long long y, long long mo)
{
    return (long long)((__int128)x*y%mo);
}

inline long long qmul(long long x, long long y, long long mo)
{
    // x*y - floor(x*y/mo)*mo
    typedef unsigned long long ull;
    typedef long double ld;
    return ((ull)x*y-(ull)((ld)x/mo*y)*mo+mo)%mo;
}
```
---
## [最小生成树](https://www.luogu.org/problemnew/show/P3366)
**[Prim](https://www.luogu.org/problemnew/show/P1265)**
```cpp
inline void prim()
{
    fill(dis, dis+n+1, INF);
    dis[1] = 0;
    for(int t = 1; t <= n; ++t)
    {
        int mini = 0;
        for(int i = 1; i <= n; ++i)
            if(!vis[i] && dis[i] < dis[mini])
                mini = i;
        vis[mini] = 1;
        ans += dis[mini];
        for(int i = 1; i <= n; ++i)
            if(!vis[i]) dis[i] = min(dis[i], calc(mini, i));
    }
}
```
**Kruskra** (略)

---
## [二分图匹配](https://www.luogu.org/problemnew/show/P3386)
**匈牙利算法**
```cpp
// vector<int> _e[Maxn];
bool check(int cur)
{
    for(unsigned i = 0, len = _e[cur].size(); i < len; ++i)
    {
        int nex = _e[cur][i];
        if(vis[nex]) continue;
        vis[nex] = true;
        if(!co[nex] || check(co[nex]))
        {
            co[nex] = cur;
            return true;
        }
    }
    return false;
}

int solve()
{
    int ans = 0;
    for(int i = 1; i <= n; ++i)
    {
        memset(vis, 0, sizeof vis);
        if(check(i)) ++ans;
    }
    return ans;
}
```
---
## [LCA](https://www.luogu.org/problemnew/show/P3379)

```cpp
void build_tree(int cur, int fa)
{
    d[cur] = d[fa]+1;
    f[cur][0] = fa;
    for(int i = 1; (1<<i) <= d[cur]; ++i)
        f[cur][i] = f[f[cur][i-1]][i-1];
    for(int k = fir[cur]; k; k = nex[k])
    {
        if(e[k] == fa) continue;
        build_tree(e[k], cur);
    }
}

int lca(int x, int y)
{
    if(d[x] < d[y]) swap(x, y);
    while(d[x] > d[y])
        x = f[x][ lg2[ d[x]-d[y] ]-1 ];
    if(x == y) return x;
    for(int i = lg2[d[x]]; i >= 0; --i)
        if(f[x][i] != f[y][i])
        {
            x = f[x][i];
            y = f[y][i];
        }
    return f[x][0];
}
```
---
## [网络流](https://www.luogu.org/problemnew/show/P3376)
**增广路**
```cpp
// while(bfs()) update();
bool bfs()
{
    memset(v, 0, sizeof v);
    queue<int> q;
    q.push(s);
    v[s] = true;
    incf[s] = INF;
    while(q.size())
    {
        int cur = q.front();
        q.pop();
        for(int i = fir[cur], to; i; i = nex[i])
        {
            to = ver[i];
            if(!edge[i] || v[to]) continue;
            incf[to] = min(incf[cur], edge[i]);
            pre[to] = i;
            if(to == t) return true;
            q.push(to);
            v[to] = true;
        }
    }
    return false;
}

void update()
{
    int cur = t, e;
    while(cur != s)
    {
        e = pre[cur];
        edge[e] -= incf[t];
        edge[e^1] += incf[t];
        cur = ver[e^1];
    }
    maxflow += incf[t];
}
```
**Dinic**
```cpp
main()
{
    int flow = 0;
    while(bfs())
        while((flow = dinic(s, INF)))
            maxflow += flow;
}

bool bfs()
{
    memset(d, 0, sizeof d);
    queue<int> q;
    q.push(s);
    d[s] = 1;
    int cur;
    while(q.size())
    {
        cur = q.front(); q.pop();
        for(int i = fir[cur], to; i; i = nex[i])
        {
            to = ver[i];
            if(d[to] || !edge[i]) continue;
            d[to] = d[cur]+1;
            if(to == t) return true;
            q.push(to);
        }
    }
    return false;
}

int dinic(int cur, int flow)
{
    if(cur == t) return flow;
    int rest = flow;
    for(int i = fir[cur], to, now; i; i = nex[i])
    {
        to = ver[i];
        if(d[to] != d[cur]+1 || !edge[i]) continue;
        now = dinic(to, min(rest, edge[i]));
        if(!now) d[to] = 0;
        else
        {
            edge[i] -= now;
            edge[i^1] += now;
            rest -= now;
        }
    }
    return flow - rest;
}
```
---
## 最短路
[弱化](https://www.luogu.org/problemnew/show/P3371)
[标准](https://www.luogu.org/problemnew/show/P4779)
### Floyd
略
### Dijiskra
邻接表+堆优化
```cpp
inline void Dijiska()
{
    priority_queue<pair<int,int>,vector<pair<int,int> >,greater<pair<int,int> > >q;
    memset(dis, 0x7f, sizeof dis);
    dis[S] = 0;
    q.push(make_pair(0, S));
    pair<int, int> cur;
    while(q.size())
    {
        cur = q.top(); q.pop();
        if(dis[cur.second] < cur.first) continue;
        for(int i = fir[cur.second], to, now; i; i = nex[i])
        {
            to = ver[i];
            now = cur.first+w[i];
            if(now >= dis[to]) continue;
            dis[to] = now;
            q.push(make_pair(now, to));
        }
    }
}
```
### SPFA
```cpp
inline void SPFA()
{
    fill(dis+1, dis+n+1, INT_MAX);
    dis[S] = 0;
    head = tail = 0;
    q[++tail] = S;
    while(head < tail)
    {
        int cur = q[++head];
        for(int i = fir[cur], to, tmp; i; i = nex[i])
        {
            to = ver[i];
            tmp = dis[cur]+w[i];
            if(tmp >= dis[to]) continue;
            dis[to] = tmp;
            q[++tail] = to;
        }
    }
}
```
---
## [负环](https://www.luogu.org/problemnew/show/P3385)
```cpp
// 返回true有负环,返回false没负环
inline bool SPFA()
{
    q[++tail] = 1;
    vis[1] = 1;
    cnt[1] = 1;
    dis[1] = 0;

    while(head < tail)
    {
        int cur = q[(++head)%Maxn];
        vis[cur] = 0;
        for(int i = fir[cur], to; i; i = nex[i])
        {
            to = ver[i];
            if(dis[cur]+w[i] < dis[to])
            {
                dis[to] = dis[cur]+w[i];
                if(!vis[to])
                {
                    q[(++tail)%Maxn] = to;
                    vis[to] = 1;
                    if(++cnt[to] > n) return true;
                }
            }
        }
    }
    return false;
}
```
---
## [ST表](https://www.luogu.org/problemnew/show/P3865)
```cpp
// for(int i = 2; i <= n; ++i) log_2[i] = log_2[i>>1]+1;

// st[i][j] --> [i-2^j+1, i]
inline void init_ST(int *a, int st[][20])
{
    for(int i = 1; i <= n; ++i)
    {
        st[i][0] = a[i];
        for(int j = 1; j <= log_2[i]; ++j)
            st[i][j] = max(st[i][j-1], st[i-(1<<(j-1))][j-1]);
    }
}

inline int query(int l, int r, int st[][20])
{
    int k = log_2[r-l+1];
    return max(st[r][k], st[l+(1<<k)-1][k]);
}

// st[i][j] --> [i, i+2^j-1]
void init()
{
    for(int j = 1; j <= log_2[n]; ++j)
        for(int i = 1; 1<<j <= n-i+1; ++i) // i + 1<<j - 1 <= n
            st[i][j] = max(st[i][j-1], st[i+(1<<(j-1))][j-1]);
}

int query(int l, int r)
{
    int k = log_2[r-l+1];
    return max(st[l][k], st[r-(1<<k)+1][k]);
}
```
---
## [割点](https://www.luogu.org/problemnew/show/P3388)
```cpp
void tarjan(int cur, int fa)
{
    dfn[cur] = low[cur] = ++_dfn;
    int child = 0;
    for(auto i : e[cur])
    {
        if(!dfn[i])
        {
            child++;
            tarjan(i, fa);
            low[cur] = min(low[cur], low[i]);
            if(cur != fa && low[i] >= dfn[cur]) flag[cur] = 1;
        }
        low[cur] = min(low[cur], dfn[i]);
    }
    if(cur == fa && child >= 2) flag[cur] = 1;
}
```
---
## [缩点](https://www.luogu.org/problemnew/show/P3387)
```cpp
void tarjan(int u)
{
    dfn[u] = low[u] = ++_dfn;
    vis[u] = 1;
    sta[++top] = u;
    for (int v : e[u]) {
        if (!dfn[v]) {
            tarjan(v);
            low[u] = min(low[u], low[v]);
        } else if (vis[v]) {
            low[u] = min(low[u], low[v]);
        }
    }
    if (dfn[u] == low[u]) {
        w_col[++_col] = 0;
        do {
            col[sta[top]] = _col;
            vis[sta[top]] = 0;
            w_col[_col] += w[sta[top]];
        } while (sta[top--] != u);
    }
}

inline void suodian()
{
    for (int i = 1; i <= n; ++i) {
        if (!dfn[i]) tarjan(i);
    }
    for (int i = 1; i <= n; ++i) {
        for (int j : e[i]) {
            if (col[i] == col[j]) continue;
            // if (mp.count({col[i], col[j]})) continue;
            // mp.insert({{col[i], col[j]}, 1});
            e_col[col[i]].push_back(col[j]);
        }
    }
}
```
---
## [并查集](https://www.luogu.org/problemnew/show/P3367)
```cpp
struct DSU
{
    int fa[N];
    void init(int sz) { for (int i = 0; i <= sz; ++i) fa[i] = i; }
    int get(int s) { return s == fa[s] ? s : fa[s] = get(fa[s]); }
    int& operator [] (int i) { return fa[get(i)]; }
    bool connect(int x, int y) {
        int fx = get(x), fy = get(y);
        if (fx == fy) return false;
        fa[fx] = fy; return true;
    }
} dsu;
```
---
## 二分
```cpp
int l, r, mid;
while(l < r)
{
    mid = (l+r+1)>>1;
    if(check(mid)) l = mid;
    r = mid-1;
}
while(l < r)
{
    mid = (l+r)>>1;
    if(check(mid)) l = mid+1;
    r = mid;
}
```
---
## 欧几里得
### 最大公因数 gcd
```cpp
__gcd(a, b); // <algorithm>
int gcd(int a, int b) { return b ? gcd(b, a%b) : a; }
inline int gcd(int a, int b) { while (b) a %= b, swap(a, b); return a; }
```
### 最小公倍数 lcm
```cpp
inline int lcm(int a, int b) { return a*b/gcd(a, b); }
```
### 拓展欧几里得([同余方程](https://www.luogu.org/problemnew/show/P1082))
```cpp
void exgcd(int a, int b, int &x, int &y)
{
    if(!b) { x = 1; y = 0; return; }
    exgcd(b, a%b, y, x);
    y -= a/b*x;
}
```
---
## [乘法逆元](https://www.luogu.org/problemnew/show/P3811)
### 拓展欧几里得
```cpp
inline int mul_inverse(int a, int mo)
{
    int x, y;
    exgcd(a, mo, x, y);
    return (x%mo+mo)%mo;
}
```
### 费马小定理
```cpp
inline int mul_inverse(int a, int mo)
{
    // a^(mo-2)%mo
    return qpow(a, mo-2, mo);
}
```
### 线性递推
```cpp
inline void mul_inverse(int *inv, int mo)
{
    inv[0] = 0; inv[1] = 1;
    for(int i = 2; i <= n; ++i)
        inv[i] = mo-mo/i*inv[mo%i]%mo;
}
```
---
## [中国剩余定理](https://blog.csdn.net/niiick/article/details/80229217)
### [中国剩余定理CRT(m互质)](https://www.luogu.org/problem/P3868)
```cpp
inline long long CRT(int a[], int m[])
{
    long long res = 0, M = 1;
    for (int i = 1; i <= n; ++i)
        M *= m[i];
    for (int i = 1; i <= n; ++i)
        res = (res + a[i]*(M/m[i])*mul_inverse(M/m[i], m[i]))%M;
    return (res+M)%M;
}
```
### [扩展中国剩余定理EXCRT(m不互质)](https://www.luogu.org/problem/P4777)
```cpp
inline long long EXCRT(long long a[], long long m[])
{
    // M*x + m[i]*y = a[i]-res (mod m[i])
    // res = res+x*M;
    long long M = m[1], res = a[1], x, y, c, d;
    for (int i = 2; i <= n; ++i) {
        d = exgcd(M, m[i], x, y);
        c = (a[i]-res%m[i]+m[i])%m[i];
        if (c%d != 0) return -1;
        x = (c/d)*x%(m[i]/d);
        res += x*M;
        M *= m[i]/d;
        res = (res%M+M)%M;
    }
    return res;
}

```
---
## [线性筛](https://www.luogu.org/problemnew/show/P3383)
```cpp
inline void init()
{
    check[1] = true;
    for(int i = 2; i <= n; ++i)
    {
        if(!check[i]) prime[++cnt] = i;
        for(int j = 1; j <= cnt && i*prime[j] <= n; ++j)
        {
            check[ i*prime[j] ] = true;
            if(i % prime[j] == 0) break;
        }
    }
}
```
---
## 判断素数(质数)
 某较优方法
```cpp
inline bool is_prime(long long x)
{
    if(x == 1) return false;
    if(x == 2 || x == 3) return true;
    if(x%6 != 1 && x%6 != 5) return false;
    for(long long i = 5; i*i <= x; i += 6)
        if(x%i == 0 || x%(i+2) == 0) return false;
    return true;
}
```
---
## [分解质因数](https://www.luogu.org/problemnew/show/P1075)
```cpp
// x = pi^ki...
for(int i = 2; i*i <= x; ++i)
    if(x%i == 0) {
        p[++tot] = i;
        for(; x%i == 0; x /= i) k[tot]++;
    }
if(x > 1) p[++tot] = x, k[tot] = 1;
```
---
## BSGS
**[luogu 4884](https://www.luogu.org/problemnew/show/P4884)**
```cpp
// map<long long, int> mmp;
inline long long BSGS(long long a, long long x, long long m) // a^n = x
{
    long long t = (long long)ceil(sqrt(m)); // b = a^i
    for(int i = 0; i < t; ++i)
        mmp[mul(x, qpow(a, i))] = i;
    a = qpow(a, t);
    long long now, ans; // now = (a^t)^i
    for(int i = 0; i <= t; ++i)
    {
        now = qpow(a, i);
        if(mmp.count(now))
        {
            ans = t*i-mmp[now];
            if(ans > 0) return ans;
        }
    }
    return -1;
}
```
---
## 背包问题
### 01背包
### 完全背包
### 混合背包
### [分组背包](https://www.luogu.org/problemnew/show/P1757)
### [多重背包](https://www.luogu.org/problemnew/show/P1776)
**二进制拆分**
```cpp
for(int i = 1, cnt, vi, wi, m; i <= n; ++i)
{
    scanf("%d%d%d", &vi, &wi, &m);
    cnt = 1;
    while(m-cnt > 0)
    {
        m -= cnt;
        v.push_back(vi*cnt);
        w.push_back(wi*cnt);
        cnt <<= 1;
    }
    v.push_back(vi*m);
    w.push_back(wi*m);
}
for(int i = 0; i < w.size(); ++i)
    for(int j = W; j >= w[i]; --j)
        b[j] = max(b[j], b[j-w[i]]+v[i]);
```
**单调队列**
```cpp
for(int i = 1; i <= n; ++i)
{
    scanf("%d%d%d", &v, &w, &m);
    for(int u = 0; u < w; ++u)
    {
        int maxp = (W-u)/w;
        head = 1; tail = 0;
        for(int k = maxp-1; k >= max(0, maxp-m); --k)
        {
            while(head <= tail && calc(u, q[tail]) <= calc(u, k)) tail--;
            q[++tail] = k;
        }
        for(int p = maxp; p >= 0; --p)
        {
            while(head <= tail && q[head] >= p) head++;
            if(head <= tail) f[u+p*w] = max(f[u+p*w], p*v+calc(u, q[head]));
            if(p-m-1 < 0) continue;
            while(head <= tail && calc(u, q[tail]) <= calc(u, p-m-1)) tail--;
            q[++tail] = p-m-1;
        }
    }
}
int ans = 0;
for(int i = 1; i <= W; ++i)
    ans = max(ans, f[i]);
```
---
## DP
(我**全**都不会)
### 记忆化搜索
### 线性DP
#### [最长上升子序列LIS](http://codevs.cn/problem/1576/)
```cpp
for(int i = 1; i <= n; ++i)
{
    f[i] = 1;
    for(int j = 1; j < i; ++j)
        if(a[i] > a[j]) f[i] = max(f[i],f[j]+1);
}
```
#### 最长公共子序列LCS
```cpp
f[i][j] = max{  f[i-1][j],
                f[i][j-1],
                f[i-1][j-1]+1 (if A[i] == B[j])}
```
#### 数字三角形
### 区间DP
### [树形DP](https://www.luogu.org/problemnew/show/P1352)
### 状压DP
### 队列优化
### 斜率优化
---
## STL
### 数据结构
```cpp
// const N
// typename T
vector<T>
stack<T>
deque<T>
queue<T>
priority_queue<T>
set<T>
bitset<N>
map<T, T>
```
### 函数
```cpp
// vector<T> a;
sort(a.begin(), a.end(), cmp);
reverse(a.begin(), a.end());
unique(a.begin(), a.end());
next_permutation(a.begin(), a.end());
lower_bound(a.begin(), a.end(), val); // >=
upper_bound(a.begin(), a.end(), val); // >
fill(a.begin(), a.end(), val);
memset(a, 0, sizeof a);
memcpy(a, b, sizeof b); // b --> a
// string s
s.find(target_string, start_pos); // 找不到返回s.npos
s.substr(start_pos, len);
s.replace(start_pos, len, target_string);
```
### unordered_map 重载
```cpp
struct Node
{
    int a, b;
    // 重载 ==
    friend bool operator == (const Node &x, const Node &y) {
        return x.a == y.a && x.b == y.b;
    }
};

// 方法一
namespace std {
    template <>
    struct hash<Node>
    {
        size_t operator () (const Node &x) const {
            return x.a+x.b;
        }
    };
}
unordered_map<Node, int> mp;

// 方法二
struct KeyHasher
{
    size_t operator () (const Node &x) const {
        return x.a+x.b;
    }
};
unordered_map<Node, int, KeyHasher> mmp;
```
---
## 模数
```cpp
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
```
---
## 高精度
[压位+vector+符号 版本](https://github.com/KaizynX/Oier/blob/master/BigInteger/BigInteger.cpp)
[一本通习题](http://ybt.ssoier.cn:8088/)
[洛谷习题](https://www.luogu.org/problemnew/lists?name=a%2Bb)

此版本 压位+数组,支持cin,cout,string,long long转换,比较运算符,四则运算(包括高精度乘/除低精度,取模),支持带符号的减法运算,支持幂运算,开根运算

可以通过开根外所有习题
```cpp
struct BigInteger
{
    static const int SIZE = 1e4; // 位数SIZE*4
    static const int BASE = 1e4; // 压位
    static const int WIDTH = 4;

    int v[SIZE], len;
    int tag; // 假装有正负符号

    BigInteger(long long num = 0) { *this = num; }
    BigInteger(const string &str) { *this = str; }
    // long long 转 BigInteger
    BigInteger operator = (long long num)
    {
        len = tag = 0;
        memset(v, 0, sizeof v);
        do
        {
            v[++len] = (int)(num%BASE);
            num /= BASE;
        } while(num > 0);
        return *this;
    }
    // string 转 BigInteger
    BigInteger operator = (const string &str)
    {
        string buf;
        int r = (int)str.length()-1, l = max(0, r-WIDTH+1);
        len = tag = 0;
        memset(v, 0, sizeof v);
        while(r >= 0)
        {
            buf = str.substr(l, r-l+1);
            sscanf(buf.c_str(), "%d", &v[++len]);
            r -= WIDTH; l = max(0, r-WIDTH+1);
        }
        return *this;
    }
    // 比较运算
    bool operator < (const BigInteger &b) const
    {
        if(len != b.len) return len < b.len;
        for(int i = len; i; --i)
            if(v[i] != b.v[i]) return v[i] < b.v[i];
        return false;
    }

    bool operator > (const BigInteger &b) const { return b < *this; }
    bool operator <= (const BigInteger &b) const { return !(b < *this); }
    bool operator >= (const BigInteger &b) const { return !(*this < b); }
    bool operator != (const BigInteger &b) const { return *this < b || b < *this; }
    bool operator == (const BigInteger &b) const { return !(*this < b) && !(b < *this); }
    // 四则运算
    BigInteger operator + (const BigInteger &b) const
    {
        BigInteger res = b;
        res.len = max(len, b.len);
        for(int i = 1; i <= len; ++i)
            res.v[i] += v[i];
        for(int i = 1; i <= res.len; ++i)
            res.v[i+1] += res.v[i]/BASE,
            res.v[i] %= BASE;
        while(res.v[res.len+1] > 0) res.len++;
        return res;
    }
    // 单目运算
    BigInteger operator + () const { return *this; }
    BigInteger operator - () const
    {
        BigInteger res = *this;
        res.tag ^= 1;
        return res;
    }

    BigInteger operator - (const BigInteger &b) const
    {
        if(*this < b) return -(b-*this);
        BigInteger res = *this;
        for(int i = 1; i <= b.len; ++i)
            res.v[i] -= b.v[i];
        for(int i = 1; i <= res.len; ++i)
            if(res.v[i] < 0)
                res.v[i] += BASE,
                res.v[i+1]--;
        while(res.len > 1 && res.v[res.len] == 0) res.len--;
        return res;
    }
    // 高精度乘低精度
    BigInteger operator * (int b) const
    {
        BigInteger res;
        long long tmp;
        res.len = len;
        for(int i = 1; i <= len; ++i)
        {
            tmp = 1ll*b*v[i];
            res.v[i] += (int)(tmp%BASE);
            res.v[i+1] += (int)(tmp/BASE+res.v[i]/BASE);
            res.v[i] %= BASE;
        }
        while(res.v[res.len+1] > 0) res.len++;
        return res;
    }
    // 高精度乘高精度
    BigInteger operator * (const BigInteger &b) const
    {
        BigInteger res;
        res.len = len+b.len;
        for(int i = 1; i <= len; ++i)
            for(int j = 1; j <= b.len; ++j)
            {
                res.v[i+j-1] += v[i]*b.v[j];
                res.v[i+j] += res.v[i+j-1]/BASE;
                res.v[i+j-1] %= BASE;
            }
        while(res.len > 1 && res.v[res.len] == 0) res.len--;
        return res;
    }
    // 高精度除低精度
    BigInteger operator / (int b) const
    {
        long long divisor = 0;
        BigInteger res;
        for(int i = len; i; --i)
        {
            divisor = divisor*BASE+v[i];
            if(divisor < b) continue;
            res.v[i] = (int)(divisor/b);
            divisor %= b;
            res.len = max(res.len, i);
        }
        return res;
    }
    // 高精度除高精度
    BigInteger operator / (const BigInteger &b) const
    {
        BigInteger divisor, res;
        int l, r, mid;
        for(int i = len; i; --i)
        {
            divisor = divisor*BASE+v[i];
            /*
            memcpy(divisor.v+1, divisor.v, sizeof(int)*(divisor.len+1));
            while(divisor.v[divisor.len+1] > 0) divisor.len++;
            divisor.v[1] = v[i];
            */
            if(divisor < b) continue;
            l = 0; r = BASE-1;
            while(l < r)
            {
                mid = (l+r+1)>>1;
                if(b*mid <= divisor) l = mid;
                else r = mid-1;
            }
            divisor -= b*l;
            res.v[i] = l;
            res.len = max(res.len, i);
        }
        return res;
    }

    BigInteger operator % (const BigInteger &b) const { return *this-*this/b*b; }

    BigInteger operator ++ () { return *this = *this+1; }
    BigInteger operator -- () { return *this = *this-1; }
    BigInteger operator += (const BigInteger &b) { return *this = *this+b; }
    BigInteger operator -= (const BigInteger &b) { return *this = *this-b; }
    BigInteger operator *= (const BigInteger &b) { return *this = *this*b; }
    BigInteger operator /= (const BigInteger &b) { return *this = *this/b; }
    BigInteger operator %= (const BigInteger &b) { return *this = *this%b; }
    BigInteger operator *= (int b) { return *this = *this*b; }
    BigInteger operator /= (int b) { return *this = *this/b; }
    BigInteger operator %= (int b) { return *this = *this%b; }
};

// 重载输入运算符
istream &operator >> (istream &in, BigInteger &big)
{
    string buf;
    if(in >> buf) big = buf;
    return in;
}
// 重载输出运算符
ostream &operator << (ostream &os, const BigInteger &big)
{
    char buf[10];
    if(big.tag) os << '-';
    os << big.v[big.len];
    for(int i = big.len-1; i; --i)
    {
        sprintf(buf, "%04d", big.v[i]);
        for(int j = 0; j < 4; ++j) os << buf[j];
    }
    return os;
}
// 幂
template <typename T>
BigInteger pow (BigInteger a, T p)
{
    if(p == 0) return 1;
    BigInteger res = 1;
    while(p)
    {
        if(p%2) res *= a;
        a *= a;
        p /= 2;
    }
    return res;
}
// 开根
BigInteger sqrt(const BigInteger &a, const int p = 2)
{
    BigInteger l, r = a, mid;
    while(l < r)
    {
        mid = (l+r+1)/2;
        if(pow(mid, p) <= a) l = mid;
        else r = mid-1;
    }
    return l;
}
```
---