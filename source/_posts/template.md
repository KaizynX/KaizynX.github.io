---
title: ACM模板
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2018-11-05 07:00:00
categories:
  - ACM
tags:
top: true
keywords: ACM模板
description:  某菜鸡收集的板子
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/template/cover.jpg
---

# 前言
```cpp
while (true) ++++++ ++++++ ++++++ RP;
```
<!--more-->
---
# 杂项
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
ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
```
[巨佬模板](https://atcoder.jp/contests/hitachi2020/submissions/10676875)
```cpp
struct Scanner {
    FILE* fp = nullptr;
    char line[(1 << 15) + 1];
    size_t st = 0, ed = 0;
    void reread() {
        memmove(line, line + st, ed - st);
        ed -= st;
        st = 0;
        ed += fread(line + ed, 1, (1 << 15) - ed, fp);
        line[ed] = '\0';
    }
    bool succ() {
        while (true) {
            if (st == ed) {
                reread();
                if (st == ed) return false;
            }
            while (st != ed && isspace(line[st])) st++;
            if (st != ed) break;
        }
        if (ed - st <= 50) reread();
        return true;
    }
    template <class T, enable_if_t<is_same<T, string>::value, int> = 0>
    bool read_single(T& ref) {
        if (!succ()) return false;
        while (true) {
            size_t sz = 0;
            while (st + sz < ed && !isspace(line[st + sz])) sz++;
            ref.append(line + st, sz);
            st += sz;
            if (!sz || st != ed) break;
            reread();
        }
        return true;
    }
    template <class T, enable_if_t<is_integral<T>::value, int> = 0>
    bool read_single(T& ref) {
        if (!succ()) return false;
        bool neg = false;
        if (line[st] == '-') {
            neg = true;
            st++;
        }
        ref = T(0);
        while (isdigit(line[st])) {
            ref = 10 * ref + (line[st++] - '0');
        }
        if (neg) ref = -ref;
        return true;
    }
    template <class T> bool read_single(V<T>& ref) {
        for (auto& d : ref) {
            if (!read_single(d)) return false;
        }
        return true;
    }
    void read() {}
    template <class H, class... T> void read(H& h, T&... t) {
        bool f = read_single(h);
        assert(f);
        read(t...);
    }
    Scanner(FILE* _fp) : fp(_fp) {}
};
 
struct Printer {
  public:
    template <bool F = false> void write() {}
    template <bool F = false, class H, class... T>
    void write(const H& h, const T&... t) {
        if (F) write_single(' ');
        write_single(h);
        write<true>(t...);
    }
    template <class... T> void writeln(const T&... t) {
        write(t...);
        write_single('\n');
    }
 
    Printer(FILE* _fp) : fp(_fp) {}
    ~Printer() { flush(); }
 
  private:
    static constexpr size_t SIZE = 1 << 15;
    FILE* fp;
    char line[SIZE], small[50];
    size_t pos = 0;
    void flush() {
        fwrite(line, 1, pos, fp);
        pos = 0;
    }
    void write_single(const char& val) {
        if (pos == SIZE) flush();
        line[pos++] = val;
    }
    template <class T, enable_if_t<is_integral<T>::value, int> = 0>
    void write_single(T val) {
        if (pos > (1 << 15) - 50) flush();
        if (val == 0) {
            write_single('0');
            return;
        }
        if (val < 0) {
            write_single('-');
            val = -val; // todo min
        }
        size_t len = 0;
        while (val) {
            small[len++] = char('0' + (val % 10));
            val /= 10;
        }
        for (size_t i = 0; i < len; i++) {
            line[pos + i] = small[len - 1 - i];
        }
        pos += len;
    }
    void write_single(const string& s) {
        for (char c : s) write_single(c);
    }
    void write_single(const char* s) {
        size_t len = strlen(s);
        for (size_t i = 0; i < len; i++) write_single(s[i]);
    }
    template <class T> void write_single(const V<T>& val) {
        auto n = val.size();
        for (size_t i = 0; i < n; i++) {
            if (i) write_single(' ');
            write_single(val[i]);
        }
    }
};
 
Scanner sc = Scanner(stdin);
Printer pr = Printer(stdout);
```
---
## 正则表达式
```cpp
char str[];
scanf("%3s", str); // 读取长度为n的字符串
scanf("%[abc]", str); // 读取a,b,c,读到之外的立即停止
scanf("%[a-z0-9]", str); // 同上,读取小写字母和数字
scanf("%*[a-z]%s", str); // 过滤掉小写字母读取
scanf("%[^a-z]", str); // 读取小写字符外字符,^表示非 
```
## 随机数
```cpp
#include <random>
// 范围 unsigned int
mt19937 rnd(time(NULL));
mt19937 rnd(chrono::high_resolution_clock::now().time_since_epoch().count());
cout << rnd() << endl;
```
```cpp
std::random_device rd;  //获取随机数种子
std::mt19937 gen(rd()); //Standard mersenne_twister_engine seeded with rd()
std::uniform_int_distribution<> dis(0, 9);
std::cout << dis(gen) << endl;
```
---
## 计算log2
```cpp
// lg2[i] = lg2(i) +1
for(int i = 1; i <= n; ++i) lg2[i] = lg2[i>>1]+1;
// lg2[i] = (int)log2(i)
for(int i = 1; i <= n; ++i) lg2[i] = lg2[i>>1]+1;
```
## 快速开根号|牛顿迭代法
```cpp
double sqrt(const double &a)
{
    double x = a, y = .0;
    while (abs(x-y) > err) {
        y = x;
        x = .5*(x+a/x);
    }
    return x;
}
```
---
# 计算几何
## 向量 坐标 直线 圆 (结构体)
```cpp
struct Point
{
    typedef double T;
    T x, y;
    int id;
    Point(){}
    Point(const T &_x, const T &_y, const int &_i = 0) : x(_x), y(_y), id(_i) {}
    friend Point operator + (const Point &p1, const Point &p2) {
        return Point(p1.x+p2.x, p1.y+p2.y, p1.id);
    }
    friend Point operator - (const Point &p1, const Point &p2) {
        return Point(p1.x-p2.x, p1.y-p2.y, p1.id);
    }
    friend Point operator - (const Point &p) {
        return Point(-p.x, -p.y, p.id);
    }
    friend T operator * (const Point &p1, const Point &p2) {
        return p1.x*p2.y-p1.y*p2.x;
    }
    template <typename TT>
    friend Point operator / (const Point &p, const TT &k) {
        return Point(p.x/k, p.y/k, p.id);
    }
    template <typename TT>
    friend Point operator * (const Point &p, const TT &k) {
        return Point(p.x*k, p.y*k, p.id);
    }
    Point operator += (const Point &p) { return *this = *this+p; }
    Point operator -= (const Point &p) { return *this = *this+p; }
    template <typename TT>
    Point operator *= (const TT &k) { return *this = *this*k; }
    template <typename TT>
    Point operator /= (const TT &k) { return *this = *this/k; }
    friend bool operator < (const Point &p1, const Point &p2) {
        return make_pair(p1.x, p1.y) < make_pair(p2.x, p2.y);
    }
    friend bool operator > (const Point &p1, const Point &p2) {
        return make_pair(p1.x, p1.y) > make_pair(p2.x, p2.y);
    }
    friend bool operator == (const Point &p1, const Point &p2) {
        return p1.x == p2.x && p1.y == p2.y;
    }
    friend bool operator != (const Point &p1, const Point &p2) {
        return p1.x != p2.x || p1.y != p2.y;
    }
    friend istream& operator >> (istream &is, Point &p) {
        return is >> p.x >> p.y;
    }
    friend ostream& operator << (ostream &os, Point &p) {
        return os << p.x << " " << p.y << " " << p.id << endl;
    }
    double length() { return sqrt(1.0*x*x+1.0*y*y); }
    friend double dis(const Point &p1, const Point &p2) { return (p2-p1).length(); }
    double dis(const Point &p) { return (p-*this).length(); }
    friend T dot(const Point &p1, const Point &p2) { return p1.x*p2.x+p1.y*p2.y; }
    T dot(const Point &p) { return x*p.x+y*p.y; }
    friend Point rotate_90_c(const Point &p) { return Point(p.y, -p.x, p.id); }
    Point rotate_90_c() { return Point(y, -x, id); }
    friend double atan(const Point &p) { return atan2(p.y, p.x); }
};

inline bool polar_angle1(const Point &p1, const Point &p2)
{
    double d1 = atan(p1), d2 = atan(p2); 
    return d1 == d2 ? p1 < p2 : d1 < d2;
}

inline bool polar_angle2(const Point &p1, const Point &p2)
{
    auto tmp = p1*p2;
    return tmp == 0 ? p1 < p2 : tmp > 0;
}

inline long long S(const Point &p1, const Point &p2, const Point &p3)
{
    return abs(p1.x*p2.y+p1.y*p3.x+p2.x*p3.y-p1.x*p3.y-p1.y*p2.x-p2.y*p3.x);
}

struct Line
{
    Point p1, p2;
    Line(){}
    Line(const Point &_p1, const Point &_p2) : p1(_p1), p2(_p2) {}
    friend bool cross(const Line &l1, const Line &l2) {
        #define SJ1(x) max(l1.p1.x, l1.p2.x) < min(l2.p1.x, l2.p2.x) || \
                       max(l2.p1.x, l2.p2.x) < min(l1.p1.x, l1.p2.x)
        if (SJ1(x) || SJ1(y)) return false;
        #undef SJ1
        #define SJ2(a, b, c, d) ((a-b)*(a-c))*((a-b)*(a-d)) <= 0
        return SJ2(l1.p1, l1.p2, l2.p1, l2.p2) &&
               SJ2(l2.p1, l2.p2, l1.p1, l1.p2);
        #undef SJ2
    }
    friend bool on_line(const Line &l, const Point &p) {
        return abs((l.p1-l.p2)*(l.p1-p)) < err;
    }
    friend Point cross_point(const Line &l1, const Line &l2) {
        Point v1 = l1.p2-l1.p1, v2 = l2.p2-l2.p1;
        if (abs(v1*v2) < err) return Point(0, 0); // no cross_point
        double t = (l2.p1-l1.p1)*v2/(v1*v2);
        return l1.p1+v1*t;
    }
};

struct Circular
{
    Point o;
    double r;
    Circular(){}
    Circular(const Point &_o, const double &_r) : o(_o), r(_r) {}
    template <typename T>
    Circular(const T &_x, const T &_y, const double &_r) : o(Point(_x, _y)), r(_r) {}
    friend bool in_cir(const Circular &c, const Point &p) { return dis(c.o, p) <= c.r; }
    bool in_cir(const Point &p) { return dis(o, p) <= r; }
};

inline Circular get_cir(const Point &p1, const Point &p2, const Point &p3)
{
    Circular res;
    res.o = cross_point(Line((p1+p2)/2, (p1+p2)/2+(p2-p1).rotate_90_c()),
                        Line((p1+p3)/2, (p1+p3)/2+(p3-p1).rotate_90_c()));
    res.r = dis(res.o, p1);
    return res;
}
```
---
## [二维凸包](https://www.luogu.com.cn/problem/P2742)
```cpp
int n;
int stk[N], used[N], tp;
Point p[N];

inline void Andrew()
{
    memset(used, 0, sizeof used);
    sort(p+1, p+n+1);
    tp = 0;
    stk[++tp] = 1;
    for (int i = 2; i <= n; ++i) {
        while (tp >= 2 && (p[stk[tp]]-p[stk[tp-1]])*(p[i]-p[stk[tp]]) <= 0)
            used[stk[tp--]] = 0;
        used[i] = 1;
        stk[++tp] = i;
    }
    int tmp = tp;
    for (int i = n-1; i; --i) {
        if (used[i]) continue;
        while (tp >= tmp && (p[stk[tp]]-p[stk[tp-1]])*(p[i]-p[stk[tp]]) <= 0)
            used[stk[tp--]] = 0;
        used[i] = 1;
        stk[++tp] = i;
    }
}
```
---
## [平面最近点对](https://www.luogu.com.cn/problem/P1429)
```cpp
Point a[N];
int n, ansa, ansb;
double mindist;

inline bool cmp_y(const Point &p1, const Point &p2) { return p1.y < p2.y; }

void upd_ans(const Point &p1, const Point &p2)
{
    double dist = dis(p1, p2);
    if (dist < mindist) mindist = dist, ansa = p1.id, ansb = p2.id;
}

void rec(int l, int r)
{
    if (r-l <= 3) {
        for (int i = l; i < r; ++i)
            for (int j = i+1; j <= r; ++j)
                upd_ans(a[i], a[j]);
        sort(a+l, a+r+1, cmp_y);
        return;
    }

    static Point t[N];
    int m = (l+r)>>1, midx = a[m].x;
    rec(l, m); rec(m+1, r);
    merge(a+l, a+m+1, a+m+1, a+r+1, t, cmp_y);
    copy(t, t+r-l+1, a+l);

    int tsz = 0;
    for (int i = l; i <= r; ++i)
        if (abs(a[i].x-midx) <= mindist) {
            for (int j = tsz; j && a[i].y-t[j].y < mindist; --j)
                upd_ans(a[i], t[j]);
            t[++tsz] = a[i];
        }
}

inline void mindist_pair()
{
    sort(a+1, a+n+1);
    mindist = INF;
    rec(1, n);
}
```
---
## [最小圆覆盖|随即增量法](https://www.luogu.com.cn/problem/P1742)
```cpp
inline Circular RIA()
{
    Circular cir;
    random_shuffle(a+1, a+n+1);
    for (int i = 1; i <= n; ++i) {
        if (cir.in_cir(a[i])) continue;
        cir = Circular(a[i], 0);
        for (int j = 1; j < i; ++j) {
            if (cir.in_cir(a[j])) continue;
            cir = Circular((a[i]+a[j])/2, dis(a[i], a[j])/2);
            for (int k = 1; k < j; ++k) {
                if (cir.in_cir(a[k])) continue;
                cir = get_cir(a[i], a[j], a[k]);
            }
        }
    }
    return cir;
}
```
---
# 数据结构
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
## 字典树
```cpp
struct TireTree
{
    static const int NN = 5e5+7;
    static const int SZ = 26;
    char beg;
    int nex[NN][SZ], num[NN], cnt;
    bool exist[NN];
    TireTree(char _beg = 'a') : beg(_beg) { clear(); }
    void clear() {
        memset(nex, 0, sizeof nex);
        memset(num, 0, sizeof num);
        memset(exist, 0, sizeof exist);
        cnt = 0;
    }
    void insert(const char *s) {
        int len = strlen(s), p = 0;
        for (int i = 0, c; i < len; ++i) {
            c = s[i]-beg;
            if (!nex[p][c]) nex[p][c] = ++cnt;
            p = nex[p][c];
            ++num[p];
        }
        exist[p] = true;
    }
    bool find(const char *s) {
        int len = strlen(s), p = 0;
        for (int i = 0, c; i < len; ++i) {
            c = s[i]-beg;
            if (!nex[p][c]) return false;
            p = nex[p][c];
        }
        return exist[p];
    }
    int count(const char *s) {
        int len = strlen(s), p = 0;
        for (int i = 0, c; i < len; ++i) {
            c = s[i]-beg;
            if (!nex[p][c]) return 0;
            p = nex[p][c];
        }
        return num[p];
    }
    void insert(const string &s) { insert(s.c_str()); }
    bool find(const string &s) { return find(s.c_str()); }
    int count(const string &s) { return count(s.c_str()); }
};
```
## 二叉查找树
## [平衡树](https://www.luogu.org/problemnew/show/P3369)
### [Splay](https://www.luogu.org/blog/user19027/solution-p3369)
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
## [线段树](https://www.luogu.org/problemnew/show/P3372)
### 区间加减区间和
```cpp
template <typename T>
struct SegmentTree
{
    int sz;
    T tr[N<<2], lazy[N<<2];
    SegmentTree(){}
    void build(const int &n, const T &k = 0) { sz = n; _build(1, n, k); }
    template <typename TT>
    void build(const TT a[], const int &n) { sz = n; _build(a, 1, n); }
    void modify(const int &x, const T &k) { _modify(x, k, 1, sz); }
    void add(const int &x, const T &k) { _add(x, x, k, 1, sz); }
    void add(int l, int r, const T &k) { if (l > r) swap(l, r); _add(l, r, k, 1, sz); }
    T query(const int &x) { return _query(x, x, 1, sz); }
    T query(int l, int r) { if (l > r) swap(l, r); return _query(l, r, 1, sz); }
private :
    void push_up(const int &i) { tr[i] = tr[i<<1]+tr[i<<1|1]; }
    void push_down(const int &i, const int &len) {
        if (!lazy[i]) return;
        tr[i<<1] += lazy[i]*(len-len/2);
        tr[i<<1|1] += lazy[i]*(len/2);
        lazy[i<<1] += lazy[i];
        lazy[i<<1|1] += lazy[i];
        lazy[i] = 0;
    }
    void _build(const int &l, const int &r, const T &k = 0, const int &i = 1) {
        lazy[i] = 0;
        if (l == r) { tr[i] = k; return; }
        int mid = (l+r)>>1;
        _build(l, mid, i<<1);
        _build(mid+1, r, i<<1|1);
        push_up(i);
    }
    template <typename TT>
    void _build(const TT a[], const int &l, const int &r, const int &i = 1) {
        lazy[i] = 0;
        if (l == r) { tr[i] = a[l]; return; }
        int mid = (l+r)>>1;
        _build(a, l, mid, i<<1);
        _build(a, mid+1, r, i<<1|1);
        push_up(i);
    }
    void _modify(const int &x, const T &k, const int &trl, const int &trr, const int &i = 1) {
        if (trl == x && trr == x) {
            tr[i] = k;
            lazy[i] = 0;
            return;
        }
        push_down(i, trr-trl+1);
        int mid = (trl+trr)>>1;
        if (x <= mid) _modify(x, k, trl, mid, i<<1);
        else _modify(x, k, mid+1, trr, i<<1|1);
        push_up(i);
    }
    void _add(const int &l, const int &r, const T &k, const int &trl, const int &trr, const int &i = 1) {
        if (trl >= l && trr <= r) {
            tr[i] += k*(trr-trl+1);
            lazy[i] += k;
            return;
        }
        push_down(i, trr-trl+1);
        int mid = (trl+trr)>>1;
        if (l <= mid) _add(l, r, k, trl, mid, i<<1);
        if (r >  mid) _add(l, r, k, mid+1, trr, i<<1|1);
        push_up(i);
    }
    T _query(const int &l, const int &r, const int &trl, const int &trr, const int &i = 1) {
        if (trl >= l && trr <= r) return tr[i];
        push_down(i, trr-trl+1);
        int mid = (trl+trr)>>1;
        T res = 0;
        if (l <= mid) res += _query(l, r, trl, mid, i<<1);
        if (r >  mid) res += _query(l, r, mid+1, trr, i<<1|1);
        return res;
    }
};
```
### 区间修改区间和
```cpp
template <typename T>
struct SegmentTree
{
    int sz;
    int tag[N<<2];
    T tr[N<<2], lazy[N<<2];
    SegmentTree(){}
    void build(const int &n, const T &k = 0) { sz = n; _build(1, n, k); }
    template <typename TT>
    void build(const TT a[], const int &n) { sz = n; _build(a, 1, n); }
    void update(const int &x, const T &k) { _update(x, x, k, 1, sz); }
    void update(int l, int r, const T &k) { if (l > r) swap(l, r); _update(l, r, k, 1, sz); }
    T query(const int &x) { return _query(x, x, 1, sz); }
    T query(int l, int r) { if (l > r) swap(l, r); return _query(l, r, 1, sz); }
private :
    void push_up(const int &i) { tr[i] = tr[i<<1]+tr[i<<1|1]; }
    void push_down(const int &i, const int &len) {
        if (!tag[i]) return;
        tr[i<<1] = lazy[i]*(len-len/2);
        tr[i<<1|1] = lazy[i]*(len/2);
        lazy[i<<1] = lazy[i<<1|1] = lazy[i];
        tag[i<<1] = tag[i<<1|1] = 1;
        tag[i] = 0;
    }
    void _build(const int &l, const int &r, const T &k = 0, const int &i = 1) {
        lazy[i] = tag[i] = 0;
        if (l == r) { tr[i] = k; return; }
        int mid = (l+r)>>1;
        _build(l, mid, k, i<<1);
        _build(mid+1, r, k, i<<1|1);
        push_up(i);
    }
    template <typename TT>
    void _build(const TT a[], const int &l, const int &r, const int &i = 1) {
        lazy[i] = tag[i] = 0;
        if (l == r) { tr[i] = a[l]; return; }
        int mid = (l+r)>>1;
        _build(a, l, mid, i<<1);
        _build(a, mid+1, r, i<<1|1);
        push_up(i);
    }
    void _update(const int &l, const int &r, const T &k, const int &trl, const int &trr, const int &i = 1) {
        if (trl >= l && trr <= r) {
            tr[i] = k*(trr-trl+1);
            lazy[i] = k;
            tag[i] = 1;
            return;
        }
        push_down(i, trr-trl+1);
        int mid = (trl+trr)>>1;
        if (l <= mid) _update(l, r, k, trl, mid, i<<1);
        if (r >  mid) _update(l, r, k, mid+1, trr, i<<1|1);
        push_up(i);
    }
    T _query(const int &l, const int &r, const int &trl, const int &trr, const int &i = 1) {
        if (trl >= l && trr <= r) return tr[i];
        push_down(i, trr-trl+1);
        int mid = (trl+trr)>>1;
        T res = 0;
        if (l <= mid) res += _query(l, r, trl, mid, i<<1);
        if (r >  mid) res += _query(l, r, mid+1, trr, i<<1|1);
        return res;
    }
};
```
### 区间加减区间最值
```cpp
template <typename T, typename U = greater<T>>
struct SegmentTree
{
    U cmp = U();
    int n;
    T tr[N<<2], lazy[N<<2], init_val = cmp(0, 1) ? INF : -INF;
    SegmentTree(){}
    T mv(const T &x, const T &y) { return cmp(x, y) ? x : y;}
    void build(const int &_n, const T &k = 0) { n = _n; _build(1, n, k); }
    template <typename TT>
    void build(const TT a[], const int &_n) { n = _n; _build(a, 1, n); }
    void modify(const int &x, const T &k) { _modify(x, k, 1, n); }
    void add(const int &x, const T &k) { _add(x, x, k, 1, n); }
    void add(const int &l, const int &r, const T &k) { _add(l, r, k, 1, n); }
    T query(const int &x) { return _query(x, x, 1, n); }
    T query(const int &l, const int &r) { return _query(l, r, 1, n); }
private :
    void push_up(const int &i) { tr[i] = mv(tr[i<<1], tr[i<<1|1]); }
    void push_down(const int &i) {
        if (!lazy[i]) return;
        tr[i<<1] += lazy[i];
        tr[i<<1|1] += lazy[i];
        lazy[i<<1] += lazy[i];
        lazy[i<<1|1] += lazy[i];
        lazy[i] = 0;
    }
    void _build(const int &l, const int &r, const T &k = 0, const int &i = 1) {
        lazy[i] = 0;
        if (l == r) { tr[i] = k; return; }
        int mid = (l+r)>>1;
        _build(l, mid, k, i<<1);
        _build(mid+1, r, k, i<<1|1);
        push_up(i);
    }
    template <typename TT>
    void _build(const TT a[], const int &l, const int &r, const int &i = 1) {
        lazy[i] = 0;
        if (l == r) { tr[i] = a[l]; return; }
        int mid = (l+r)>>1;
        _build(a, l, mid, i<<1);
        _build(a, mid+1, r, i<<1|1);
        push_up(i);
    }
    void _modify(const int &x, const T &k, const int &trl, const int &trr, const int &i = 1) {
        if (trl == x && trr == x) {
            tr[i] = k;
            return;
        }
        push_down(i);
        int mid = (trl+trr)>>1;
        if (x <= mid) _modify(x, k, trl, mid, i<<1);
        else _modify(x, k, mid+1, trr, i<<1|1);
        push_up(i);
    }
    void _add(const int &l, const int &r, const T &k, const int &trl, const int &trr, const int &i = 1) {
        if (trl >= l && trr <= r) {
            tr[i] += k;
            lazy[i] += k;
            return;
        }
        push_down(i);
        int mid = (trl+trr)>>1;
        if (l <= mid) _add(l, r, k, trl, mid, i<<1);
        if (r >  mid) _add(l, r, k, mid+1, trr, i<<1|1);
        push_up(i);
    }
    T _query(const int &l, const int &r, const int &trl, const int &trr, const int &i = 1) {
        if (trl >= l && trr <= r) return tr[i];
        push_down(i);
        int mid = (trl+trr)>>1;
        T res = init_val;
        if (l <= mid) res = mv(res, _query(l, r, trl, mid, i<<1));
        if (r >  mid) res = mv(res, _query(l, r, mid+1, trr, i<<1|1));
        return res;
    }
};
```
### 区间更新最值
```cpp
template <typename T, typename U = greater<T>>
struct SegmentTree
{
    SegmentTree(){}
    void build(const int &_n, const T &k = 0) { n = _n; _build(1, n, k); }
    template <typename TT>
    void buiil(const int _n, const TT a[]) { n = _n; _build(1, n, a); }
    void modify(const int &x, const T &k) { _modify(x, k); }
    void modify(const int &l, const int &r, const T &k) { _modify(l, r, k); }
    void add(const int &x, const T &k) { _add(x, k); }
    T query(const int &x) { return _query(x, x); }
    T query(const int &l, const int &r) { return _query(l, r); }
private:
    struct TreeNode
    {
        int l, r;
        T v, lazy;
    } tr[N<<2];
    int n;
    T init_val = cmp(0, 1) ? INF : -INF;
    U cmp = U();
    T mv(const T &x, const T &y) { return cmp(x, y) ? x : y;}
    void update(const int &i, const T &k) { tr[i].v = mv(tr[i].v, k); tr[i].lazy = mv(tr[i].lazy, k); }
    void push_up(const int &i) { tr[i].v = mv(tr[i<<1].v, tr[i<<1|1].v); }
    void push_down(const int &i) {
        if (tr[i].lazy == init_val) return;
        update(i<<1, tr[i].lazy);
        update(i<<1|1, tr[i].lazy);
        tr[i].lazy = init_val;
    }
    void _build(const int &l, const int &r, const T &k = 0, const int &i = 1) {
        tr[i].lazy = init_val;
        tr[i].l = l; tr[i].r = r;
        if (l == r) { tr[i].v = k; return; }
        int mid = (l+r)>>1;
        _build(l, mid, k, i<<1);
        _build(mid+1, r, k, i<<1|1);
        push_up(i);
    }
    template <typename TT>
    void _build(const int &l, const int &r, const TT a[], const int &i = 1) {
        tr[i].lazy = init_val;
        tr[i].l = l; tr[i].r = r;
        if (l == r) { tr[i].v = a[l]; return; }
        int mid = (l+r)>>1;
        _build(l, mid, a, i<<1);
        _build(mid+1, r, a, i<<1|1);
        push_up(i);
    }
    void _modify(const int &l, const int &r, const T &k, const int &i = 1) {
        if (tr[i].l  >= l && tr[i].r <= r) { update(i, k); return; }
        push_down(i);
        int mid = (tr[i].l+tr[i].r)>>1;
        if (l <= mid) _modify(l, r, k, i<<1);
        if (r >  mid) _modify(l, r, k, i<<1|1);
        push_up(i);
    }
    void _add(const int &x, const T &k, const int &i = 1) {
        if (tr[i].l == x && tr[i].r == x) { tr[i].v += k; return; }
        push_down(i);
        int mid = (tr[i].l+tr[i].r)>>1;
        if (x <= mid) _add(x, k, i<<1);
        else _add(x, k, i<<1|1);
        push_up(i);
    }
    T _query(const int &l, const int &r, const int &i = 1) {
        if (tr[i].l  >= l && tr[i].r <= r) return tr[i].v;
        push_down(i);
        int mid = (tr[i].l+tr[i].r)>>1;
        T res = init_val;
        if (l <= mid) res = mv(res, _query(l, r, i<<1));
        if (r >  mid) res = mv(res, _query(l, r, i<<1|1));
        return res;
    }
};
```
## ZKW线段树
`warning:区间最值尚为验证`
```cpp
template <typename T>
struct zkwSegmentTree
{
    int sz;
    T sum[N<<2], mn[N<<2], mx[N<<2], add[N<<2];
    void update(const int &x) {
        T tmp;
        tmp = min(mn[x], mn[x^1]); mn[x] -= tmp; mn[x^1] -= tmp; mn[x>>1] += tmp;
        tmp = max(mx[x], mx[x^1]); mx[x] -= tmp; mx[x^1] -= tmp; mx[x>>1] += tmp;
    }
    template <typename TT>
    void build(const TT a[], const int &n) {
        for (sz = 1; sz <= n+1; sz <<= 1);
        for (int i = sz+1; i <= sz+n; ++i)
            sum[i] = mn[i] = mx[i] = a[i-sz];
        for (int i = sz-1; i; --i) {
            sum[i] = sum[i<<1]+sum[i<<1|1];
            mn[i] = min(mn[i<<1], mn[i<<1|1]); mn[i<<1] -= mn[i]; mn[i<<1|1] -= mn[i];
            mx[i] = max(mx[i<<1], mx[i<<1|1]); mx[i<<1] -= mx[i]; mx[i<<1|1] -= mx[i];
        }
    }
    void update(int x, const T &v) {
        x += sz; mx[x] += v; mn[x] += v; sum[x] += v;
        for ( ; x > 1; x >>= 1) {
            sum[x] += v;
            update(x);
        }
    }
    void update(int s, int t, const T &v) {
        int lc = 0, rc = 0, len = 1;
        for (s += sz-1, t += sz+1; s^t^1; s >>= 1, t >>= 1, len <<= 1) {
            if (~s&1) add[s^1] += v, lc += len, mn[s^1] += v, mx[s^1] += v;
            if ( t&1) add[t^1] += v, rc += len, mn[t^1] += v, mx[t^1] += v;
            sum[s>>1] += v*lc; sum[t>>1] += v*rc;
            update(s); update(t);
        }
        for (lc += rc; s; s >>= 1) {
            sum[s>>1] += v*lc;
            update(s);
        }
    }
    T query(int x) {
        T res = 0;
        for (x += sz; x; x >>= 1) res += mn[x];
        return res;
    }
    T query_sum(int s, int t) {
        int lc = 0, rc = 0, len = 1;
        T res = 0;
        for (s += sz-1, t += sz+1; s^t^1; s >>= 1, t >>= 1, len <<= 1) {
            if (~s&1) res += sum[s^1]+len*add[s^1], lc += len;
            if ( t&1) res += sum[t^1]+len*add[t^1], rc += len;
            if (add[s>>1]) res += add[s>>1]*lc;
            if (add[t>>1]) res += add[t>>1]*rc;
        }
        for (lc += rc, s >>= 1; s; s >>= 1) if (add[s]) res += add[s]*lc;
        return res;
    }
    T query_min(int s, int t) {
        if (s == t) return query(s);
        T l = 0, r = 0, res = 0;
        for (s += sz, t += sz; s^t^1; s >>= 1, t >>= 1) {
            l += mn[s]; r += mn[t];
            if (~s^1) l = min(l, mn[s^1]);
            if ( t^1) r = min(r, mn[t^1]);
        }
        for (res = min(l, r), s >>= 1; s; s >>= 1) res += mn[s];
        return res;
    }
    T query_max(int s, int t) {
        if (s == t) return query(s);
        T l = 0, r = 0, res = 0;
        for (s += sz, t += sz; s^t^1; s >>= 1, t >>= 1) {
            l += mx[s]; r += mx[t];
            if (~s^1) l = max(l, mx[s^1]);
            if ( t^1) r = max(r, mx[t^1]);
        }
        for (res = max(l, r), s >>= 1; s; s >>= 1) res += mx[s];
        return res;
    }
};
```
---
## 树状数组
### 一维
[单点修改区间查询](https://www.luogu.org/problemnew/show/P3374)

[区间修改单点查询](https://www.luogu.org/problemnew/show/P3368)

```cpp
template <typename T>
struct BinaryIndexedTree
{
    int n;
    T tr[N];
    BinaryIndexedTree() { memset(tr, 0, sizeof tr); }
    void init(const int &_n) { n = _n; clear(); }
    void clear() { for (int i = 1; i <= n; ++i) tr[i] = 0; }
    void add(const int &x, const T &v) { for (int i = x ; i <= n; i += i&-i) tr[i] += v; }
    void add(const int &x, const int &y, const T &v) { add(x, v); add(y+1, -v); }
    T query(const int &x) { T res = 0; for (int i = x ; i; i -= i&-i) res += tr[i]; return res; }
    T query(const int &x, const int &y) { return query(y)-query(x-1); }
};
```

[O(n)初始化](http://codeforces.com/blog/entry/63064)
```cpp
template <typename TT>
void init(const int &_n, const TT a[]) {
    n = _n; clear();
    for (int i = 1; i <= n; ++i) {
        tr[i] += a[i];
        if (i+(i&-i) <= n) tr[i+(i&-i)] += tr[i];
    }
}
```
### 二维
#### 单点修改区间查询
```cpp
template <typename T>
struct BIT_2D
{
    int n, m;
    T a[N][N], tr[N][N];
    BIT_2D() { memset(tr, 0, sizeof tr); }
    void init(const int &_n, const int &_m) {
        n = _n; m = _m;
        memset(a, 0, sizeof a);
        memset(tr, 0, sizeof tr);
    }
    void add(const int &x, const int &y, const T &k) {
        a[x][y] += k;
        for (int i = x; i <= n; i += i&-i)
            for (int j = y; j <= m; j += j&-j)
                tr[i][j] += k;
    }
    T query(const int &x, const int &y) {
        return a[x][y];
        // return query(x, y, x, y);
    }
    T query(int r1, int c1, int r2, int c2) {
        if (r1 > r2) swap(r1, r2);
        if (c1 > c2) swap(c1, c2);
        return _query(r2, c2)-_query(r1-1, c2)-_query(r2, c1-1)+_query(r1-1, c1-1);
    }
    T _query(const int &x, const int &y) {
        T res = 0;
        for (int i = x; i; i -= i&-i)
            for (int j = y; j; j -= j&-j)
                res += tr[i][j];
        return res;
    }
};
```
## [可持久化线段树(主席树)](https://www.luogu.com.cn/problem/P3834)
```cpp
template <typename T>
struct PersistenceSegmentTree
{
    static const int NN = N*(log2(N)+3);
    int rt[N], sum[NN], ls[NN], rs[NN], tot, sz;
    vector<T> des;
    void build(const T a[], const int &n) {
        vector<T>(a+1, a+n+1).swap(des);
        sort(des.begin(), des.end());
        des.erase(unique(des.begin(), des.end()), des.end());
        sz = des.size();
        tot = 0;
        rt[0] = _build(1, sz);
        for (int i = 1; i <= n; ++i) {
            int t = lower_bound(des.begin(), des.end(), a[i])-des.begin()+1;
            rt[i] = _update(rt[i-1], 1, sz, t);
        }
    }
    void update(const int &id, const T &k) {
        int t = lower_bound(des.begin(), des.end(), k)-des.begin()+1;
        rt[id] = _update(rt[id-1], 1, sz, t);
    }
    T query(const int &l, const int &r, const int &k) {
        return des[_query(rt[l-1], rt[r], 1, sz, k)-1];
    }
private:
    int _build(const int &l, const int &r) {
        int cur = ++tot;
        sum[++cur] = 0;
        if (l >= r) return cur;
        int mid = (l+r)>>1;
        ls[cur] = _build(l, mid);
        rs[cur] = _build(mid+1, r);
        return cur;
    }
    int _update(const int &pre, const int &l, const int &r, const int &k) {
        int cur = ++tot;
        ls[cur] = ls[pre]; rs[cur] = rs[pre]; sum[cur] = sum[pre]+1;
        if (l >= r) return cur;
        int mid = (l+r)>>1;
        if (k <= mid) ls[cur] = _update(ls[pre], l, mid, k);
        else rs[cur] = _update(rs[pre], mid+1, r, k);
        return cur;
    }
    int _query(const int &u, const int &v, const int &l, const int &r, const int &k) {
        if (l >= r) return l;
        int num = sum[ls[v]]-sum[ls[u]], mid = (l+r)>>1;
        if (num >= k) return _query(ls[u], ls[v], l, mid, k);
        else return _query(rs[u], rs[v], mid+1, r, k-num);
    }
};
```
## [分块](http://hzwer.com/8053.html)
[例题](https://loj.ac/problems/search?keyword=%E5%88%86%E5%9D%97)
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
## [ST表](https://www.luogu.org/problemnew/show/P3865)
一维
```cpp
template <typename T, typename U = std::greater<T>>
struct ST
{
    static const int NN = (int)log2(N)+3;
    static const T INF = 1e9;
    U cmp = U();
    T rmq[N][NN]; // rmq[i][j] ==> [i-2^j+1, i]
    ST() { init(); }
    ST(const T &val) { init(val); }
    T& operator [] (const int &i) { return rmq[i][0]; }
    void init(){ fill(rmq[0], rmq[0]+N*NN, cmp(-INF, +INF) ? INF : -INF); }
    void init(const T &val) { fill(rmq[0], rmq[0]+N*NN, val); }
    T mv(const T &x, const T &y) { return cmp(x, y) ? x : y; }
    void build(T a[], const int &n) {
        for (int i = 1; i <= n; ++i) {
            rmq[i][0] = a[i];
            for (int j = 1; j <= log_2[i]; ++j)
                rmq[i][j] =  mv(rmq[i][j-1], rmq[i-(1<<(j-1))][j-1]);
        }
    }
    T query(const int &l, const int &r) {
        int k = log_2[r-l+1];
        return mv(rmq[r][k], rmq[l+(1<<k)-1][k]);
    }
};
ST<int, greater<int>> st;
```
二维
```cpp
template <typename T, typename U = std::greater<T>>
struct ST
{
    static const int NN = (int)log2(N)+3;
    static const T INF = 1e9;
    U cmp = U();
    T rmq[N][N][NN][NN]; // rmq[i][j][k][l] [i, j] [i+2^k-1, j+2^l-1]
    ST() { init(); }
    ST(const T &val) { init(val); }
    T& operator [] (const int &i) { return rmq[i][0]; }
    void init(){ fill(rmq[0][0][0], rmq[0][0][0]+N*N*NN*NN, cmp(-INF, +INF) ? INF : -INF); }
    void init(const T &val) { fill(rmq[0][0][0], rmq[0][0][0]+N*N*NN*NN, val); }
    T mv(const T &x, const T &y) { return cmp(x, y) ? x : y; }
    void build(T a[N][N], const int &n, const int &m) {
        for (int k = 0; k <= log_2[n]; ++k)
        for (int l = 0; l <= log_2[m]; ++l)
        for (int i = 1; i+(1<<k)-1 <= n; ++i)
        for (int j = 1; j+(1<<l)-1 <= m; ++j) {
            T &cur = rmq[i][j][k][l];
            if (!k && !l) cur = a[i][j];
            else if (!l) cur = mv(rmq[i][j][k-1][l], rmq[i+(1<<(k-1))][j][k-1][l]);
            else cur = mv(rmq[i][j][k][l-1], rmq[i][j+(1<<(l-1))][k][l-1]);
        }
    }
    T query(const int &r1, const int &c1, const int &r2, const int &c2) {
        int k = log_2[r2-r1+1], l = log_2[c2-c1+1];
        return mv(mv(rmq[r1][c1][k][l], rmq[r2-(1<<k)+1][c2-(1<<l)+1][k][l]),
                  mv(rmq[r2-(1<<k)+1][c1][k][l], rmq[r1][c2-(1<<l)+1][k][l]));
    }
};
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
    bool merge(int x, int y) {
        int fx = get(x), fy = get(y);
        if (fx == fy) return false;
        fa[fx] = fy; return true;
    }
} dsu;
```
加上按秩合并
```cpp
struct DSU
{
    int fa[N], num[N];
    void init(int sz) { for (int i = 0; i <= sz; ++i) fa[i] = i, num[i] = 1; }
    int get(int s) { return s == fa[s] ? s : fa[s] = get(fa[s]); }
    int& operator [] (int i) { return fa[get(i)]; }
    bool merge(int x, int y) {
        int fx = get(x), fy = get(y);
        if (fx == fy) return false;
        if (num[fx] >= num[fy]) num[fx] += num[fy], fa[fy] = fx;
        else num[fy] += num[fx], fa[fx] = fy;
        return true;
    }
} dsu;
```
---
# 字符串
## [回文字符串|manacher算法](https://www.luogu.org/problemnew/show/P3805)
从 0 开始，第 i 位对应 p[i*2+2]
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
### 判断s[l, r]是否为回文
```cpp
p[l+r+2]-1 >= r-l+1
```
---
## [KMP](https://www.luogu.org/problemnew/show/P3375)
`.c_str() 未知异常 Segment Fault`
```cpp
inline void get_next(const string &s, int nex[]) { get_next(s.c_str(), nex); }
inline void get_next(const char *s, int nex[])
{
    nex[0] = nex[1] = 0;
    for (int i = 1, j = 0, l = strlen(s); i < l; ++i) {
        while (j && s[i] != s[j]) j = nex[j];
        nex[i+1] = s[i] == s[j] ? ++j : 0;
    }
}

inline void kmp(const string &s1, const string &s2, int nex[]) { kmp(s1.c_str(), s2.c_str(), nex); }
inline void kmp(const char *s1, const char *s2, int nex[])
{
    for (int i = 0, j = 0, l1 = strlen(s1), l2 = strlen(s2); i < l1; ++i) {
        while (j && s1[i] != s2[j]) j = nex[j];
        if (s1[i] == s2[j]) ++j;
        if (j == l2) {
            cout << i-l2+2 << endl;
            j = nex[j];
        }
    }
}
```
```cpp
inline void get_next(const string &s, int nex[])
{
    nex[0] = nex[1] = 0;
    for (int i = 1, j = 0; i < (int)s.size(); ++i) {
        while (j && s[i] != s[j]) j = nex[j];
        nex[i+1] = s[i] == s[j] ? ++j : 0;
    }
}

inline void kmp(const string &s1, const string &s2, int nex[])
{
    for (int i = 0, j = 0; i < (int)s1.size(); ++i) {
        while (j && s1[i] != s2[j]) j = nex[j];
        if (s1[i] == s2[j]) ++j;
        if (j == (int)s2.size()) {
            cout << i-s2.size()+2 << endl;
            j = nex[j];
        }
    }
}
```
## [扩展KMP|Z函数](https://subetter.com/algorithm/extended-kmp-algorithm.html)
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
## 最长公共前后缀
[hdu2594](http://acm.hdu.edu.cn/showproblem.php?pid=2594)
### Hash
```cpp
string max_pre_suf(const string &s1, const string &s2)
{
    static const int M1 = 805306457, M2 = 1000173169, P = 31;
    static pii pre[N], suf[N], pwp[N];
    int l1 = s1.size(), l2 = s2.size();
    pre[0] = {0, 0};
    for (int i = 1; i <= l1; ++i) {
        pre[i] = {static_cast<int>((1ll*pre[i-1].first*P+s1[i-1]-'a')%M1),
                  static_cast<int>((1ll*pre[i-1].second*P+s1[i-1]-'a')%M2)};
    }
    pwp[0] = {1, 1};
    for (int i = 1; i <= l2; ++i) {
        pwp[i] = {static_cast<int>(1ll*pwp[i-1].first*P%M1),
                  static_cast<int>(1ll*pwp[i-1].second*P%M2)};
    }
    suf[l2+1] = {0, 0}; 
    for (int i = l2; i; --i) {
        suf[i] = {static_cast<int>((1ll*(s2[i-1]-'a')*pwp[l2-i].first+suf[i+1].first)%M1),
                  static_cast<int>((1ll*(s2[i-1]-'a')*pwp[l2-i].second+suf[i+1].second)%M2)};
    }
    for (int i = min(l1, l2); i; --i)
        if (pre[i] == suf[l2-i+1]) return s1.substr(0, i);
    return "";
}
```
---
## BM算法
## Sunday算法
## [字符串哈希](https://www.luogu.org/problemnew/show/P3370)
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
# 图论|树论
## [DFS树](https://codeforces.com/blog/entry/68138)
## [树的重心](https://www.luogu.org/problemnew/show/P2986)
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
## 最大团
最大独立集数=补图的最大团
```cpp
struct MaxClique
{
    vector<int> res, tmp, cnt;
    bool dfs(int p) {
        for (int i = p+1, flag; i <= n; ++i) {
            if (cnt[i]+tmp.size() <= res.size()) return false;
            if (!g[p][i]) continue;
            flag = 1;
            for (int j : tmp)
                if (!g[i][j]) flag = 0;
            if (!flag) continue;
            tmp.push_back(i);
            if (dfs(i)) return true;
            tmp.pop_back();
        }
        if (tmp.size() > res.size()) {
            res = tmp;
            return true;
        }
        return false;
    }
    void solve() {
        vector<int>(n+1, 0).swap(cnt);
        vector<int>().swap(res);
        for (int i = n; i; --i) {
            vector<int>(1, i).swap(tmp);
            dfs(i);
            cnt[i] = res.size();
        }
    }
} MC;
```
---
## 稳定婚姻匹配
```cpp
template <typename T = int> struct Stable_Marriage
{
    int t[N], b[N], g[N], rkb[N][N], rkg[N][N];
    T wb[N][N], wg[N][N];
    queue<int> q;
    void init(const int &n) {
        queue<int>().swap(q);
        memset(t, 0, sizeof(int)*(n+3));
        memset(b, 0, sizeof(int)*(n+3));
        memset(g, 0, sizeof(int)*(n+3));
        for (int i = 1; i <= n; ++i) {
            q.push(i);
            for (int j = 1; j <= n; ++j)
                rkb[i][j] = rkg[i][j] = j;
            sort(rkb[i]+1, rkb[i]+n+1,
                 [&](const int &x, const int &y) { return wb[i][y] < wb[i][x]; });
            //sort(rkg[i]+1, rkg[i]+n+1,
            //     [&](const int &x, const int &y) { return wg[i][y] < wg[i][x]; });
        }
    }
    bool match(const int &x, const int &y) {
        if (g[y]) {
            if (wg[y][x] < wg[y][g[y]]) return false;
            b[g[y]] = 0;
            q.push(g[y]);
        }
        b[x] = y; g[y] = x;
        return true;
    }
    void gale_shapely(const int &n) {
        init(n);
        while (q.size()) {
            int x = q.front(); q.pop();
            int y = rkb[x][++t[x]];
            if (!match(x, y)) q.push(x);
        }
    }
};
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
**Kruskal** (略)

---
## 二分图
### [二分图匹配](https://www.luogu.org/problemnew/show/P3386)
**匈牙利算法**
```cpp
bool check(int u)
{
    for (int v : e[u]) {
        if (vis[v]) continue;
        vis[v] = 1;
        if (!co[v] || check(co[v])) {
            co[v] = u;
            return true;
        }
    }
    return false;
}

inline int solve()
{
    int res = 0;
    memset(co, 0, sizeof co);
    for (int i = 1; i <= n; ++i) {
        memset(vis, 0, sizeof(int)*(n+3));
        res += check(i);
    }
    return res;
}
```
### 二分图最小顶点覆盖
定义：假如选了一个点就相当于覆盖了以它为端点的所有边。最小顶点覆盖就是选择最少的点来覆盖所有的边。

定理：最小顶点覆盖等于二分图的最大匹配。
### 最大独立集

定义：选出一些顶点使得这些顶点两两不相邻，则这些点构成的集合称为独立集。找出一个包含顶点数最多的独立集称为最大独立集。

定理：最大独立集 = 所有顶点数 - 最小顶点覆盖 = 所有顶点数 -   最大匹配

---
## [LCA](https://www.luogu.org/problemnew/show/P3379)
```cpp
struct LCA
{
    static const int NN = (int)log2(N)+3;
    int f[N][NN], d[N], lg2[N];
    LCA() { for (int i = 2; i < N; ++i) lg2[i] = lg2[i>>1]+1; }
    template <typename TT>
    void build(const TT e[], const int &u = 1, const int &fa = 0) {
        d[u] = d[fa]+1;
        f[u][0] = fa;
        for (int i = 1; (1<<i) <= d[u]; ++i)
            f[u][i] = f[f[u][i-1]][i-1];
        for (auto v : e[u]) if (v != fa)
            build(e, v, u);
    }
    int get(int x, int y) {
        if (d[x] < d[y]) swap(x, y);
        while (d[x] > d[y])
            x = f[x][lg2[d[x]-d[y]]];
        if (x == y) return x;
        for (int i = lg2[d[x]]; i >= 0; --i)
            if(f[x][i] != f[y][i])
                x = f[x][i], y = f[y][i];
        return f[x][0];
    }
};
```
[带权LCA](https://www.luogu.com.cn/problem/P1967)
```cpp
template <typename T>
struct LCA
{
    static const int NN = (int)log2(N)+3;
    int f[N][NN], d[N], lg2[N];
    T w[N][NN], init_val = 0;
    LCA() {
        for (int i = 2; i < N; ++i) lg2[i] = lg2[i>>1]+1;
        init();
    }
    // set sum or min or max, and don't forget to set init_val
    T update(const T &x, const T &y) { return x+y; }
    void init(const int &n = N-1) {
        fill(w[0], w[0]+(n+1)*NN, init_val);
    }
    template <typename TT>
    void build(const TT e[], const int &u = 1, const int &fa = 0) {
        d[u] = d[fa]+1;
        f[u][0] = fa;
        for (int i = 1; (1<<i) <= d[u]; ++i) {
            f[u][i] = f[f[u][i-1]][i-1];
            w[u][i] = update(w[u][i-1], w[f[u][i-1]][i-1]);
        }
        for (auto v : e[u]) if (v.first != fa) {
            w[v.first][0] = v.second;
            build(e, v.first, u);
        }
    }
    T get(int x, int y) {
        T res = init_val;
        if (d[x] < d[y]) swap(x, y);
        while (d[x] > d[y]) {
            res = update(res, w[x][lg2[d[x]-d[y]]]);
            x = f[x][lg2[d[x]-d[y]]];
        }
        if (x == y) return res;
        for (int i = lg2[d[x]]; i >= 0; --i)
            if(f[x][i] != f[y][i]) {
                res = update(res, w[x][i]);
                res = update(res, w[y][i]);
                x = f[x][i], y = f[y][i];
            }
        return update(res, update(w[x][0], w[y][0]));
    }
};
```
## 最小环 
---
## [树上差分](https://www.luogu.com.cn/problem/P3128)
```cpp
template <typename T>
struct Tree
{
    T val[N];
    void update_point(const int &x, const int &y, const T &k) {
        int _lca = lca(x, y);
        val[x] += k; val[y] += k;
        val[_lca] -= k; val[f[_lca][0]] -= k;
    }
    void update_edge(const int &x, const int &y, const T &k) {
        int _lca = lca(x, y);
        val[x] += k; val[y] += k; val[_lca] -= 2*k;
    }
    void dfs(const int &u = 1, const int &fa = 0) {
        for (int v : e[u]) if (v != fa) {
            dfs(v, u);
            val[u] += val[v];
        }
    }
};
```
## [树链剖分](https://www.luogu.com.cn/problem/P3384)
```cpp
template <typename T>
struct ShuPou
{
    int dfn;
    int f[N], d[N], num[N], son[N], rk[N], id[N], tp[N];
    T init_val[N];
    SegmentTree<T> ST;
    template <typename TT, typename EDGE>
    void build(const EDGE e[], const TT a[], const int &n, const int &rt = 1) {
        memset(son, 0, sizeof son);
        d[0] = num[0] = dfn = 0;
        dfs1(e, rt);
        dfs2(e, rt, rt);
        for (int i = 1; i <= n; ++i)
            init_val[i] = a[rk[i]];
        ST.build(init_val, n);
    }
    template <typename EDGE>
    void dfs1(const EDGE e[], const int &u = 1, const int &fa = 0) {
        f[u] = fa;
        d[u] = d[fa]+1;
        num[u] = 1;
        for (auto v : e[u]) if (v != fa) {
            dfs1(e, v, u);
            num[u] += num[v];
            if (num[v] > num[son[u]])
                son[u] = v;
        }
    }
    template <typename EDGE>
    void dfs2(const EDGE e[], const int &u = 1, const int &t = 1) {
        tp[u] = t;
        id[u] = ++dfn;
        rk[dfn] = u;
        if (!son[u]) return;
        dfs2(e, son[u], t);
        for (auto v : e[u]) if (v != son[u] && v != f[u])
            dfs2(e, v, v);
    }
    void add_sons(const int &x, const T &k) { ST.add(id[x], id[x]+num[x]-1, k); }
    void add(int x, int y, const T &k) {
        while (tp[x] != tp[y]) {
            if (d[tp[x]] < d[tp[y]]) swap(x, y);
            ST.add(id[tp[x]], id[x], k);
            x = f[tp[x]];
        }
        if (d[x] > d[y]) swap(x, y);
        ST.add(id[x], id[y], k);
    }
    T query_sons(const int &x) { return ST.query(id[x], id[x]+num[x]-1); }
    T query(int x, int y) {
        T res = 0;
        while (tp[x] != tp[y]) {
            if (d[tp[x]] < d[tp[y]]) swap(x, y);
            res += ST.query(id[tp[x]], id[x]);
            x = f[tp[x]];
        }
        if (d[x] > d[y]) swap(x, y);
        return res+ST.query(id[x], id[y]);
    }
};
```
---
## 网络流
[网络流24题](https://loj.ac/problems/tag/30)
### [最大流](https://www.luogu.org/problemnew/show/P3376)
#### EK
$O(nm^2)$
```cpp
template <typename T>
struct EK
{
    struct Edge
    {
        int v, nex;
        T w;
    } e[M<<1];
    int tot = 0, n;
    int fir[N], vis[N], pre[N];
    T incf[N];
    T work(const int &s, const int &t) {
        T res = 0;
        while (bfs(s, t)) {
            int u = t, id;
            while (u != s) {
                id = pre[u];
                e[id].w -= incf[t];
                e[id^1].w += incf[t];
                u = e[id^1].v;
            }
            res += incf[t];
        }
        return res;
    }
    void init(const int &sz) {
        n = sz;
        tot = 0;
        memset(fir, -1, sizeof(int)*(n+3));
    }
    void add_edge(const int &u, const int &v, const T &w) {
        e[tot] = {v, fir[u], w}; fir[u] = tot++;
        e[tot] = {u, fir[v], 0}; fir[v] = tot++;
    }
    bool bfs(const int &s, const int &t) {
        queue<int> q;
        memset(vis, 0, sizeof(int)*(n+3));
        q.push(s);
        vis[s] = 1;
        incf[s] = INF;
        while (q.size()) {
            int u = q.front();
            q.pop();
            for (int i = fir[u], v; i != -1; i = e[i].nex) {
                v = e[i].v;
                if (vis[v] || !e[i].w) continue;
                incf[v] = min(incf[u], e[i].w);
                pre[v] = i;
                if (v == t) return true;
                q.push(v);
                vis[v] = 1;
            }
        }
        return false;
    }
};
```
#### Dinic
普通情况下 $O(n^2m)$
二分图中 $O(\sqrt{n}m)$
```cpp
template <typename T>
struct Dinic
{
    struct EDGE
    {
        int v, nex;
        T w;
    } e[M<<1];
    int tot, n;
    int fir[N], dep[N];
    T work(const int &s, const int &t) {
        T maxflow = 0, flow;
        while (bfs(s, t))
            while ((flow = dfs(s, t, INF)))
                maxflow += flow;
        return maxflow;
    }
    void init(const int &sz) {
        n = sz;
        tot = 0;
        memset(fir, -1, sizeof(int)*(n+3));
    }
    void add_edge(const int &u, const int &v, const T &w) {
        e[tot] = {v, fir[u], w}; fir[u] = tot++;
        e[tot] = {u, fir[v], 0}; fir[v] = tot++;
    }
    bool bfs(const int &s, const int &t) {
        queue<int> q;
        memset(dep, 0, sizeof(int)*(n+3));
        q.push(s);
        dep[s] = 1;
        while (q.size()) {
            int u = q.front();
            q.pop();
            for (int i = fir[u], v; i != -1; i = e[i].nex) {
                v = e[i].v;
                if (dep[v] || !e[i].w) continue;
                dep[v] = dep[u]+1;
                if (v == t) return true;
                q.push(v);
            }
        }
        return false;
    }
    T dfs(const int &u, const int &t, const T &flow) {
        if (!flow || u == t) return flow;
        T rest = flow, now;
        for (int i = fir[u], v; i != -1; i = e[i].nex) {
            v = e[i].v;
            if (dep[v] != dep[u]+1 || !e[i].w) continue;
            now = dfs(v, t, min(rest, e[i].w));
            if (!now) {
                dep[v] = 0;
            } else {
                e[i].w -= now;
                e[i^1].w += now;
                rest -= now;
                if (rest == flow) break;
            }
        }
        return flow-rest;
    }
};
```
#### [ISAP](https://www.luogu.com.cn/blog/ONE-PIECE/jiu-ji-di-zui-tai-liu-suan-fa-isap-yu-hlpp)
渐进时间复杂度和dinic相同，但是非二分图的情况下isap更具优势
```cpp
template <typename T>
struct ISAP
{
    struct EDGE
    {
        int v, nex;
        T w;
    } e[M<<1];
    int tot, n, s, t;
    T maxflow;
    int fir[N], gap[N], dep[N];
    T work(const int &_s, const int &_t) {
        s = _s; t = _t;
        maxflow = 0;
        bfs();
        while (dep[s] < n) dfs(s, INF);
        return maxflow;
    }
    void init(const int &sz) {
        n = sz;
        tot = 0;
        memset(fir, -1, sizeof(int)*(n+3));
    }
    void add_edge(const int &u, const int &v, const T &w) {
        e[tot] = {v, fir[u], w}; fir[u] = tot++;
        e[tot] = {u, fir[v], 0}; fir[v] = tot++;
    }
    void bfs() {
        queue<int> q;
        memset(dep, -1, sizeof(int)*(n+3));
        memset(gap, 0, sizeof(int)*(n+3));
        dep[t] = 0;
        gap[0] = 1;
        q.push(t);
        while (q.size()) {
            int u = q.front();
            q.pop();
            for (int i = fir[u], v; i != -1; i = e[i].nex) {
                v = e[i].v;
                if (dep[v] != -1) continue;
                q.push(v);
                dep[v] = dep[u]+1;
                ++gap[dep[v]];
            }
        }
    }
    T dfs(const int &u, const T &flow) {
        if (u == t) {
            maxflow += flow;
            return flow;
        }
        T used = 0;
        for (int i = fir[u], v; i != -1; i = e[i].nex) {
            v = e[i].v;
            if (!e[i].w || dep[v]+1 != dep[u]) continue;
            T minf = dfs(v, min(e[i].w, flow-used));
            if (minf) {
                e[i].w -= minf;
                e[i^1].w += minf;
                used += minf;
            }
            if (used == flow) return used;
        }
        if (--gap[dep[u]] == 0) dep[s] = n+1;
        ++gap[++dep[u]];
        return used;
    }
};
```
vector存边版本
```cpp
template <typename T>
struct ISAP
{
    struct EDGE
    {
        int v, nex;
        T w;
        EDGE(const int &_v, const int &_nex, const T &_w) : v(_v), nex(_nex), w(_w) {}
    };
    vector<EDGE> e;
    int n, s, t;
    T maxflow;
    int fir[N], gap[N], dep[N];
    T work(const int &_s, const int &_t) {
        s = _s; t = _t;
        maxflow = 0;
        bfs();
        while (dep[s] < n) dfs(s, INF);
        return maxflow;
    }
    void init(const int &_n) {
        n = _n;
        e.clear();
        e.reserve(N<<2);
        memset(fir, -1, sizeof(int)*(n+3));
    }
    void add_edge(const int &u, const int &v, const T &w) {
        e.emplace_back(v, fir[u], w); fir[u] = e.size()-1;
        e.emplace_back(u, fir[v], 0); fir[v] = e.size()-1;
    }
    void bfs() {
        queue<int> q;
        memset(dep, -1, sizeof(int)*(n+3));
        memset(gap, 0, sizeof(int)*(n+3));
        dep[t] = 0;
        gap[0] = 1;
        q.push(t);
        while (q.size()) {
            int u = q.front();
            q.pop();
            for (int i = fir[u], v; i != -1; i = e[i].nex) {
                v = e[i].v;
                if (dep[v] != -1) continue;
                q.push(v);
                dep[v] = dep[u]+1;
                ++gap[dep[v]];
            }
        }
    }
    T dfs(const int &u, const T &flow) {
        if (u == t) {
            maxflow += flow;
            return flow;
        }
        T used = 0;
        for (int i = fir[u], v; i != -1; i = e[i].nex) {
            v = e[i].v;
            if (!e[i].w || dep[v]+1 != dep[u]) continue;
            T minf = dfs(v, min(e[i].w, flow-used));
            if (minf) {
                e[i].w -= minf;
                e[i^1].w += minf;
                used += minf;
            }
            if (used == flow) return used;
        }
        if (--gap[dep[u]] == 0) dep[s] = n+1;
        ++gap[++dep[u]];
        return used;
    }
};
```
#### HLPP
### 最小割
最小割等价最大流
### 费用流
最大流 dfs 改成最短路 
#### MCMF
大常数?
```cpp
template <typename T>
struct MCMF
{
    struct Edge
    {
        int v, nex;
        T w, c; // edge wight and cost
        Edge(const int &_v, const int &_nex, const T &_w, const T &_c) \
        : v(_v), nex(_nex), w(_w), c(_c) {}
    };
    vector<Edge> e;
    int n, s, t;
    int fir[N], vis[N], pre[N];
    T incf[N], dis[N];
    void init(const int &_n) {
        n = _n;
        e.clear();
        e.reserve(N<<4);
        memset(fir, -1, sizeof(int)*(n+3));
    }
    void add_edge(const int &u, const int &v, const T &w, const T &c) {
        e.emplace_back(v, fir[u], w, c); fir[u] = e.size()-1;
        e.emplace_back(u, fir[v], 0, -c); fir[v] = e.size()-1;
    }
    pair<T, T> work(const int &_s, const int &_t) {
        s = _s; t = _t;
        T maxflow = 0, mincost = 0;
        while (spfa()) {
            for (int u = t, id; u != s; u = e[id^1].v) {
                id = pre[u];
                e[id].w -= incf[t];
                e[id^1].w += incf[t];
                mincost += incf[t]*e[id].c;
            }
            maxflow += incf[t];
        }
        return {maxflow, mincost};
    }
    bool spfa() {
        queue<int> q;
        memset(dis, 0x3f, sizeof(T)*(n+3));
        memset(vis, 0, sizeof(int)*(n+3));
        q.push(s);
        dis[s] = 0;
        incf[s] = INF;
        incf[t] = 0;
        while (q.size()) {
            int u = q.front();
            q.pop();
            vis[u] = 0;
            for (int i = fir[u], v; i != -1; i = e[i].nex) {
                v = e[i].v;
                if (!e[i].w || dis[v] <= dis[u]+e[i].c) continue;
                dis[v] = dis[u]+e[i].c;
                incf[v] = min(incf[u], e[i].w);
                pre[v] = i;
                if (vis[v]) continue;
                q.push(v);
                vis[v] = 1;
            }
        }
        return incf[t];
    }
};
```
#### 类Dinic
```cpp
template <typename T>
struct Dinic
{
    struct Edge
    {
        int v, nex;
        T w, c; // edge wight and cost
        Edge(const int &_v, const int &_nex, const T &_w, const T &_c) \
        : v(_v), nex(_nex), w(_w), c(_c) {}
    };
    vector<Edge> e;
    int n, s, t;
    int fir[N], vis[N];
    T maxflow, mincost;
    T dis[N];
    void init(const int &_n) {
        n = _n;
        e.clear();
        e.reserve(N<<4);
        maxflow = mincost = 0;
        memset(vis, 0, sizeof(int)*(n+3));
        memset(fir, -1, sizeof(int)*(n+3));
    }
    void add_edge(const int &u, const int &v, const T &w, const T &c) {
        e.emplace_back(v, fir[u], w, c); fir[u] = e.size()-1;
        e.emplace_back(u, fir[v], 0, -c); fir[v] = e.size()-1;
    }
    pair<T, T> work(const int &_s, const int &_t) {
        s = _s; t = _t;
        T flow;
        while (spfa())
            while ((flow = dfs(s, INF)))
                maxflow += flow;
        return {maxflow, mincost};
    }
    bool spfa() {
        queue<int> q;
        memset(dis, 0x3f, sizeof(int)*(n+3));
        q.push(s);
        dis[s] = 0;
        vis[s] = 1;
        while (q.size()) {
            int u = q.front();
            q.pop();
            vis[u] = 0;
            for (int i = fir[u], v; i != -1; i = e[i].nex) {
                v = e[i].v;
                if (!e[i].w || dis[v] <= dis[u]+e[i].c) continue;
                dis[v] = dis[u]+e[i].c;
                if (vis[v]) continue;
                q.push(v);
                vis[v] = 1;
            }
        }
        return dis[t] != INF;
    }
    T dfs(const int &u, const T &flow) {
        if (!flow || u == t) return flow;
        T res = 0 , now;
        vis[u] = 1;
        for (int i = fir[u], v; i != -1 && res < flow; i = e[i].nex) {
            v = e[i].v;
            if (vis[v] || !e[i].w || dis[v] != dis[u]+e[i].c) continue;
            now = dfs(v, min(flow-res, e[i].w));
            if (!now) continue;
            mincost += now*e[i].c;
            e[i].w -= now;
            e[i^1].w += now;
            res += now;
        }
        vis[u] = 0;
        return res;
    }
};
```
#### Dijkstra
Primal-Dual 原始对偶算法
### 上下界网络流
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
# 数论
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
## 线性基
求最大值[Luogu3812](https://www.luogu.com.cn/problem/P3812)

求第k大数[HDOJ3949](http://acm.hdu.edu.cn/showproblem.php?pid=3949)
```cpp
template <typename T>
struct XXJ
{
    int sz = sizeof(T)*8, zero;
    T tot;
    vector<T> b, rb, p;
    XXJ(){ init(); }
    void init() {
        tot = zero = 0;;
        vector<T>(sz, 0).swap(b);
        vector<T>().swap(rb);
        vector<T>().swap(p);
    }
    template <typename TT>
    void build(TT a[], const int &n) {
        init();
        for (int i = 1; i <= n; ++i) insert(a[i]);
    }
    void merge(const XXJ xj) {
        for (int i : xj.b) if (i) insert(i);
    }
    void insert(T x) {
        for (int i = sz-1; i >= 0; --i) if ((x>>i)&1) {
            if (!b[i]) { b[i] = x; return; }
            x ^= b[i];
        }
        zero = 1;
    }
    bool find(T x) {
        for (int i = sz-1; i >= 0; --i) if ((x>>i)&1) {
            if (!b[i]) { return false; }
            x ^= b[i];
        }
        return true;
    }
    T max_xor() {
        T res = 0;
        for (int i = sz-1; i >= 0; --i)
            if (~(res>>i)&1) res ^= b[i];
            // res = max(res, res^b[i]);
        return res;
    }
    T min_xor() {
        if (zero) return 0;
        for (int i = 0; i < sz; ++i)
            if (b[i]) return b[i];
    }
    void rebuild() {
        rb = b;
        vector<T>().swap(p);
        for (int i = sz-1; i >= 0; --i)
            for (int j = i-1; j >= 0; --j)
                if ((rb[i]>>j)&1) rb[i] ^= rb[j];
        for (int i = 0; i < sz; ++i)
            if (rb[i]) p.emplace_back(rb[i]);
        tot = ((T)1<<p.size())+zero;
    }
    T kth_min(T k) {
        if (k >= tot || k < 1) return -1;
        if (zero && k == 1) return 0;
        if (zero) --k;
        T res = 0;
        for (int i = (int)p.size()-1; i >= 0; --i)
            if ((k>>i)&1) res ^= p[i];
        return res;
    }
    T kth_max(const T &k) {
        return kth_min(tot-k);
    }
};
```
---
## 矩阵
### [矩阵快速幂](https://www.luogu.org/problemnew/show/P3390)
### [矩阵求逆](https://www.luogu.com.cn/problem/P4783)
```cpp
template <typename T>
struct Martix {
    int n, m;
    T a[N][N];
    Martix(){}
    Martix(const int &_n) : n(_n), m(_n) { init(); }
    Martix(const int &_n, const int &_m) : n(_n), m(_m) { init(); }
    T* operator [] (const int &i) { return a[i]; }
    void init(const int &tag = 0) {
        for (int i = 1; i <= n; ++i) memset(a[i], 0, sizeof(T)*(n+1));
        for (int i = 1; i <= n; ++i) a[i][i] = tag;
    }
    friend Martix operator * (const Martix &m1, const Martix &m2) {
        Martix res(m1.n, m2.m);
        for (int i = 1; i <= res.n; ++i)
            for (int j = 1; j <= res.m; ++j)
                for (int k = 1; k <= m1.m; ++k)
                    res.a[i][j] = (res.a[i][j]+m1.a[i][k]*m2.a[k][j])%MOD;
        return res;
    }
    Martix& operator *= (const Martix &mx) { return *this = *this*mx; }
    template <typename TT>
    Martix pow(const TT &p) const {
        Martix res(n, m), a = *this;
        res.init(1);
        for (TT i = p; i; i >>= 1, a *= a) if (i&1) res *= a;
        return res;
    }
    Martix inv() const {
        Martix res = *this;
        vector<int> is(n+1), js(n+1);
        for (int k = 1; k <= n; ++k) {
            for (int i = k; i <= n; ++i)
                for (int j = k; j <= n; ++j) if (res.a[i][j]) {
                    is[k] = i; js[k] = j; break;
                }
            for (int i = 1; i <= n; ++i) swap(res.a[k][i], res.a[is[k]][i]);
            for (int i = 1; i <= n; ++i) swap(res.a[i][k], res.a[i][js[k]]);
            if (!res.a[k][k]) return Martix(0);
            res.a[k][k] = mul_inverse(res.a[k][k]); // get inv of number
            for (int j = 1; j <= n; ++j) if (j != k)
                res.a[k][j] = res.a[k][j]*res.a[k][k]%MOD;
            for (int i = 1; i <= n; ++i) if (i != k)
                for (int j = 1; j <= n; ++j) if (j != k)
                    res.a[i][j] = (res.a[i][j]+MOD-res.a[i][k]*res.a[k][j]%MOD)%MOD;
            for (int i = 1; i <= n; ++i) if (i != k)
                res.a[i][k] = (MOD-res.a[i][k]*res.a[k][k]%MOD)%MOD;
        }
        for (int k = n; k; --k) {
            for (int i = 1; i <= n; ++i) swap(res.a[js[k]][i], res.a[k][i]);
            for (int i = 1; i <= n; ++i) swap(res.a[i][is[k]], res.a[i][k]);
        }
        return res;
    }
    T det() {
        long long res = 1;
        Martix cpy = *this;
        for (int i = 1; i <= n; ++i) {
            for (int j = i+1; j <= n; ++j) while (cpy.a[j][i]) {
                long long t = cpy.a[i][i]/cpy.a[j][i];
                for (int k = i; k <= n; ++k)
                    cpy.a[i][k] = (cpy.a[i][k]+MOD-t*cpy.a[j][k]%MOD)%MOD;
                swap(cpy.a[i], cpy.a[j]);
                res = -res;
            }
            res = res*cpy.a[i][i]%MOD;
        }
        return (res+MOD)%MOD;
    }
    void print() {
        for (int i = 1; i <= n; ++i)
            for (int j = 1; j <= m; ++j)
                cout << a[i][j] << " \n"[j==m];
    }
};
```
## [高斯消元](https://www.luogu.com.cn/problem/P3389)
```cpp
struct GaussElimination
{
    double a[N][N];
    void init() { memset(a, 0, sizeof a); }
    void init(const int &n) {
        for (int i = 1; i <= n; ++i)
            for (int j = 1; j <= n+1; ++j)
                a[i][j] = 0;
    }
    // ans is a[i][n+1]
    bool solve(const int &n) {
        for (int i = 1, j, k; i <= n; ++i) {
            for (j = i+1, k = i; j <= n; ++j)
                if (abs(a[j][i]) > abs(a[k][i])) k = j;
            if (abs(a[k][i]) < eps) return false;
            swap(a[k], a[i]);
            for (j = 1; j <= n; ++j) if (i != j) {
                double d = a[j][i]/a[i][i];
                for (k = i+1; k <= n+1; ++k)
                    a[j][k] -= d*a[i][k];
            }
        }
        for (int i = 1; i <= n; ++i) a[i][n+1] /= a[i][i];
        return true;
    }
};
```
### [异或方程组](http://www.cppblog.com/MatoNo1/archive/2012/05/20/175404.html)
[luogu 2962](https://www.luogu.com.cn/problem/P2962)
```cpp
// -1 : no solution, 0 : multi , 1 : one
template <typename T>
int XorGauss(T a[N], const int &n)
{
    int flag = 1;
    for (int i = 1, j, k; i <= n; ++i) {
        for (k = i; !a[k][i] && k <= n; ++k) {}
        if (k > n) {
            if (a[i][n+1]) return -1;
            flag = 0; continue;
        }
        swap(a[k], a[i]);
        for (j = 1; j <= n; ++j) if (i != j && a[j][i])
            for (k = i; k <= n+1; ++k) a[j][k] ^= a[i][k];
            // a[j] ^= a[i]; // bitset<N> a[N]
    }
    return flag;
}
// dfs(n, 0)
void dfs(const int &u, const int &num)
{
    if (num >= res) return;
    if (u <= 0) { res = num; return; }
    if (a[u][u]) {
        int t = a[u][n+1];
        for (int i = u+1; i <= n; ++i) {
            if (a[u][i]) t ^= used[i];
        }
        dfs(u-1, num+t);
    } else { // 自由元
        dfs(u-1, num);
        used[u] = 1;
        dfs(u-1, num+1);
        used[u] = 0;
    }
}
```
---
## [拉格朗日插值](https://www.luogu.com.cn/problem/P4781)
```cpp
template <typename T, typename H, typename P>
long long Largrange(const T &k, const int &n, const H x[], const P y[])
{
    long long res = 0, s1 = 1, s2 = 1;
    for (int i = 1; i <= n; ++i, s1 = s2 = 1) {
        for (int j = 1; j <= n; ++j) if (i != j) {
            s1 = s1*(x[i]-x[j]+MOD)%MOD;
            s2 = s2*(k-x[j]+MOD)%MOD;
        }
        res = (res+y[i]*s2%MOD*mul_inverse(s1)%MOD)%MOD;
    }
    return res;
}
```
```cpp
template <typename T, typename P> // x[i] = i -> y[i] = f(i)
long long Largrange(const T &k, const int &n, const P y[])
{
    if (k <= n) return y[k];
    static long long pre[N], suf[N];
    long long res = 0;
    pre[0] = suf[n+1] = 1;
    for (int i = 1; i <= n; ++i) pre[i] = pre[i-1]*(k-i)%MOD;
    for (int i = n; i >= 1; --i) suf[i] = suf[i+1]*(k-i)%MOD;
    for (int i = 1; i <= n; ++i) {
        res = (res+y[i]*(pre[i-1]*suf[i+1]%MOD)%MOD
            *mul_inverse(((n-i)&1 ? -1 : 1)*fac[i-1]*fac[n-i]%MOD)%MOD)%MOD;
    }
    return (res+MOD)%MOD;
}
```
---
## [快速幂](https://www.luogu.org/problemnew/show/P1226)
```cpp
template <typename T, typename H>
inline T qpow(const T &a, const H &p, const int &mo = MOD)
{
    long long res = 1, x = a;
    for (H i = p; i; i >>= 1, x = x*x%mo)
        if (i&1) res = res*x%mo;
    return static_cast<T>(res);
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
```
```cpp
inline long long qmul(long long x, long long y, long long mo)
{
    return (long long)((__int128)x*y%mo);
}
```
```cpp
inline long long qmul(long long x, long long y, long long mo)
{
    // x*y - floor(x*y/mo)*mo
    typedef unsigned long long ull;
    typedef long double ld;
    return ((ull)x*y-(ull)((ld)x/mo*y)*mo+mo)%mo;
}
```
---
## 复数
```cpp
struct complex
{
    double real = 0.0, imag = 0.0;
    complex () {}
    complex (int _real, int _imag) : real(_real), imag(_imag) {}
    complex (double _real, double _imag) : real(_real), imag(_imag) {}
    complex operator + (const complex &b) {
        return complex(this->real+b.real, this->imag+b.imag);
    }
    complex operator - (const complex &b) {
        return complex(this->real-b.real, this->imag-b.imag);
    }
    complex operator * (const complex &b) {
        return complex(this->real*b.real-this->imag*b.imag, this->real*b.imag+this->imag*b.real);
    }
    complex operator *= (const complex &b) {
        return *this = *this*b;
    }
};
```
---
## [FFT](https://www.luogu.com.cn/problem/P3803)
```cpp
void FFT(complex a[], int flag)
{
    for (int i = 0; i < len; ++i)
        if (i < rev[i]) swap(a[i], a[rev[i]]);

    for (int base = 1; base < len; base <<= 1) {
        complex w, wn = { cos(PI/base), flag*sin(PI/base) };
        for (int i = 0; i < len; i += base*2) {
            w = { 1.0, 0.0 };
            for (int j = 0; j < base; ++j) {
                complex x = a[i+j], y = w*a[i+j+base];
                a[i+j] = x+y;
                a[i+j+base] = x-y;
                w *= wn;
            }
        }
    }
}
int main()
{
    // read f[n] g[m]
    while (len <= n+m) len <<= 1, ++bit;
    for (int i = 0; i < len; ++i)
        rev[i] = (rev[i>>1]>>1)|((i&1)<<(bit-1));

    FFT(f, 1); FFT(g, 1);
    for (int i = 0; i <= len; ++i) f[i] *= g[i];
    FFT(f, -1);
    for (int i = 0; i <= n+m; ++i)
        printf("%d%c", static_cast<int>(f[i].real/len+0.5), " \n"[i==n+m]);
}
```
---
## NTT
```cpp
void NTT(long long a[], int flag)
{
    for (int i = 0; i < len; ++i)
        if (i < rev[i]) swap(a[i], a[rev[i]]);

    for (int base = 1; base < len; base <<= 1) {
        long long wn = qpow(G, (MOD-1)/(base*2)), w;
        if (flag == -1) wn = qpow(wn, MOD-2);
        for (int i = 0; i < len; i += base*2) {
            w = 1;
            for (int j = 0; j < base; ++j) {
                long long x = a[i+j], y = w*a[i+j+base]%MOD;
                a[i+j] = (x+y)%MOD;
                a[i+j+base] = (x-y+MOD)%MOD;
                w = w*wn%MOD;
            }
        }
    }
}
int main()
{
    while (len <= n+m) len <<= 1, ++bit;
    for (int i = 0; i < len; ++i)
        rev[i] = (rev[i>>1]>>1)|((i&1)<<(bit-1));

    FFT(f, 1);
    FFT(g, 1);
    for (int i = 0; i <= len; ++i) {
        f[i] *= g[i];
    }
    FFT(f, -1);
    long long inv = qpow(len, MOD-2);
    for (int i = 0; i <= n+m; ++i) {
        printf("%lld", f[i]*inv%MOD);
        putchar((i == n+m ? '\n' : ' '));
    }
}
```
---
## [第二类斯特林数](https://www.luogu.com.cn/problem/P5395)
```cpp
inline void stirling(const int &n)
{
    S[0][0] = 1;
    // 注意取模
    for (int i = 1; i <= n; ++i)
        for (int j = 1; j <= i; ++j)
            S[i][j] = S[i-1][j-1]+S[i-1][j]*j;
}
```
```cpp
void stirling(const int &n)
{
    inv[0] = inv[1] = 1;
    for(int i = 2; i <= n; ++i)
        inv[i] = MOD-MOD/i*inv[MOD%i]%MOD;
    for (int i = 1; i <= n; ++i)
        inv[i] = inv[i-1]*inv[i]%MOD;
    while (len <= (n<<1)) len <<= 1, ++bit;
    for (int i = 0; i < len; ++i)
        rev[i] = (rev[i>>1]>>1)|((i&1)<<(bit-1));
    for (int i = 0, one = 1; i <= n; ++i, one = MOD-one) {
        f[i] = one*inv[i]%MOD;
        g[i] = qpow(i, n)*inv[i]%MOD;
    }
    NTT(f, 1); NTT(g, 1);
    for (int i = 0; i < len; ++i) f[i] = f[i]*g[i]%MOD;
    NTT(f, -1);
    long long invv = qpow(len, MOD-2);
    for (int i = 0; i <= n; ++i)
        printf("%lld%c", f[i]*invv%MOD, " \n"[i==n]);
}
```
---
## 约瑟夫环
### [O(n)](https://blog.csdn.net/weixin_42659809/article/details/82596676)
```cpp
int solve(int n, int v) { return n == 1 ? 0 : (solve(n-1, v)+v)%n; }
// res = solve(num, step)+1
```
---
## 最大公因数 gcd
```cpp
__gcd(a, b); // <algorithm>
int gcd(int a, int b) { return b ? gcd(b, a%b) : a; }
inline int gcd(int a, int b) { while (b) a %= b, swap(a, b); return a; }
```
## 最小公倍数 lcm
$LCM(\frac{a}{b},\frac{c}{d})=\frac{LCM(a, c)}{GCD(b,d)}$

$LCM(\frac{a_1}{b_1},\frac{a_2}{b_2},...)=\frac{LCM(a1, a2,...)}{GCD(b1, b2,...)}$
```cpp
inline int lcm(int a, int b) { return a/gcd(a, b)*b; }
```
## 扩展欧几里得([同余方程](https://www.luogu.org/problemnew/show/P1082))
```cpp
template <typename T>
T exgcd(const T a, const T b, T &x, T &y)
{
    if (!b) { x = 1; y = 0; return a; }
    T d = exgcd(b, a%b, y, x);
    y -= a/b*x;
    return d;
}
```
---
## [乘法逆元](https://www.luogu.org/problemnew/show/P3811)
### 拓展欧几里得
```cpp
template <typename T>
inline T mul_inverse(const T &a, const T &mo = MOD)
{
    T x, y;
    exgcd(a, mo, x, y);
    return (x%mo+mo)%mo;
}
```
### 费马小定理
```cpp
template <typename T>
inline T mul_inverse(const T &a, const int &mo = MOD)
{
    return qpow(a, mo-2);
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
## 排列组合
### [奇偶性](https://blog.csdn.net/baodream/article/details/77822072)
C(n,k) 当 `n&k == k` 为奇数反之偶数
---
## 欧拉函数
```cpp
inline long long phi(long long x) {
    long long res = x;
    for (long long i = 2; i*i <= x; ++i) {
        if (x%i) continue;
        res = res/i*(i-1);
        while (x%i == 0) x /= i;
    }
    if (x > 1) res = res/x*(x-1);
    return res;
}
```
### 筛法
```cpp
struct Euler
{
    int phi[N], check[N];
    vector<int> prime;
    void init(int sz) {
        for (int i = 1; i <= sz; ++i) check[i] = 1;
        phi[1] = 1; check[1] = 0;
        for (int i = 2; i <= sz; ++i) {
            if (check[i]) {
                prime.emplace_back(i);
                phi[i] = i-1;
            }
            for (int j : prime) {
                if (i*j > sz) break;
                check[i*j] = 0;
                if (i%j) {
                    phi[i*j] = (j-1)*phi[i];
                } else {
                    phi[i*j] = j*phi[i];
                    break;
                }
            }
        }
    }
} E;
```
---
## [线性筛](https://www.luogu.org/problemnew/show/P3383)
```cpp
struct Euler
{
    int tot = 0;
    int prime[N];
    bool check[N];
    bool& operator [] (const int i) { return check[i]; }
    void init(int sz) {
        tot = 0;
        for (int i = 1; i <= sz; ++i) check[i] = true;
        check[1] = false;
        for (register int i = 2, j; i <= sz; ++i) {
            if (check[i]) prime[++tot] = i;
            for (j = 1; j <= tot && i*prime[j] <= sz; ++j) {
                check[i*prime[j]] = false;
                if (i%prime[j] == 0) break;
            }
        }
    }
} E;
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
### Miller-Rabin 素性测试
```cpp
inline bool MillerRabin(int x)
{
    static const int test_time = 10;
    if (x < 3) return x == 2;
    int a = x-1, b = 0;
    while (!(a&1)) a >>= 1, ++b;
    for (int i = 1, j, v; i <= test_time; ++i) {
        v = (qpow(rnd()%(x-2)+2, a, x));
        if (v == 1 || v == x-1) continue;
        for (j = 0; j < b && v != x-1; ++j)
            v = static_cast<int>(1ll*v*v%x);
        if (j >= b) return false;
    }
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
# 动态规划 DP
(我**全**都不会)
## 记忆化搜索
## 线性DP
### [最长上升子序列LIS](http://codevs.cn/problem/1576/)
```cpp
for(int i = 1; i <= n; ++i)
{
    f[i] = 1;
    for(int j = 1; j < i; ++j)
        if(a[i] > a[j]) f[i] = max(f[i],f[j]+1);
}
```
### 最长公共子序列LCS
```cpp
f[i][j] = max{  f[i-1][j],
                f[i][j-1],
                f[i-1][j-1]+1 (if A[i] == B[j])}
```
### 数字三角形
## 区间DP
## [树形DP](https://www.luogu.org/problemnew/show/P1352)
## 状压DP
### [枚举子集](https://cp-algorithms.com/algebra/all-submasks.html)
```cpp
for (int i = s; i; i = (i-1)&s) {}
```
### 枚举n个元素大小为k的二进制子集
```cpp
int s=(1<<k)-1;
while(s<(1<<n)){
    work(s);
    int x=s&-s,y=s+x;
    s=((s&~y)/x>>1)|y; //这里有一个位反~
}
```

## 队列优化
## 斜率优化

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
## [SOS DP](https://codeforces.com/blog/entry/45223)
---
# STL
## 数据结构
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
## 函数
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
## unordered_map 重载
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
            return hash<int>()(x.a)^hash<int>()(x.b);
        }
    };
}
unordered_map<Node, int> mp;

// 方法二
struct KeyHasher
{
    size_t operator () (const Node &x) const {
        return hash<int>()(x.a)^hash<int>()(x.b);
    }
};
unordered_map<Node, int, KeyHasher> mmp;
```
## 定义函数
```cpp
function<void(int&, int)> f = [&](int &x, int y) -> void {
    x += y;
};
```
---
# 模数
## 弟弟操作
```cpp
template <int _MOD> struct Mint
{
    int v = 0;
    Mint() {}
    Mint(int _v) : v((_v%_MOD+_MOD)%_MOD) {}
    Mint(long long _v) : v(static_cast<int>((_v%_MOD+_MOD)%_MOD)) {}
    Mint operator = (const int &_v) { return *this = Mint(_v); }
    Mint operator = (const long long &_v) { return *this = Mint(_v); }
    bool operator ! () const { return !this->v; }
    bool operator < (const Mint &b) const { return v < b.v; }
    bool operator > (const Mint &b) const { return v > b.v; }
    bool operator == (const Mint &b) const { return v == b.v; }
    bool operator != (const Mint &b) const { return v != b.v; }
    bool operator <= (const Mint &b) const { return v < b.v || v == b.v; }
    bool operator >= (const Mint &b) const { return v > b.v || v == b.v; }
    Mint operator + (const Mint &b) const { return Mint(v+b.v); }
    Mint operator - (const Mint &b) const { return Mint(v-b.v); }
    Mint operator * (const Mint &b) const { return Mint(1ll*v*b.v); }
    Mint operator / (const Mint &b) const { return Mint(b.inv()*v); }
    Mint& operator += (const Mint &b) { return *this = *this+b; }
    Mint& operator -= (const Mint &b) { return *this = *this-b; }
    Mint& operator *= (const Mint &b) { return *this = *this*b; }
    Mint& operator /= (const Mint &b) { return *this = *this/b; }
    Mint operator - () const { return Mint(-v); }
    Mint& operator ++ () { return *this += 1; }
    Mint& operator -- () { return *this -= 1; }
    Mint operator ++ (int) { Mint tmp = *this; *this += 1; return tmp; }
    Mint operator -- (int) { Mint tmp = *this; *this -= 1; return tmp; }
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
## tourist的模板(用不来)
```cpp
template <typename T>
class Modular {
 public:
  using Type = typename decay<decltype(T::value)>::type;
 
  constexpr Modular() : value() {}
  template <typename U>
  Modular(const U& x) {
    value = normalize(x);
  }
 
  template <typename U>
  static Type normalize(const U& x) {
    Type v;
    if (-mod() <= x && x < mod()) v = static_cast<Type>(x);
    else v = static_cast<Type>(x % mod());
    if (v < 0) v += mod();
    return v;
  }
 
  const Type& operator()() const { return value; }
  template <typename U>
  explicit operator U() const { return static_cast<U>(value); }
  constexpr static Type mod() { return T::value; }
 
  Modular& operator+=(const Modular& other) { if ((value += other.value) >= mod()) value -= mod(); return *this; }
  Modular& operator-=(const Modular& other) { if ((value -= other.value) < 0) value += mod(); return *this; }
  template <typename U> Modular& operator+=(const U& other) { return *this += Modular(other); }
  template <typename U> Modular& operator-=(const U& other) { return *this -= Modular(other); }
  Modular& operator++() { return *this += 1; }
  Modular& operator--() { return *this -= 1; }
  Modular operator++(int) { Modular result(*this); *this += 1; return result; }
  Modular operator--(int) { Modular result(*this); *this -= 1; return result; }
  Modular operator-() const { return Modular(-value); }
 
  template <typename U = T>
  typename enable_if<is_same<typename Modular<U>::Type, int>::value, Modular>::type& operator*=(const Modular& rhs) {
#ifdef _WIN32
    uint64_t x = static_cast<int64_t>(value) * static_cast<int64_t>(rhs.value);
    uint32_t xh = static_cast<uint32_t>(x >> 32), xl = static_cast<uint32_t>(x), d, m;
    asm(
      "divl %4; \n\t"
      : "=a" (d), "=d" (m)
      : "d" (xh), "a" (xl), "r" (mod())
    );
    value = m;
#else
    value = normalize(static_cast<int64_t>(value) * static_cast<int64_t>(rhs.value));
#endif
    return *this;
  }
  template <typename U = T>
  typename enable_if<is_same<typename Modular<U>::Type, int64_t>::value, Modular>::type& operator*=(const Modular& rhs) {
    int64_t q = static_cast<int64_t>(static_cast<long double>(value) * rhs.value / mod());
    value = normalize(value * rhs.value - q * mod());
    return *this;
  }
  template <typename U = T>
  typename enable_if<!is_integral<typename Modular<U>::Type>::value, Modular>::type& operator*=(const Modular& rhs) {
    value = normalize(value * rhs.value);
    return *this;
  }
 
  Modular& operator/=(const Modular& other) { return *this *= Modular(inverse(other.value, mod())); }
 
  template <typename U>
  friend const Modular<U>& abs(const Modular<U>& v) { return v; }
 
  template <typename U>
  friend bool operator==(const Modular<U>& lhs, const Modular<U>& rhs);
 
  template <typename U>
  friend bool operator<(const Modular<U>& lhs, const Modular<U>& rhs);
 
  template <typename U>
  friend std::istream& operator>>(std::istream& stream, Modular<U>& number);
 
 private:
  Type value;
};
 
template <typename T> bool operator==(const Modular<T>& lhs, const Modular<T>& rhs) { return lhs.value == rhs.value; }
template <typename T, typename U> bool operator==(const Modular<T>& lhs, U rhs) { return lhs == Modular<T>(rhs); }
template <typename T, typename U> bool operator==(U lhs, const Modular<T>& rhs) { return Modular<T>(lhs) == rhs; }
 
template <typename T> bool operator!=(const Modular<T>& lhs, const Modular<T>& rhs) { return !(lhs == rhs); }
template <typename T, typename U> bool operator!=(const Modular<T>& lhs, U rhs) { return !(lhs == rhs); }
template <typename T, typename U> bool operator!=(U lhs, const Modular<T>& rhs) { return !(lhs == rhs); }
 
template <typename T> bool operator<(const Modular<T>& lhs, const Modular<T>& rhs) { return lhs.value < rhs.value; }
 
template <typename T> Modular<T> operator+(const Modular<T>& lhs, const Modular<T>& rhs) { return Modular<T>(lhs) += rhs; }
template <typename T, typename U> Modular<T> operator+(const Modular<T>& lhs, U rhs) { return Modular<T>(lhs) += rhs; }
template <typename T, typename U> Modular<T> operator+(U lhs, const Modular<T>& rhs) { return Modular<T>(lhs) += rhs; }
 
template <typename T> Modular<T> operator-(const Modular<T>& lhs, const Modular<T>& rhs) { return Modular<T>(lhs) -= rhs; }
template <typename T, typename U> Modular<T> operator-(const Modular<T>& lhs, U rhs) { return Modular<T>(lhs) -= rhs; }
template <typename T, typename U> Modular<T> operator-(U lhs, const Modular<T>& rhs) { return Modular<T>(lhs) -= rhs; }
 
template <typename T> Modular<T> operator*(const Modular<T>& lhs, const Modular<T>& rhs) { return Modular<T>(lhs) *= rhs; }
template <typename T, typename U> Modular<T> operator*(const Modular<T>& lhs, U rhs) { return Modular<T>(lhs) *= rhs; }
template <typename T, typename U> Modular<T> operator*(U lhs, const Modular<T>& rhs) { return Modular<T>(lhs) *= rhs; }
 
template <typename T> Modular<T> operator/(const Modular<T>& lhs, const Modular<T>& rhs) { return Modular<T>(lhs) /= rhs; }
template <typename T, typename U> Modular<T> operator/(const Modular<T>& lhs, U rhs) { return Modular<T>(lhs) /= rhs; }
template <typename T, typename U> Modular<T> operator/(U lhs, const Modular<T>& rhs) { return Modular<T>(lhs) /= rhs; }
 
template<typename T, typename U>
Modular<T> power(const Modular<T>& a, const U& b) {
  assert(b >= 0);
  Modular<T> x = a, res = 1;
  U p = b;
  while (p > 0) {
    if (p & 1) res *= x;
    x *= x;
    p >>= 1;
  }
  return res;
}
 
template <typename T>
bool IsZero(const Modular<T>& number) {
  return number() == 0;
}
 
template <typename T>
string to_string(const Modular<T>& number) {
  return to_string(number());
}
 
template <typename T>
std::ostream& operator<<(std::ostream& stream, const Modular<T>& number) {
  return stream << number();
}
 
template <typename T>
std::istream& operator>>(std::istream& stream, Modular<T>& number) {
  typename common_type<typename Modular<T>::Type, int64_t>::type x;
  stream >> x;
  number.value = Modular<T>::normalize(x);
  return stream;
}
 
/*
using ModType = int;
 
struct VarMod { static ModType value; };
ModType VarMod::value;
ModType& md = VarMod::value;
using Mint = Modular<VarMod>;
*/
 
constexpr int md = 998244353;
using Mint = Modular<std::integral_constant<decay<decltype(md)>::type, md>>;
```
---
# 高精度
## vector版本
[压位+vector+符号 版本](https://github.com/KaizynX/Oier/blob/master/BigInteger/BigInteger.cpp)
## int[]版本
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
    BigInteger operator = (long long num) {
        len = tag = 0;
        memset(v, 0, sizeof v);
        do {
            v[++len] = (int)(num%BASE);
            num /= BASE;
        } while (num > 0);
        return *this;
    }
    // string 转 BigInteger
    BigInteger operator = (const string &str) {
        string buf;
        int r = (int)str.length()-1, l = max(0, r-WIDTH+1);
        len = tag = 0;
        memset(v, 0, sizeof v);
        while (r >= 0) {
            buf = str.substr(l, r-l+1);
            sscanf(buf.c_str(), "%d", &v[++len]);
            r -= WIDTH; l = max(0, r-WIDTH+1);
        }
        return *this;
    }
    // 比较运算
    bool operator < (const BigInteger &b) const {
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
    BigInteger operator + (const BigInteger &b) const {
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
    BigInteger operator - () const {
        BigInteger res = *this;
        res.tag ^= 1;
        return res;
    }
    BigInteger operator - (const BigInteger &b) const {
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
    BigInteger operator * (int b) const {
        BigInteger res;
        long long tmp;
        res.len = len;
        for(int i = 1; i <= len; ++i) {
            tmp = 1ll*b*v[i];
            res.v[i] += (int)(tmp%BASE);
            res.v[i+1] += (int)(tmp/BASE+res.v[i]/BASE);
            res.v[i] %= BASE;
        }
        while(res.v[res.len+1] > 0) res.len++;
        return res;
    }
    // 高精度乘高精度
    BigInteger operator * (const BigInteger &b) const {
        BigInteger res;
        res.len = len+b.len;
        for(int i = 1; i <= len; ++i)
            for(int j = 1; j <= b.len; ++j) {
                res.v[i+j-1] += v[i]*b.v[j];
                res.v[i+j] += res.v[i+j-1]/BASE;
                res.v[i+j-1] %= BASE;
            }
        while(res.len > 1 && res.v[res.len] == 0) res.len--;
        return res;
    }
    // 高精度除低精度
    BigInteger operator / (int b) const {
        long long divisor = 0;
        BigInteger res;
        for(int i = len; i; --i) {
            divisor = divisor*BASE+v[i];
            if(divisor < b) continue;
            res.v[i] = (int)(divisor/b);
            divisor %= b;
            res.len = max(res.len, i);
        }
        return res;
    }
    // 高精度除高精度
    BigInteger operator / (const BigInteger &b) const {
        BigInteger divisor, res;
        int l, r, mid;
        for(int i = len; i; --i) {
            divisor = divisor*BASE+v[i];
            /*
            memcpy(divisor.v+1, divisor.v, sizeof(int)*(divisor.len+1));
            while(divisor.v[divisor.len+1] > 0) divisor.len++;
            divisor.v[1] = v[i];
            */
            if(divisor < b) continue;
            l = 0; r = BASE-1;
            while(l < r) {
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
    BigInteger& operator += (const BigInteger &b) { return *this = *this+b; }
    BigInteger& operator -= (const BigInteger &b) { return *this = *this-b; }
    BigInteger& operator *= (const BigInteger &b) { return *this = *this*b; }
    BigInteger& operator /= (const BigInteger &b) { return *this = *this/b; }
    BigInteger& operator %= (const BigInteger &b) { return *this = *this%b; }
    BigInteger& operator *= (int b) { return *this = *this*b; }
    BigInteger& operator /= (int b) { return *this = *this/b; }
    BigInteger& operator %= (int b) { return *this = *this%b; }
    // 重载输入运算符
    friend istream& operator >> (istream &is, BigInteger &big) {
        string buf;
        if (is >> buf) big = buf;
        return is;
    }
    // 重载输出运算符
    friend ostream& operator << (ostream &os, const BigInteger &big)
    {
        static char buf[10];
        if (big.tag) os << '-';
        os << big.v[big.len];
        for (int i = big.len-1; i; --i) {
            sprintf(buf, "%04d", big.v[i]);
            for (int j = 0; j < 4; ++j) os << buf[j];
        }
        return os;
    }
    // 幂
    template <typename T>
    friend BigInteger pow (BigInteger a, T p) {
        if(p == 0) return 1;
        BigInteger res = 1;
        while(p) {
            if(p%2) res *= a;
            a *= a;
            p /= 2;
        }
        return res;
    }
    // 开根
    friend BigInteger sqrt(const BigInteger &a, const int p = 2) {
        BigInteger l, r = a, mid;
        while(l < r) {
            mid = (l+r+1)/2;
            if(pow(mid, p) <= a) l = mid;
            else r = mid-1;
        }
        return l;
    }
    friend BigInteger gcd(BigInteger a, BigInteger b) {
        while (b > 0) a %= b, swap(a, b);
        return a;
    }
    friend BigInteger lcm(const BigInteger &a, const BigInteger &b) {
        return a/gcd(a, b)*b;
    }
};
```
---