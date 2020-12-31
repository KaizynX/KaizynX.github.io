---
title: ACM模板
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2018-11-05 07:00:00
mathjax: true
categories:
  - ACM
tags:
top: true
keywords: ACM模板
description:  某菜鸡收集的板子
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/template/cover.jpg
---

# 前言
{% spoiler "代码" %}
```cpp
while (true) ++++++ ++++++ ++++++ RP;
```

{% endspoiler %}
<!--more-->

---
# 杂项
## 快读快写
{% spoiler "代码" %}
```cpp
template <typename T> inline void read(T &x) {
  int c; T tag = 1;
  while(!isdigit((c=getchar()))) if(c == '-') tag = -1;
  x = c-'0';
  while(isdigit((c=getchar()))) x = (x<<1)+(x<<3) + c-'0';
  x *= tag;
}

template <typename T> void write(T x) {
  if(x < 0) x = -x, putchar('-');
  if(x > 9) write(x/10);
  putchar(x%10+'0');
}
```

{% endspoiler %}
{% spoiler "代码" %}
```cpp
ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
```

{% endspoiler %}
[巨佬模板](https://atcoder.jp/contests/hitachi2020/submissions/10676875)
{% spoiler "代码" %}
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

{% endspoiler %}

---
## 玄学优化

吸氧,吸臭氧
{% spoiler "代码" %}
```cpp
#pragma GCC optimize("Ofast,no-stack-protector")
#pragma GCC target("sse,sse2,sse3,ssse3,sse4,popcnt,abm,mmx,avx,avx2,tune=native")
```

{% endspoiler %}

## 正则表达式
{% spoiler "代码" %}
```cpp
char str[];
scanf("%3s", str); // 读取长度为n的字符串
scanf("%[abc]", str); // 读取a,b,c,读到之外的立即停止
scanf("%[a-z0-9]", str); // 同上,读取小写字母和数字
scanf("%*[a-z]%s", str); // 过滤掉小写字母读取
scanf("%[^a-z]", str); // 读取小写字符外字符,^表示非 
```

{% endspoiler %}
## 随机数
{% spoiler "代码" %}
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
```cpp
inline ull xorshift128(){
  static U SX=335634763,SY=873658265,SZ=192849106,SW=746126501;
  U t=SX^(SX<<11);
  SX=SY;
  SY=SZ;
  SZ=SW;
  return SW=SW^(SW>>19)^t^(t>>8);
}
inline ull myrand(){return (xorshift128()<<32)^xorshift128();}
```

{% endspoiler %}

---

## 哈希
双哈希,好多哈希,[题目链接](https://codeforces.com/contest/1394/problem/B)
{% spoiler "代码" %}
```cpp
struct HashNumber {
  static const int HN = 2;
  int a[HN], mod[HN] = {25165843, 50331653};
  HashNumber() { memset(a, 0, sizeof a); }
  template <typename U> HashNumber(const U &x) {
    for (int i = 0; i < HN; ++i) a[i] = x%mod[i];
  }
  friend bool operator ==(const HashNumber &lhs, const HashNumber &rhs) {
    for (int i = 0; i < HN; ++i) if (lhs.a[i] != rhs.a[i]) return false;
    return true;
  }
  friend bool operator !=(const HashNumber &lhs, const HashNumber &rhs) {
    return !(lhs == rhs);
  }
  HashNumber& operator +=(const HashNumber &hs) {
    for (int i = 0; i < HN; ++i) (a[i] += hs.a[i]) %= mod[i];
    return *this;
  }
  HashNumber& operator *=(const HashNumber &hs) {
    for (int i = 0; i < HN; ++i) a[i] = (1ll*a[i]*hs.a[i])%mod[i];
    return *this;
  }
  friend HashNumber operator +(const HashNumber &lhs, const HashNumber &rhs) {
    HashNumber res = lhs;
    return res += rhs;
  }
  friend HashNumber operator *(const HashNumber &lhs, const HashNumber &rhs) {
    HashNumber res = lhs;
    return res *= rhs;
  }
};
```

{% endspoiler %}

---
## 计算log2
{% spoiler "代码" %}
```cpp
#define log(x) (31-__builtin_clz(x))
// lg2[i] = lg2(i) +1
for(int i = 1; i <= n; ++i) lg2[i] = lg2[i>>1]+1;
// lg2[i] = (int)log2(i)
for(int i = 2; i <= n; ++i) lg2[i] = lg2[i>>1]+1;
```

{% endspoiler %}
## 快速开根号|牛顿迭代法
{% spoiler "代码" %}
```cpp
double sqrt(const double &a) {
  double x = a, y = .0;
  while (abs(x-y) > err) {
    y = x;
    x = .5*(x+a/x);
  }
  return x;
}
```

{% endspoiler %}

## i/k == j 的 k 的个数
{% spoiler "代码" %}
```cpp
for (int i = 1; i <= n; ++i) {
  for (int j = 1, l, r; j <= n; ++j) {
    l = max(1, i/(j+1));
    while (l-1 >= 1 && i/(l-1) == j) --l;
    while (i/l > j) ++l;
    r = i/j;
    while (r+1 <= i && i/(r+1) == j) ++r;
    while (i/r < j) --r;
    if (r-l+1 != i/j-i/(j+1)) {
        cout << i << " " << j << endl;
    }
  }
}
```

{% endspoiler %}

## [三分法](https://blog.csdn.net/inf_force/article/details/44891121)
示例为凹函数
{% spoiler "代码" %}
```cpp
while (l < r) {
  int mid = (l+r)>>1;
  if (f(mid) < f(mid+1)) r = mid;
  else l = mid+1;
}
```
```cpp
while(r-l>5){
	int ml=(l+l+r)/3;
	int mr=(l+r+r)/3;
	if(f(ml)<f(mr))r=mr;
	else l=ml;
}
for (int i = l; i <= r; ++i) res = min(res, f(i));
```
```cpp
while (r-l > eps) {
  double ml = l+(r-l)/3, mr = r-(r-l)/3;
  if (f(ml) < f(mr)) r = mr;
  else l = ml;
}
```

{% endspoiler %}

---
# 计算几何
## 向量 坐标 直线 圆 (结构体)
{% spoiler "代码" %}
```cpp
struct Point {
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
  // a*b b在a的顺负逆正
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

template <typename T = double>
struct Vec { // 三维向量
  T x, y, z;
  Vec(const T &_x = 0, const T &_y = 0, const T &_z = 0) : x(_x), y(_y), z(_z) {}
  double len() { return sqrt(1.0*x*x+1.0*y*y+1.0*z*z); }
  friend Vec operator +(const Vec &v1, const Vec &v2) { return Vec(v1.x+v2.x, v1.y+v2.y, v1.z+v2.z); }
  friend Vec operator -(const Vec &v1, const Vec &v2) { return Vec(v1.x-v2.x, v1.y-v2.y, v1.z-v2.z); }
  friend Vec operator *(const T &k, const Vec &v) { return Vec(k*v.x, k*v.y, k*v.z); }
  friend Vec operator *(const Vec &v, const T &k) { return k*v; }
  friend Vec operator *(const Vec &v1, const Vec &v2) {
    return Vec(
        v1.y*v2.z-v1.z*v2.y,
        v1.z*v2.x-v1.x*v2.z,
        v1.x*v2.y-v1.y*v2.x
    );
  }
  friend T dot(const Vec &v1, const Vec &v2) { return v1.x*v2.x+v1.y*v2.y+v1.z*v2.z; }
  T dot(const Vec &v) { return dot(*this, v); }
  Vec& operator +=(const Vec &v) { return *this = *this+v; }
  Vec& operator -=(const Vec &v) { return *this = *this-v; }
  Vec& operator *=(const T &k) { return *this = *this*k; }
  Vec& operator *=(const Vec &v) { return *this = *this*v; }
  friend istream& operator >>(istream &is, Vec &v) { return is >> v.x >> v.y >> v.z; }
};

inline bool polar_angle1(const Point &p1, const Point &p2) {
  double d1 = atan(p1), d2 = atan(p2); 
  return d1 == d2 ? p1 < p2 : d1 < d2;
}

inline bool polar_angle2(const Point &p1, const Point &p2) {
  auto tmp = p1*p2;
  return tmp == 0 ? p1 < p2 : tmp > 0;
}

inline long long S(const Point &p1, const Point &p2, const Point &p3) {
  return abs(p1.x*p2.y+p1.y*p3.x+p2.x*p3.y-p1.x*p3.y-p1.y*p2.x-p2.y*p3.x);
}

struct Line {
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
    return abs((l.p1-l.p2)*(l.p1-p)) < eps;
  }
  friend Point cross_point(const Line &l1, const Line &l2) {
    Point v1 = l1.p2-l1.p1, v2 = l2.p2-l2.p1;
    if (abs(v1*v2) < eps) return Point(0, 0); // no cross_point
    double t = (l2.p1-l1.p1)*v2/(v1*v2);
    return l1.p1+v1*t;
  }
};

struct Circular {
  Point o;
  double r;
  Circular(){}
  Circular(const Point &_o, const double &_r) : o(_o), r(_r) {}
  template <typename T>
  Circular(const T &_x, const T &_y, const double &_r) : o(Point(_x, _y)), r(_r) {}
  friend bool in_cir(const Circular &c, const Point &p) { return dis(c.o, p) <= c.r; }
  bool in_cir(const Point &p) { return dis(o, p) <= r; }
};

inline Circular get_cir(const Point &p1, const Point &p2, const Point &p3) {
  Circular res;
  res.o = cross_point(Line((p1+p2)/2, (p1+p2)/2+(p2-p1).rotate_90_c()),
            Line((p1+p3)/2, (p1+p3)/2+(p3-p1).rotate_90_c()));
  res.r = dis(res.o, p1);
  return res;
}
```

{% endspoiler %}

---
## [二维凸包](https://www.luogu.com.cn/problem/P2742)
{% spoiler "代码" %}
```cpp
int n;
int stk[N], used[N], tp;
Point p[N];

inline void Andrew() {
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

{% endspoiler %}

---
## [平面最近点对](https://www.luogu.com.cn/problem/P1429)
{% spoiler "代码" %}
```cpp
Point a[N];
int n, ansa, ansb;
double mindist;

inline bool cmp_y(const Point &p1, const Point &p2) { return p1.y < p2.y; }

void upd_ans(const Point &p1, const Point &p2) {
  double dist = dis(p1, p2);
  if (dist < mindist) mindist = dist, ansa = p1.id, ansb = p2.id;
}

void rec(int l, int r) {
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

inline void mindist_pair() {
  sort(a+1, a+n+1);
  mindist = INF;
  rec(1, n);
}
```

{% endspoiler %}

---
## [最小圆覆盖|随即增量法](https://www.luogu.com.cn/problem/P1742)
{% spoiler "代码" %}
```cpp
inline Circular RIA() {
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

{% endspoiler %}

---
# 数据结构
## [堆](https://www.luogu.org/problemnew/show/P3378)
{% spoiler "代码" %}
```cpp
struct Heap {
  static const int Maxn = 1e6+7;
  int sz, a[Maxn];
  Heap() { sz = 0; memset(a, 0, sizeof a); }
  inline bool cmp(int x, int y) { return x < y; } // 小根堆
  inline int size() { return sz; }
  inline bool empty() { return sz == 0; }
  inline int top() { return a[1]; }
  inline void push(int x) { a[++sz] = x; swift_up(sz); }
  inline void pop() { swap(a[1], a[sz--]); swift_down(1); }
  inline void swift_up(int p) {
    while(p > 1 && cmp(a[p], a[p>>1])) // a[p] < a[p<<1]
      swap(a[p], a[p>>1]), p >>= 1;
  }
  inline void swift_down(int p) {
    int l, r, s;
    while(true) {
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

{% endspoiler %}

## 二叉查找树
## [平衡树](https://www.luogu.org/problemnew/show/P3369)
### [Splay](https://www.luogu.org/blog/user19027/solution-p3369)
{% spoiler "代码" %}
```cpp
struct Splay {
  #define root e[0].ch[1]
  typedef int T;
  struct node {
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

{% endspoiler %}
区间反转
{% spoiler "代码" %}
```cpp
struct Splay {
  typedef int T;
  struct node {
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

{% endspoiler %}
## [线段树](https://www.luogu.org/problemnew/show/P3372)
### 区间加减区间和
{% spoiler "代码" %}
```cpp
template <typename T>
struct SegmentTree {
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
动态开点
```cpp
template <typename T>
struct SegmentTree { // 区间加减区间和
  struct TreeNode {
    int l, r;
    TreeNode *ls, *rs;
    T val, lazy;
    TreeNode() { ls = rs = nullptr; }
    TreeNode(const int &_l, const int &_r, const T &_val = 0)
        : l(_l), r(_r), val(_val) {
      ls = rs = nullptr;
      lazy = 0;
    }
    int len() const { return r-l+1; }
  };
  TreeNode *root;
  void clear(TreeNode *rt) {
    if (!rt) return;
    clear(rt->ls);
    clear(rt->rs);
    delete rt;
  }
  void build(const int &n) {
    clear(root);
    root = new TreeNode(1, n);
  }
  void push_up(TreeNode *rt) {
    rt->val = (rt->ls ? rt->ls->val : 0)+(rt->rs ? rt->rs->val : 0);
  }
  void push_down(TreeNode *rt) {
    if (!rt->lazy) return;
    int mid = (rt->l+rt->r)>>1;
    if (!rt->ls) rt->ls = new TreeNode(rt->l, mid);
    if (!rt->rs) rt->rs = new TreeNode(mid+1, rt->r);
    rt->ls->lazy += rt->lazy;
    rt->ls->val += rt->lazy*rt->ls->len();
    rt->rs->lazy += rt->lazy;
    rt->rs->val += rt->lazy*rt->rs->len();
    rt->lazy = 0;
  }
  void update(const int &x, const T &k) { update(x, k, root); }
  void update(const int &x, const T &k, TreeNode *rt) {
    if (rt->l == x && rt->r == x) return rt->val = k, void();
    push_down(rt);
    int mid = (rt->l+rt->r)>>1;
    if (x <= mid) update(x, k, rt->ls ? rt->ls : rt->ls = new TreeNode(rt->l, mid));
    else update(x, k, rt->rs ? rt->rs : rt->rs = new TreeNode(mid+1, rt->r));
    push_up(rt);
  }
  void add(const int &x, const T &k) { add(x, x, k, root); }
  void add(const int &l, const int &r, const T &k) { add(l, r, k, root); }
  void add(const int &l, const int &r, const T &k, TreeNode *rt) {
    if (rt->l >= l && rt->r <= r) {
      rt->val += rt->len()*k;
      rt->lazy += k;
      return;
    }
    push_down(rt);
    int mid = (rt->l+rt->r)>>1;
    if (l <= mid) add(l, r, k, rt->ls ? rt->ls : rt->ls = new TreeNode(rt->l, mid));
    if (r >  mid) add(l, r, k, rt->rs ? rt->rs : rt->rs = new TreeNode(mid+1, rt->r));
    push_up(rt);
  }
  T query(const int &x) { return query(x, x, root); }
  T query(const int &l, const int &r) { return query(l, r, root); }
  T query(const int &l, const int &r, TreeNode *rt) {
    if (rt->l >= l && rt->r <= r) return rt->val;
    push_down(rt);
    int mid = (rt->l+rt->r)>>1;
    if (l <= mid && !rt->ls) rt->ls = new TreeNode(rt->l, mid);
    if (r >  mid && !rt->rs) rt->rs = new TreeNode(mid+1, rt->r);
    if (r <= mid) return query(l, r, rt->ls);
    if (l >  mid) return query(l, r, rt->rs);
    return query(l, r, rt->ls)+query(l, r, rt->rs);
  }
};
```
内存池
```cpp
template <typename T>
struct SegmentTree { // 区间加减区间和
  struct TreeNode {
    int l, r;
    TreeNode *ls, *rs;
    T val, lazy;
    int len() const { return r-l+1; }
  } tr[N<<1];
  queue<TreeNode*> memory_pool;
  SegmentTree() { for (int i = 0; i < N<<2; ++i) memory_pool.push(tr+i); }
  TreeNode* fuck(const int &l, const int &r, const T &val = 0) { // new
    TreeNode *ptr = memory_pool.front();
    memory_pool.pop();
    ptr->l = l; ptr->r = r; ptr->val = val;
    ptr->ls = ptr->rs = nullptr;
    return ptr;
  }
  void shit(TreeNode *ptr) { memory_pool.push(ptr); } // delete
  TreeNode *root;
  void clear(TreeNode *rt) {
    if (!rt) return;
    clear(rt->ls);
    clear(rt->rs);
    shit(rt);
  }
  void build(const int &n) {
    clear(root);
    root = fuck(1, n);
  }
  void push_up(TreeNode *rt) {
    rt->val = (rt->ls ? rt->ls->val : 0)+(rt->rs ? rt->rs->val : 0);
  }
  void push_down(TreeNode *rt) {
    if (!rt->lazy) return;
    int mid = (rt->l+rt->r)>>1;
    if (!rt->ls) rt->ls = fuck(rt->l, mid);
    if (!rt->rs) rt->rs = fuck(mid+1, rt->r);
    rt->ls->lazy += rt->lazy;
    rt->ls->val += rt->lazy*rt->ls->len();
    rt->rs->lazy += rt->lazy;
    rt->rs->val += rt->lazy*rt->rs->len();
    rt->lazy = 0;
  }
  void update(const int &x, const T &k) { update(x, k, root); }
  void update(const int &x, const T &k, TreeNode *rt) {
    if (rt->l == x && rt->r == x) return rt->val = k, void();
    push_down(rt);
    int mid = (rt->l+rt->r)>>1;
    if (x <= mid) update(x, k, rt->ls ? rt->ls : rt->ls = fuck(rt->l, mid));
    else update(x, k, rt->rs ? rt->rs : rt->rs = fuck(mid+1, rt->r));
    push_up(rt);
  }
  void add(const int &x, const T &k) { add(x, x, k, root); }
  void add(const int &l, const int &r, const T &k) { add(l, r, k, root); }
  void add(const int &l, const int &r, const T &k, TreeNode *rt) {
    if (rt->l >= l && rt->r <= r) {
      rt->val += rt->len()*k;
      rt->lazy += k;
      return;
    }
    push_down(rt);
    int mid = (rt->l+rt->r)>>1;
    if (l <= mid) add(l, r, k, rt->ls ? rt->ls : rt->ls = fuck(rt->l, mid));
    if (r >  mid) add(l, r, k, rt->rs ? rt->rs : rt->rs = fuck(mid+1, rt->r));
    push_up(rt);
  }
  T query(const int &x) { return query(x, x, root); }
  T query(const int &l, const int &r) { return query(l, r, root); }
  T query(const int &l, const int &r, TreeNode *rt) {
    if (rt->l >= l && rt->r <= r) return rt->val;
    push_down(rt);
    int mid = (rt->l+rt->r)>>1;
    if (l <= mid && !rt->ls) rt->ls = fuck(rt->l, mid);
    if (r >  mid && !rt->rs) rt->rs = fuck(mid+1, rt->r);
    if (r <= mid) return query(l, r, rt->ls);
    if (l >  mid) return query(l, r, rt->rs);
    return query(l, r, rt->ls)+query(l, r, rt->rs);
  }
};
```

{% endspoiler %}
### 区间修改区间和
{% spoiler "代码" %}
```cpp
template <typename T>
struct SegmentTree {
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

{% endspoiler %}
### 区间加减区间最值
{% spoiler "代码" %}
```cpp
template <typename T, typename U = greater<T>>
struct SegmentTree {
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
    if (trl == x && trr == x) return tr[i] = k, void();
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
    if (r <=  mid) return _query(l, r, trl, mid, i<<1);
    if (l >  mid) return _query(l, r, mid+1, trr, i<<1|1);
    return mv(_query(l, r, trl, mid, i<<1), _query(l, r, mid+1, trr, i<<1|1));
  }
};
```

{% endspoiler %}
### 区间更新最值
{% spoiler "代码" %}
```cpp
template <typename T, typename U = greater<T>>
struct SegmentTree {
  SegmentTree(){}
  void build(const int &_n, const T &k = 0) { n = _n; _build(1, n, k); }
  template <typename TT>
  void buiil(const int _n, const TT a[]) { n = _n; _build(1, n, a); }
  void modify(const int &x, const T &k) { _modify(x, x, k); }
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

{% endspoiler %}
## ZKW线段树
`warning:区间最值尚为验证`
{% spoiler "代码" %}
```cpp
template <typename T>
struct zkwSegmentTree {
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

{% endspoiler %}

## 李超线段树

李超线段树是一种用于维护平面直角坐标系内线段关系的数据结构。它常被用来处理这样一种形式的问题：给定一个平面直角坐标系，支持动态插入一条线段，询问从某一个位置 (x,+∞) 向下看能看到的最高的一条线段（也就是给一条竖线，问这条竖线与所有线段的最高的交点。

## 吉老师线段树|吉司机线段树

区间最值操作 & 区间历史最值

栗子:给出一个数列, 每次操作让某个区间中对给定值取 min 询问某个区间的和

## 树套树

在第一维线段树的每个结点建立第二维线段树

## K-D Tree | KDT

高效处理 k 维空间信息

## Link Cut Tree | LCT

---
## 树状数组
### 一维
[单点修改区间查询](https://www.luogu.org/problemnew/show/P3374)

[区间修改单点查询](https://www.luogu.org/problemnew/show/P3368)

{% spoiler "代码" %}
```cpp
template <typename T>
struct BinaryIndexedTree {
  int n;
  T tr[N];
  BinaryIndexedTree() { memset(tr, 0, sizeof tr); }
  void init(const int &_n) { n = _n; clear(); }
  void clear() { memset(tr+1, 0, sizeof(T)*n); }
  void add(const int &x, const T &v) { for (int i = x; i <= n; i += i&-i) tr[i] += v; }
  void add(const int &x, const int &y, const T &v) { add(x, v); add(y+1, -v); }
  T query(const int &x) { T res = 0; for (int i = x ; i; i -= i&-i) res += tr[i]; return res; }
  T query(const int &x, const int &y) { return query(y)-query(x-1); }
};
```

{% endspoiler %}

{% spoiler "代码" %}
```cpp
template <typename T>
struct BinaryIndexedTree {
  int n;
  vector<T> tr;
  void init(const int &_n) { tr = vector<T>(n+1, 0); }
  void add(const int &x, const T &v) { for (int i = x; i <= n; i += i&-i) tr[i] += v; }
  void add(const int &x, const int &y, const T &v) { add(x, v); add(y+1, -v); }
  T query(const int &x) { T res = 0; for (int i = x ; i; i -= i&-i) res += tr[i]; return res; }
  T query(const int &x, const int &y) { return query(y)-query(x-1); }
};
```

{% endspoiler %}

[O(n)初始化](http://codeforces.com/blog/entry/63064)
{% spoiler "代码" %}
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

{% endspoiler %}
### 二维
#### 单点修改区间查询
{% spoiler "代码" %}
```cpp
template <typename T>
struct BIT_2D {
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

{% endspoiler %}

## 可持久化线段树(可持久化数组)

{% spoiler "代码" %}
```cpp
template <typename T>
struct PersistantArray {
  static const int NN = N*(log2(N)+3);
  int rt[N], ls[NN], rs[NN], val[NN], tot, n;
  void build(const int &n) {
    this->n = n;
    tot = 0;
    rt[0] = build(1, n);
  }
  int build(const int &l, const int &r) {
    int cur = ++tot; assert(tot < NN);
    if (l == r) return val[cur] = a[l], cur;
    int mid = (l+r)>>1;
    ls[cur] = build(l, mid);
    rs[cur] = build(mid+1, r);
    return cur;
  }
  void update(const int &cur, const int &pre, const int &x, const T &k) {
    rt[cur] = update(rt[pre], x, k, 1, n);
  }
  int update(const int &pre, const int &x, const T &k, const int &l, const int &r) {
    int cur = ++tot; assert(tot < NN);
    if (l == x && r == x) return val[cur] = k, cur;
    ls[cur] = ls[pre]; rs[cur] = rs[pre];
    int mid = (l+r)>>1;
    if (x <= mid) ls[cur] = update(ls[pre], x, k, l, mid);
    else rs[cur] = update(rs[pre], x, k, mid+1, r);
    return cur;
  }
  T query(const int &cur, const int &x) {
    return query(rt[cur], x, 1, n);
  }
  T query(const int &cur, const int &x, const int &l, const int &r) {
    if (l == x && r == x) return val[cur];
    int mid = (l+r)>>1;
    if (x <= mid) return query(ls[cur], x, l, mid);
    return query(rs[cur], x, mid+1, r);
  }
};
```

{% endspoiler %}

## 可持久化并查集

{% spoiler "代码" %}
```cpp
struct PersistantUnionSet {
  static const int NN = N*(log2(N)+3);
  int rt[N], ls[NN], rs[NN], fa[NN], dep[NN], n, tot;
  void build(const int &n) {
    this->n = n;
    tot = 0;
    rt[0] = build(1, n);
  }
  int build(const int &l, const int &r) {
    int cur = ++tot; assert(tot < NN);
    if (l == r) return fa[cur] = l, dep[cur] = 0, cur;
    int mid = (l+r)>>1;
    ls[cur] = build(l, mid);
    rs[cur] = build(mid+1, r);
    return cur;
  }
  bool query(const int &cur, const int &x, const int &y) {
    return fa[getf(rt[cur], x)] == fa[getf(rt[cur], y)];
  }
  // return the id of fa[], dep[]
  int query(const int &cur, const int &x, const int &l, const int &r) {
    if (l == r) return cur;
    int mid = (l+r)>>1;
    if (x <= mid) return query(ls[cur], x, l, mid);
    else return query(rs[cur], x, mid+1, r);
  }
  // return the id of fa[], dep[]
  int getf(const int &cur, int x) {
    int fi;
    while (fa[(fi = query(cur, x, 1, n))] != x) x = fa[fi];
    return fi;
  }
  void merge(const int &cur, const int &pre, const int &x, const int &y) {
    rt[cur] = rt[pre];
    int fx = getf(rt[cur], x), fy = getf(rt[cur], y);
    if (fa[fx] == fa[fy]) return;
    if (dep[fx] > dep[fy]) swap(fx, fy);
    rt[cur] = update(rt[pre], fa[fx], fa[fy], 1, n);
    if (dep[fx] == dep[fy]) add(rt[cur], fa[fy], 1, n);
  }
  // update fa, merge x to y
  int update(const int &pre, const int &x, const int &y, const int &l, const int &r) {
    int cur = ++tot; assert(tot < NN);
    if (l == r) return fa[cur] = y, dep[cur] = dep[pre], cur;
    ls[cur] = ls[pre]; rs[cur] = rs[pre];
    int mid = (l+r)>>1;
    if (x <= mid) ls[cur] = update(ls[pre], x, y, l, mid);
    else rs[cur] = update(rs[pre], x, y, mid+1, r);
    return cur;
  }
  // add dep
  void add(const int &cur, const int &x, const int &l, const int &r) {
    if (l == r) return ++dep[cur], void();
    int mid = (l+r)>>1;
    if (x <= mid) add(ls[cur], x, l, mid);
    else add(rs[cur], x, mid+1, r);
  }
};
```

{% endspoiler %}

## [可持久化线段树(主席树)](https://www.luogu.com.cn/problem/P3834)

自带离散

{% spoiler "代码" %}
```cpp
template <typename T>
struct PersistantSegmentTree {
  static const int NN = N*(log2(N)+5);
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
    sum[cur] = 0;
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

{% endspoiler %}

不带离散

{% spoiler "代码" %}
```cpp
template <typename T>
struct PersistantSegmentTree {
  static const int NN = N*(log2(N)+5);
  int rt[N], sum[NN], ls[NN], rs[NN], tot, n;
  void build(const int &n) {
    this->n = n;
    tot = 0;
    rt[0] = _build(1, n);
  }
  void update(const int &cur, const int &pre, const T &k) {
    rt[cur] = _update(rt[pre], 1, n, k);
  }
  T query(const int &l, const int &r, const int &k) {
    return _query(rt[l-1], rt[r], 1, n, k);
  }
private:
  int _build(const int &l, const int &r) {
    int cur = ++tot;
    sum[cur] = 0;
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

{% endspoiler %}

## [分块](http://hzwer.com/8053.html)
[例题](https://loj.ac/problems/search?keyword=%E5%88%86%E5%9D%97)
{% spoiler "代码" %}
```cpp
struct FenKuai {
  typedef long long T;
  int t; // 每组大小
  static const int NN = static_cast<int>(sqrt(N))+7;
  T a[N], sum[NN], add[NN];
  FenKuai() {
    memset(a, 0, sizeof a);
    memset(sum, 0, sizeof sum);
    memset(add, 0, sizeof add);
  }
  void init() {
    t = static_cast<int>(sqrt(n)+0.5);
    for (int i = 0; i < n; ++i) sum[i/t] += a[i];
  }
  void update(int x, T k) { a[x] += k; sum[x/t] += k; }
  void update(int x, int y, T k) {
    for ( ; x <= y && x%t; ++x) a[x] += k, sum[x/t] += k;
    for ( ; x+t-1 <= y; x += t) sum[x/t] += k*t, add[x/t] += k;
    for ( ; x <= y; ++x) a[x] += k, sum[x/t] += k;
  }
  T query(int x) { return a[x]+add[x/t]; }
  T query(int x, int y) {
    T res = 0;
    for ( ; x <= y && x%t; ++x) res += a[x]+add[x/t];
    for ( ; x+t-1 <= y; x += t) res += sum[x/t];
    for ( ; x <= y; ++x) res += a[x]+add[x/t];
    return res;
  }
} B;
```

{% endspoiler %}
{% spoiler "代码" %}
```cpp
struct FenKuai {
  typedef int T;
  int t; // 每组大小
  T a[N], b[N], add[N];
  FenKuai() {
    memset(a, 0, sizeof a);
    memset(b, 0, sizeof b);
    memset(add, 0, sizeof add);
  }
  void build(int x) {
    for (int i = x*t; i < min(x*t+t, n); ++i) b[i] = a[i];
    sort(b+x*t, b+min(x*t+t, n));
  }
  void init() {
    t = static_cast<int>(sqrt(n)+0.5);
    for (int i = 0; i*t < n; ++i) build(i);
  }
  void update(int x, int y, T c) {
    int i = x;
    for ( ; i <= y && i%t; ++i) a[i] += c;
    build(x/t);
    for ( ; i+t-1 <= y; i += t) add[i/t] += c;
    for ( ; i <= y; ++i) a[i] += c;
    build(y/t);
  }
  T query(int x, int y, long long c) {
    T res = 0; int i = x;
    for ( ; i <= y && i%t; ++i) res += (a[i]+add[i/t] < c*c);
    for ( ; i+t-1 <= y; i += t) res += lower_bound(b+i, b+i+t, c*c-add[i/t])-(b+i);
    for ( ; i <= y; ++i) res += (a[i]+add[i/t] < c*c);
    return res;
  }
} B;
```

{% endspoiler %}
{% spoiler "代码" %}
```cpp
struct FenKuai {
  typedef int T;
  int t, sz;
  static const int NN = static_cast<int>(sqrt(N))+7;
  T a[N];
  deque<int> q[NN];
  void init(int _n) {
    sz = _n;
    t = static_cast<int>(sqrt(sz*1.5)+0.5);
    for (int i = 0; i < sz; ++i) q[i/t].push_back(a[i]);
  }
  void update(int x, int k) {
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
  T query(int x) {
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

{% endspoiler %}
## 莫队
$O(1)修改$ 一般取 $block = \frac{n}{\sqrt{m} }, O(n\sqrt{m})$

移动前两步先扩大区间 $l--,r++$ 后两步缩小区间 $l++,r--$

### 奇偶性排序
{% spoiler "代码" %}
```cpp
template <typename T> bool cmp(const T &q1, const T &q2) {
  return q1.l/block != q2.l/block ? q1.l < q2.l :
      (q1.l/block)&1 ? q1.r < q2.r : q1.r > q2.r;
}
```

{% endspoiler %}

### 带修改莫队

以 $n^{\frac{2}{3} }$ 为一块，分成了 $n^{\frac{1}{3} }$ 块，第一关键字是左端点所在块，第二关键字是右端点所在块，第三关键字是时间. 复杂度 $O(n^{\frac{5}{3} })$

{% spoiler "代码" %}
```cpp
template <typename T> bool cmp(const T &q1, const T &q2) {
  return q1.l/block != q2.l/block ? q1.l < q2.l :
      q1.r/block != q2.r/block ? q1.r < q2.r : q1.t < q2.t;
}
```

{% endspoiler %}

### 值域分块

维护块的前缀和以及块内部前缀和, $O(\sqrt{n})$ 修改, $O(1)$ 求区间和

{% spoiler "代码" %}
```cpp
template <typename T>
struct PreSum {
  int n, block;
  T s[N], t[(int)sqrt(N)+3];
  void init(int n) {
    this->n = n;
    block = sqrt(n);
  }
  void add(int x, T k) {
    for (int i = x; i/block == x/block && i <= n; ++i) s[i] += k;
    for (int i = x/block+1; i <= n/block; ++i) t[i] += k;
  }
  T query(int x) {
    return t[x/block]+s[x];
  }
};

template <typename T>
struct SufSum {
  int n, block;
  T s[N], t[(int)sqrt(N)+3];
  void init(int n) {
    this->n = n;
    block = sqrt(n);
  }
  void add(int x, T k) {
    for (int i = x; i/block == x/block && i >= 1; --i) s[i] += k;
    for (int i = x/block-1; i >= 0; --i) t[i] += k;
  }
  T query(int x) {
    return t[x/block]+s[x];
  }
};
```

{% endspoiler %}

### 二次离线莫队
大概是一种需要维护信息具有可减性的莫队。只要具可减性，就可以容斥，就可以二次离线。所谓『二次离线』，大概是指由于普通莫队无法快速计算贡献，所以第一次离线把询问离线下来，第二次离线把莫队的转移过程离线下来。

由于信息具有可减性(比如常见的「点对数」)，记 $(a,b)(c,d)$ 表示区间 $[a,b]$ 内的点和区间 $[c,d]$ 内的点对彼此产生的贡献(区间内部不算)。

$[l,r]\to[l+t,r],\sum\limits_{i=l}^{l+t−1}(i,i)(i+1,r)=\sum\limits_{i=l}^{l+t−1}(i,i)(1,r)−(i,i)(1,i)$

$[l,r]\to[l-t,r],\sum\limits_{i=l-t}^{l-1}(i,i)(i+1,r)=\sum\limits_{i=l-t}^{l-1}(i,i)(1,r)−(i,i)(1,i)$

$[l,r]\to[l,r+t],\sum\limits_{i=r+1}^{r+t}(i,i)(l,i-1)=\sum\limits_{i=r+1}^{r+t}(1,i-1)(i,i)-(1,l-1)(i,i)$

$[l,r]\to[l,r-t],\sum\limits_{i=r-t+1}^{r}(i,i)(l,i-1)=\sum\limits_{i=r-t+1}^{r}(1,i-1)(i,i)-(1,l-1)(i,i)$


对于 $(1,i-1)(i,i)$ 没什么好说,暴力处理前缀和

对于 $(1,l-1)(i,i)$ 由于莫队的复杂度,至多有 $n\sqrt{m}$ 个不同询问,把每个询问 打标记到左端点(比如 $[l,r]\to [l,r-t]$ 就打到 $l-1$ 上), 最后扫一遍全部 $i \in [1,n]$ ,处理出询问值, 因为此时 $i$ 枚举 $O(n)$ 次,可以用『值域分块』技巧。这样最终复杂度 $O(n\sqrt n+n\sqrt{n})$

[求区间逆序对](https://www.luogu.com.cn/problem/P5047)

{% spoiler "代码" %}
```cpp
struct Query {
  int id, l, r;
  Query() {}
  Query(int i, int _l, int _r) : id(i), l(_l), r(_r) {}
};

int n, m, block;
int a[N];
long long res[N], sumil[N], sumir[N], ans[N];
Query q[N];
SufSum<int> suml;
PreSum<int> sumr;
vector<Query> ql[N], qr[N];

inline void calc_sumi() {
  static BinaryIndexedTree<int> tree;
  tree.init(n);
  for (int i = 1; i <= n; ++i) {
    sumil[i] = sumil[i-1]+i-1-tree.query(a[i]);
    tree.add(a[i], 1);
  }
  tree.clear();
  for (int i = n; i; --i) {
    sumir[i] = sumir[i+1]+tree.query(a[i]-1);
    tree.add(a[i], 1);
  }
}

signed main() {
  read(n); read(m);
  for (int i = 1; i <= n; ++i) read(a[i]);
  discrete();
  block = n/sqrt(m);
  for (int i = 1; i <= m; ++i) {
    q[i].id = i;
    read(q[i].l);
    read(q[i].r);
  }
  sort(q+1, q+m+1, cmp);
  calc_sumi();
  q[0] = Query(0, 1, 0);
  for (int i = 1, ul, vl, ur, vr; i <= m; ++i) {
    ul = q[i-1].l; ur = q[i-1].r;
    vl = q[i].l; vr = q[i].r;
    res[i] = sumil[vr]-sumil[ur]+sumir[vl]-sumir[ul];
    if (vl < ul) qr[vr+1].emplace_back(-i, vl, ul-1);
    if (vl > ul) qr[vr+1].emplace_back(+i, ul, vl-1);
    if (vr < ur) ql[ul-1].emplace_back(+i, vr+1, ur);
    if (vr > ur) ql[ul-1].emplace_back(-i, ur+1, vr);
  }
  suml.init(n+1);
  for (int i = 1; i <= n; ++i) {
    suml.add(a[i], 1);
    for (auto &qq : ql[i]) {
      for (int j = qq.l; j <= qq.r; ++j) {
        if (qq.id > 0) res[qq.id] += suml.query(a[j]+1);
        else res[-qq.id] -= suml.query(a[j]+1);
      }
    }
  }
  sumr.init(n);
  for (int i = n; i; --i) {
    sumr.add(a[i], 1);
    for (auto &qq : qr[i]) {
      for (int j = qq.l; j <= qq.r; ++j) {
        if (qq.id > 0) res[qq.id] += sumr.query(a[j]-1);
        else res[-qq.id] -= sumr.query(a[j]-1);
      }
    }
  }
  for (int i = 1; i <= m; ++i) {
    res[i] += res[i-1];
    ans[q[i].id] = res[i];
  }
  for (int i = 1; i <= m; ++i) write(ans[i]), putchar('\n');
  return 0;
}
```

{% endspoiler %}

## ST表
### [一维](https://www.luogu.org/problemnew/show/P3865)
{% spoiler "代码" %}
```cpp
template <typename T, typename U = std::greater<T>>
struct ST {
  static const int NN = (int)log2(N)+3;
  static const T INF = 1e9;
  int lg2[N];
  U cmp = U();
  T rmq[N][NN];
  ST() {
    fill(rmq[0], rmq[0]+N*NN, cmp(-INF, +INF) ? INF : -INF);
    for (int i = 2; i < N; ++i) lg2[i] = lg2[i>>1]+1;
  }
  T& operator [] (const int &i) { return rmq[i][0]; }
  void init(const T &val = 0) { fill(rmq[0], rmq[0]+N*NN, val); }
  T mv(const T &x, const T &y) { return cmp(x, y) ? x : y; }
  // rmq[i][j] ==> [i, i+2^j-1]
  void build(T a[], const int &n) {
    for (int i = n; i; --i) {
      rmq[i][0] = a[i];
      for (int j = 1; j <= lg2[n-i+1]; ++j)
        rmq[i][j] =  mv(rmq[i][j-1], rmq[i+(1<<(j-1))][j-1]);
    }
  }
  T query(const int &l, const int &r) {
    if (l > r) return query(r, l);
    int k = lg2[r-l+1];
    return mv(rmq[l][k], rmq[r-(1<<k)+1][k]);
  }
};
  /* rmq[i][j] ==> [i-2^j+1, i]
  void build(T a[], const int &n) {
    for (int i = 1; i <= n; ++i) {
      rmq[i][0] = a[i];
      for (int j = 1; j <= lg2[i]; ++j)
        rmq[i][j] =  mv(rmq[i][j-1], rmq[i-(1<<(j-1))][j-1]);
    }
  }
  T query(const int &l, const int &r) {
    if (l > r) return query(r, l);
    int k = lg2[r-l+1];
    return mv(rmq[r][k], rmq[l+(1<<k)-1][k]);
  }
  */
```

{% endspoiler %}

### 二维
$O(nm\log n \log m)$

{% spoiler "代码" %}
```cpp
template <typename T, typename U = std::greater<T>>
struct ST {
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

{% endspoiler %}

### [反向ST](http://acm.hdu.edu.cn/showproblem.php?pid=6356)
{% spoiler "代码" %}
```cpp
template <typename T, typename U = std::greater<T>>
struct rST {
  static const int NN = (int)log2(N)+3;
  static const T INF = 1e9;
  int n;
  int lg2[N];
  U cmp = U();
  T rmq[N][NN]; // rmq[i][j] ==> [i, i+2^j-1]
  rST() { for (int i = 2; i < N; ++i) lg2[i] = lg2[i>>1]+1; }
  T& operator [] (const int &i) { return rmq[i][0]; }
  T mv(const T &x, const T &y) { return cmp(x, y) ? x : y; }
  void init(const int &_n, const T &val = 0) { 
    n = _n;
    for (int i = 1; i <= n; ++i) fill(rmq[i], rmq[i]+NN, val);
  }
  void update(const int &l, const int &r, const T &k) {
    if (l > r) return void(update(r, l, k));
    int b = lg2[r-l+1];
    rmq[l][b] = mv(rmq[l][b], k);
    rmq[r-(1<<b)+1][b] = mv(rmq[r-(1<<b)+1][b], k);
  }
  void build() {
    for (int i = lg2[n]; i >= 0; --i) {
      for (int l = 1, r; l <= n; ++l) {
        r = l+(1<<i);
        if (r <= n) rmq[r][i] = mv(rmq[r][i], rmq[l][i+1]);
        rmq[l][i] = mv(rmq[l][i], rmq[l][i+1]);
      }
    }
  }
  T query(const int &l, const int &r) {
    if (l > r) return query(r, l);
    int b = lg2[r-l+1];
    return mv(rmq[l][b], rmq[r-(1<<b)+1][b]);
  }
};
```

{% endspoiler %}

---
## [并查集](https://www.luogu.org/problemnew/show/P3367)
{% spoiler "代码" %}
```cpp
struct DSU {
  int fa[N];
  void init(int n) { iota(fa, fa+n+1, 0); }
  int get(int s) { return s == fa[s] ? s : fa[s] = get(fa[s]); }
  int& operator [] (int i) { return fa[get(i)]; }
  bool merge(int x, int y) { // merge x to y
    int fx = get(x), fy = get(y);
    if (fx == fy) return false;
    fa[fx] = fy; return true;
  }
};
```

{% endspoiler %}
加上数量
{% spoiler "代码" %}
```cpp
struct DSU {
  int fa[N], num[N];
  void init(int n) { for (int i = 0; i <= n; ++i) fa[i] = i, num[i] = 1; }
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

{% endspoiler %}

---
## 单调队列
{% spoiler "代码" %}
```cpp
template <typename T>
struct MonotonousQueue {
  unsigned head;
  vector<T> q;
  MonotonousQueue() { clear(); }
  T& operator [](const int &i) { return q[head+i];}
  void clear() { head = 0; q.clear(); }
  size_t size() { return q.size()-head; }
  bool empty() { return head == q.size(); }
  T front() { return q[head]; }
  T back() { return q.back(); }
  void push_front(const T &x) {/*unsupported*/}
  void push_back(const T &x) { q.push_back(x); }
  void pop_front() { ++head; }
  void pop_back() { q.pop_back(); }
};
```

{% endspoiler %}
---
# 字符串
## [回文字符串|manacher算法](https://www.luogu.org/problemnew/show/P3805)
从 0 开始，第 i 位对应 p[i*2+2]
{% spoiler "代码" %}
```cpp
inline int manacher(const char *str, char *buf, int *p) {
  int str_len = strlen(str), buf_len = 2;
  buf[0] = buf[1] = '#';
  for(int i = 0; i < str_len; ++i)
    buf[buf_len++] = str[i], buf[buf_len++] = '#';

  int mx = 0, id, ans = 0;
  for(int i = 1; i < buf_len; ++i) {
    if(i <= mx) p[i] = min(p[id*2-i], mx-i);
    else p[i] = 1;
    while(buf[i-p[i]] == buf[i+p[i]]) p[i]++;
    if(i+p[i] > mx) mx = i+p[i], id = i;
    ans = max(ans, p[i]-1);
  }
  return ans;
}
```

{% endspoiler %}
### 判断s[l, r]是否为回文
{% spoiler "代码" %}
```cpp
p[l+r+2]-1 >= r-l+1
```

{% endspoiler %}

---
## [KMP](https://www.luogu.org/problemnew/show/P3375)
`.c_str() 未知异常 Segment Fault`
{% spoiler "代码" %}
```cpp
inline void get_next(const string &s, int nex[]) { get_next(s.c_str(), nex); }
inline void get_next(const char *s, int nex[]) {
  nex[0] = nex[1] = 0;
  for (int i = 1, j = 0, l = strlen(s); i < l; ++i) {
    while (j && s[i] != s[j]) j = nex[j];
    nex[i+1] = s[i] == s[j] ? ++j : 0;
  }
}

inline void kmp(const string &s1, const string &s2, int nex[]) { kmp(s1.c_str(), s2.c_str(), nex); }
inline void kmp(const char *s1, const char *s2, int nex[]) {
  for (int i = 0, j = 0, l1 = strlen(s1), l2 = strlen(s2); i < l1; ++i){
    while (j && s1[i] != s2[j]) j = nex[j];
    if (s1[i] == s2[j]) ++j;
    if (j == l2) {
      cout << i-l2+2 << endl;
      j = nex[j];
    }
  }
}
```

{% endspoiler %}
{% spoiler "代码" %}
```cpp
inline void get_next(const string &s, int nex[]) {
  nex[0] = nex[1] = 0;
  for (int i = 1, j = 0; i < (int)s.size(); ++i) {
    while (j && s[i] != s[j]) j = nex[j];
    nex[i+1] = s[i] == s[j] ? ++j : 0;
  }
}

inline void kmp(const string &s1, const string &s2, int nex[]) {
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

{% endspoiler %}
## [扩展KMP|Z函数](https://subetter.com/algorithm/extended-kmp-algorithm.html)
{% spoiler "代码" %}
```cpp
inline void GetNext(char *s, int *_nex) {
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

inline void GetExtend(char *s, char *ss, int *_ext, int *_nex) {
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

{% endspoiler %}
## 最长公共前后缀
[hdu2594](http://acm.hdu.edu.cn/showproblem.php?pid=2594)
### Hash
{% spoiler "代码" %}
```cpp
string max_pre_suf(const string &s1, const string &s2) {
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

{% endspoiler %}

---
## BM算法
## Sunday算法
## [字符串哈希](https://www.luogu.org/problemnew/show/P3370)
{% spoiler "代码" %}
```cpp
struct StringHash {
  static const int M1 = 1e9+7;
  static const int M2 = 998244353;
  static const int P1 = 31;
  static const int P2 = 29;
  int ha1[N], ha2[N], pw1[N], pw2[N];
  StringHash() {
    pw1[0] = pw2[0] = 1;
    for (int i = 1; i < N; ++i) {
      pw1[i] = 1ll*pw1[i-1]*P1%M1;
      pw2[i] = 1ll*pw2[i-1]*P2%M2;
    }
  }
  void init(char *s, int len) {
    for (int i = 0; i < len; ++i) {
      ha1[i+1] = (1ll*ha1[i]*P1+s[i]-'a'+1)%M1;
      ha2[i+1] = (1ll*ha2[i]*P2+s[i]-'a'+1)%M2;
    }
  }
  pair<int, int> get(int l, int r) {
    return {(ha1[r]-1ll*ha1[l-1]*pw1[r-l+1]%M1+M1)%M1,
            (ha2[r]-1ll*ha2[l-1]*pw2[r-l+1]%M2+M2)%M2};
  }
};
```

{% endspoiler %}

---
## [后缀数组|SA](https://loj.ac/problem/111)
### $O(nlog^2n)$
{% spoiler "代码" %}
```cpp
int sa[N], rk[N<<1], height[N];
template <typename T> // s start from 1
inline void SA(const T *s, const int &n) {
  static int oldrk[N<<1];
  memset(rk+n+1, 0, sizeof(int)*n);
  for (int i = 1; i <= n; ++i) rk[i] = s[i];
  for (int w = 1; w <= n; w <<= 1) {
    iota(sa+1, sa+n+1, 1);
    sort(sa+1, sa+n+1, [&](const int &x, const int &y) {
      return rk[x] == rk[y] ? rk[x+w] < rk[y+w] : rk[x] < rk[y];
    });
    memcpy(oldrk+1, rk+1, sizeof(int)*2*n);
    for (int p = 0, i = 1; i <= n; ++i) {
      if (oldrk[sa[i]] == oldrk[sa[i-1]] &&
        oldrk[sa[i]+w] == oldrk[sa[i-1]+w]) {
        rk[sa[i]] = p;
      } else {
        rk[sa[i]] = ++p;
      }
    }
  }
  for (int i = 1, k = 0; i <= n; ++i) {
    if (k) --k;
    while (s[i+k] == s[sa[rk[i]-1]+k]) ++k;
    height[rk[i]] = k;
  }
}
```

{% endspoiler %}

### $O(nlogn)$
{% spoiler "代码" %}
```cpp
int sa[N], rk[N<<1], height[N];
template <typename T> // s start from 1
inline void SA(const T *s, const int &n) {
#define cmp(x, y, w) oldrk[x] == oldrk[y] && oldrk[x + w] == oldrk[y + w]
  static int oldrk[N<<1], id[N], px[N], cnt[N], m;
  memset(cnt, 0, sizeof(int) * (m = 128));
  for (int i = 1; i <= n; ++i) ++cnt[rk[i] = s[i]];
  for (int i = 1; i <= m; ++i) cnt[i] += cnt[i - 1];
  for (int i = n; i; --i) sa[cnt[rk[i]]--] = i;
  for (int w = 1, p, i; w <= n; w <<= 1, m = p) {
    for (p = 0, i = n; i > n - w; --i) id[++p] = i;
    for (int i = 1; i <= n; ++i)
      if (sa[i] > w)
        id[++p] = sa[i] - w;
    memset(cnt + 1, 0, sizeof(int) * m);
    for (int i = 1; i <= n; ++i) ++cnt[px[i] = rk[id[i]]];
    for (int i = 1; i <= m; ++i) cnt[i] += cnt[i - 1];
    for (int i = n; i; --i) sa[cnt[px[i]]--] = id[i];
    memcpy(oldrk + 1, rk + 1, sizeof(int) * 2 * n);
    for (p = 0, i = 1; i <= n; ++i) rk[sa[i]] = cmp(sa[i], sa[i - 1], w) ? p : ++p;
  }
  for (int i = 1, k = 0; i <= n; ++i) {
    if (k) --k;
    while (s[i+k] == s[sa[rk[i]-1]+k]) ++k;
    height[rk[i]] = k;
  }
#undef cmp
}
```

{% endspoiler %}

### [$O(n)$](https://loj.ac/submission/653573)
{% spoiler "代码" %}
```cpp
namespace SuffixArray {

int sa[N], rk[N], ht[N];
bool t[N << 1];

inline bool islms(const int i, const bool *t) { return i > 0 && t[i] && !t[i - 1]; }

template <class T>
inline void sort(T s, int *sa, const int len, const int sz, const int sigma, bool *t, int *b, int *cb, int *p) {
  memset(b, 0, sizeof(int) * sigma);
  memset(sa, -1, sizeof(int) * len);
  for (register int i = 0; i < len; i++) b[static_cast<int>(s[i])]++;
  cb[0] = b[0];
  for (register int i = 1; i < sigma; i++) cb[i] = cb[i - 1] + b[i];
  for (register int i = sz - 1; i >= 0; i--) sa[--cb[static_cast<int>(s[p[i]])]] = p[i];
  for (register int i = 1; i < sigma; i++) cb[i] = cb[i - 1] + b[i - 1];
  for (register int i = 0; i < len; i++)
    if (sa[i] > 0 && !t[sa[i] - 1])
      sa[cb[static_cast<int>(s[sa[i] - 1])]++] = sa[i] - 1;
  cb[0] = b[0];
  for (register int i = 1; i < sigma; i++) cb[i] = cb[i - 1] + b[i];
  for (register int i = len - 1; i >= 0; i--)
    if (sa[i] > 0 && t[sa[i] - 1])
      sa[--cb[static_cast<int>(s[sa[i] - 1])]] = sa[i] - 1;
}

template <class T>
inline void sais(T s, int *sa, const int len, bool *t, int *b, int *b1, const int sigma) {
  register int i, j, x, p = -1, cnt = 0, sz = 0, *cb = b + sigma;
  for (t[len - 1] = 1, i = len - 2; i >= 0; i--) t[i] = s[i] < s[i + 1] || (s[i] == s[i + 1] && t[i + 1]);
  for (i = 1; i < len; i++)
    if (t[i] && !t[i - 1])
      b1[sz++] = i;
  sort(s, sa, len, sz, sigma, t, b, cb, b1);
  for (i = sz = 0; i < len; i++)
    if (islms(sa[i], t))
      sa[sz++] = sa[i];
  for (i = sz; i < len; i++) sa[i] = -1;
  for (i = 0; i < sz; i++) {
    for (x = sa[i], j = 0; j < len; j++) {
      if (p == -1 || s[x + j] != s[p + j] || t[x + j] != t[p + j]) {
        cnt++, p = x;
        break;
      } else if (j > 0 && (islms(x + j, t) || islms(p + j, t))) {
        break;
      }
    }
    sa[sz + (x >>= 1)] = cnt - 1;
  }
  for (i = j = len - 1; i >= sz; i--)
    if (sa[i] >= 0)
      sa[j--] = sa[i];
  register int *s1 = sa + len - sz, *b2 = b1 + sz;
  if (cnt < sz)
    sais(s1, sa, sz, t + len, b, b1 + sz, cnt);
  else
    for (i = 0; i < sz; i++) sa[s1[i]] = i;
  for (i = 0; i < sz; i++) b2[i] = b1[sa[i]];
  sort(s, sa, len, sz, sigma, t, b, cb, b2);
}

template <class T>
inline void getHeight(T s, int n) {
  for (register int i = 1; i <= n; i++) rk[sa[i]] = i;
  register int j = 0, k = 0;
  for (register int i = 0; i < n; ht[rk[i++]] = k)
    for (k ? k-- : 0, j = sa[rk[i] - 1]; s[i + k] == s[j + k]; k++)
      ;
}

template <class T>  // s start from 0
inline void init(T s, const int len, const int sigma = 128) {
  sais(s, sa, len + 1, t, rk, ht, sigma);
  getHeight(s, len);
  for (int i = 1; i <= len; ++i) ++sa[i];
  for (int i = len; i; --i) rk[i] = rk[i-1];
}

}  // namespace SuffixArray
```

{% endspoiler %}

## 字典树
{% spoiler "代码" %}
```cpp
struct TireTree {
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

{% endspoiler %}

## AC自动机
[Luogu P3808](https://www.luogu.com.cn/problem/P3808)
如需构造可重建AC自动机,每次构造建一个nex数组的拷贝
{% spoiler "代码" %}
```cpp
struct Aho_Corasick_Automaton {
  static const int NN = 5e6+7;
  static const int SZ = 26;
  char beg;
  int nex[NN][SZ], num[NN], fail[NN], cnt;
  Aho_Corasick_Automaton(const char &_beg = 'a') : beg(_beg) {}
  void clear() {
    memset(nex, 0, sizeof(nex[0])*(cnt+1));
    memset(num, 0, sizeof(int)*(cnt+1));
    memset(fail, 0, sizeof(int)*(cnt+1));
    cnt = 0;
  }
  void insert(const char *s) {
    int len = strlen(s), p = 0;
    for (int i = 0, c; i < len; ++i) {
      c = s[i]-beg;
      if (!nex[p][c]) nex[p][c] = ++cnt;
      p = nex[p][c];
    }
    ++num[p];
  }
  void build() {
    static queue<int> q;
    for (int i = 0; i < SZ; ++i) if (nex[0][i]) q.push(nex[0][i]);
    while (q.size()) {
      int u = q.front();
      q.pop();
      for (int i = 0; i < SZ; ++i) {
        if (nex[u][i]) {
          fail[nex[u][i]] = nex[fail[u]][i];
          q.push(nex[u][i]);
        } else {
          nex[u][i] = nex[fail[u]][i];
        }
      }
    }
  }
  int query(const char *s) {
    int len = strlen(s), p = 0, res = 0;
    for (int i = 0; i < len; ++i) {
      p = nex[p][s[i]-beg];
      for (int t = p; t && ~num[t]; t = fail[t]) {
        res += num[t];
        num[t] = -1;
      }
    }
    return res;
  }
};
```

{% endspoiler %}

[Luogu P5357](https://www.luogu.com.cn/problem/P5357)
{% spoiler "代码" %}
```cpp
struct Aho_Corasick_Automaton {
  static const int NN = 2e5+7;
  static const int SZ = 26;
  char beg;
  int cnt;
  int nex[NN][SZ], fail[NN], vis[NN];
  Aho_Corasick_Automaton(const char &_beg = 'a') : beg(_beg) {}
  void clear() {
    memset(nex, 0, sizeof(nex[0])*(cnt+1));
    memset(fail, 0, sizeof(int)*(cnt+1));
    memset(vis, 0, sizeof(int)*(cnt+1));
    cnt = 0;
  }
  int insert(const char *s) {
    int len = strlen(s), p = 0;
    for (int i = 0, c; i < len; ++i) {
      c = s[i]-beg;
      if (!nex[p][c]) nex[p][c] = ++cnt;
      p = nex[p][c];
    }
    return p;
  }
  void build() {
    static queue<int> q;
    for (int i = 0; i < SZ; ++i) if (nex[0][i]) q.push(nex[0][i]);
    while (q.size()) {
      int u = q.front();
      q.pop();
      for (int i = 0; i < SZ; ++i) {
        if (nex[u][i]) {
          fail[nex[u][i]] = nex[fail[u]][i];
          q.push(nex[u][i]);
        } else {
          nex[u][i] = nex[fail[u]][i];
        }
      }
    }
  }
  void query(char *s) {
    static int deg[NN];
    static queue<int> q;

    int len = strlen(s);
    for (int i = 0, p = 0; i < len; ++i) {
      p = nex[p][s[i]-beg];
      ++vis[p];
      // for (int t = p; t; t = fail[t]) ++vis[t];
    }
    for (int i = 1; i <= cnt; ++i) ++deg[fail[i]];
    for (int i = 1; i <= cnt; ++i) if (!deg[i]) q.push(i);
    while (q.size()) {
      int u = q.front();
      q.pop();
      vis[fail[u]] += vis[u];
      if (--deg[fail[u]] == 0) q.push(fail[u]);
    }
  }
} ac;
```

{% endspoiler %}

---
# 图论|树论
## [DFS树](https://codeforces.com/blog/entry/68138)
## [树的重心](https://www.luogu.org/problemnew/show/P2986)
{% spoiler "代码" %}
```cpp
void treedp(int cur, int fa) {
  s[cur] = c[cur];
  for(int i = fir[cur]; i; i = nex[i]) {
    if(e[i] == fa) continue;
    treedp(e[i], cur);
    s[cur] += s[e[i]];
    maxs[cur] = max(maxs[cur], s[e[i]]);
  }
  maxs[cur] = max(maxs[cur], sum-s[cur]);
}
```

{% endspoiler %}
## 最大团
最大独立集数=补图的最大团
{% spoiler "代码" %}
```cpp
struct MaxClique {
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

{% endspoiler %}

---
## 稳定婚姻匹配
{% spoiler "代码" %}
```cpp
template <typename T = int> struct Stable_Marriage {
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
      //   [&](const int &x, const int &y) { return wg[i][y] < wg[i][x]; });
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

{% endspoiler %}

---
## [最小生成树](https://www.luogu.org/problemnew/show/P3366)
[Prim](https://www.luogu.org/problemnew/show/P1265)
{% spoiler "代码" %}
```cpp
inline void prim() {
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

{% endspoiler %}
Kruskal (略)


---
## 二分图
### [二分图匹配](https://www.luogu.org/problemnew/show/P3386)
匈牙利算法
{% spoiler "代码" %}
```cpp
bool check(int u) {
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

inline int solve() {
  int res = 0;
  memset(co, 0, sizeof co);
  for (int i = 1; i <= n; ++i) {
    memset(vis, 0, sizeof(int)*(n+3));
    res += check(i);
  }
  return res;
}
```

{% endspoiler %}
### 二分图最小顶点覆盖
定义：假如选了一个点就相当于覆盖了以它为端点的所有边。最小顶点覆盖就是选择最少的点来覆盖所有的边。

定理：最小顶点覆盖等于二分图的最大匹配。
### 最大独立集

定义：选出一些顶点使得这些顶点两两不相邻，则这些点构成的集合称为独立集。找出一个包含顶点数最多的独立集称为最大独立集。

定理：最大独立集 = 所有顶点数 - 最小顶点覆盖 = 所有顶点数 - 最大匹配


---
## [最近公共祖先|LCA](https://www.luogu.org/problemnew/show/P3379)
### 倍增
{% spoiler "代码" %}
```cpp
struct LCA {
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

{% endspoiler %}

### 树剖
{% spoiler "代码" %}
```cpp
struct HLD {
  int dfn;
  int fa[N], d[N], num[N], son[N], id[N], tp[N];
  vector<int> *e;
  template <typename E>
  void build(E *_e, const int &rt = 1) {
    e = _e;
    fa[rt] = dfn = 0;
    dfs1(rt);
    dfs2(rt);
  }
  void dfs1(const int &u = 1) {
    d[u] = d[fa[u]]+1;
    num[u] = 1;
    son[u] = 0;
    for (const int &v : e[u]) if (v != fa[u]) {
      fa[v] = u;
      dfs1(v);
      num[u] += num[v];
      if (num[v] > num[son[u]]) son[u] = v;
    }
  }
  void dfs2(const int &u = 1) {
    tp[u] = son[fa[u]] == u ? tp[fa[u]] : u;
    id[u] = ++dfn;
    if (son[u]) dfs2(son[u]);
    for (const int &v : e[u]) if (v != son[u] && v != fa[u])
      dfs2(v);
  }
  int lca(int x, int y) {
    while (tp[x] != tp[y]) {
      if (d[tp[x]] < d[tp[y]]) swap(x, y);
      x = fa[tp[x]];
    }
    return d[x] < d[y] ? x : y;
  }
};
```

{% endspoiler %}

### [带权LCA](https://www.luogu.com.cn/problem/P1967)
{% spoiler "代码" %}
```cpp
template <typename T>
struct LCA {
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

{% endspoiler %}
## 最小环 

---
## [树上差分](https://www.luogu.com.cn/problem/P3128)
{% spoiler "代码" %}
```cpp
template <typename T>
struct Tree {
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

{% endspoiler %}
## [树链剖分](https://www.luogu.com.cn/problem/P3384)
{% spoiler "代码" %}
```cpp
template <typename T>
struct HLD {
  int dfn;
  int fa[N], d[N], num[N], son[N], id[N], tp[N];
  T init_val[N];
  SegmentTree<T> ST;
  template <typename Edge, typename TT>
  void build(const Edge e[], const TT a[], const int &n, const int &rt = 1) {
    fa[rt] = dfn = 0;
    dfs1(e, rt);
    dfs2(e, rt);
    for (int i = 1; i <= n; ++i)
      init_val[id[i]] = a[i];
    ST.build(init_val, n);
  }
  template <typename Edge>
  void dfs1(const Edge e[], const int &u = 1) {
    d[u] = d[fa[u]]+1;
    num[u] = 1;
    son[u] = 0;
    for (const int &v : e[u]) if (v != fa[u]) {
      fa[v] = u;
      dfs1(e, v);
      num[u] += num[v];
      if (num[v] > num[son[u]]) son[u] = v;
    }
  }
  template <typename Edge>
  void dfs2(const Edge e[], const int &u = 1) {
    tp[u] = son[fa[u]] == u ? tp[fa[u]] : u;
    id[u] = ++dfn;
    if (son[u]) dfs2(e, son[u]);
    for (const int &v : e[u]) if (v != son[u] && v != fa[u])
      dfs2(e, v);
  }
  void add_sons(const int &x, const T &k) { ST.add(id[x], id[x]+num[x]-1, k); }
  void add(int x, int y, const T &k, const int &is_edge = 0) {
    while (tp[x] != tp[y]) {
      if (d[tp[x]] < d[tp[y]]) swap(x, y);
      ST.add(id[tp[x]], id[x], k);
      x = fa[tp[x]];
    }
    if (d[x] > d[y]) swap(x, y);
    ST.add(id[x], id[y], k);
    if (is_edge) ST.add(id[x], -k);
  }
  T query_sons(const int &x) { return ST.query(id[x], id[x]+num[x]-1); }
  T query(const int &x) { return ST.query(id[x]); }
  T query(int x, int y) {
    T res = 0;
    while (tp[x] != tp[y]) {
      if (d[tp[x]] < d[tp[y]]) swap(x, y);
      res += ST.query(id[tp[x]], id[x]);
      x = fa[tp[x]];
    }
    if (d[x] > d[y]) swap(x, y);
    return res+ST.query(id[x], id[y]);
  }
};
```

{% endspoiler %}

---
## 网络流
[网络流24题](https://loj.ac/problems/tag/30)
### [最大流](https://www.luogu.org/problemnew/show/P3376)
#### EK
$O(nm^2)$
{% spoiler "代码" %}
```cpp
template <typename T>
struct EK {
  struct Edge {
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

{% endspoiler %}
#### Dinic
普通情况下 $O(n^2m)$
二分图中 $O(\sqrt{n}m)$
{% spoiler "代码" %}
```cpp
template <typename T>
struct Dinic {
  struct EDGE {
    int v, nex;
    T w;
    EDGE(const int &_v, const int &_nex, const T &_w) : v(_v), nex(_nex), w(_w) {}
  };
  vector<EDGE> e;
  int n, s, t;
  int fir[N], dep[N], cur[N];
  Dinic() { e.reserve(N<<2); }
  T work(const int &_s, const int &_t) {
    s = _s; t = _t;
    T maxflow = 0, flow;
    while (bfs())
      while ((flow = dfs(s, INF)))
        maxflow += flow;
    return maxflow;
  }
  void init(const int &_n) {
    n = _n;
    e.clear();
    memset(fir, -1, sizeof(int)*(n+3));
  }
  void add_edge(const int &u, const int &v, const T &w) {
    e.emplace_back(v, fir[u], w); fir[u] = e.size()-1;
    e.emplace_back(u, fir[v], 0); fir[v] = e.size()-1;
  }
  bool bfs() {
    queue<int> q;
    memset(dep, 0, sizeof(int)*(n+3));
    q.push(s);
    dep[s] = 1;
    for (int i = 0; i <= n; ++i) cur[i] = fir[i];
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
  T dfs(const int &u, const T &flow) {
    if (!flow || u == t) return flow;
    T rest = flow, now;
    for (int &i = cur[u], v; i != -1; i = e[i].nex) {
      v = e[i].v;
      if (dep[v] != dep[u]+1 || !e[i].w) continue;
      now = dfs(v, min(rest, e[i].w));
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

{% endspoiler %}
#### ISAP

渐进时间复杂度和dinic相同，但是非二分图的情况下isap更具优势
在某些情况(题目)中远慢于dinic

OI Wiki版本

{% spoiler "代码" %}
```cpp
template <typename T>
struct ISAP { // copy from oi-wiki
  struct Edge {
    int from, to;
    T cap, flow;
    Edge(int u, int v, T c, T f) : from(u), to(v), cap(c), flow(f) {}
    friend bool operator<(const Edge& a, const Edge& b) {
      return a.from < b.from || (a.from == b.from && a.to < b.to);
    }
  };
  int n, m, s, t;
  vector<Edge> edges;
  vector<int> G[N];
  bool vis[N];
  int d[N], cur[N], p[N], num[N];

  void add_edge(int from, int to, int cap) {
    edges.push_back(Edge(from, to, cap, 0));
    edges.push_back(Edge(to, from, 0, 0));
    m = edges.size();
    G[from].push_back(m - 2);
    G[to].push_back(m - 1);
  }

  bool BFS() {
    memset(vis, 0, sizeof(bool)*(n+3));
    queue<int> Q;
    Q.push(t);
    vis[t] = 1;
    d[t] = 0;
    while (!Q.empty()) {
      int x = Q.front();
      Q.pop();
      for (int i = 0; i < (int)G[x].size(); i++) {
        Edge& e = edges[G[x][i] ^ 1];
        if (!vis[e.from] && e.cap > e.flow) {
          vis[e.from] = 1;
          d[e.from] = d[x] + 1;
          Q.push(e.from);
        }
      }
    }
    return vis[s];
  }

  void init(int n) {
    this->n = n;
    for (int i = 1; i <= n; i++) G[i].clear();
    edges.clear();
  }

  T Augment() {
    int x = t;
    T flow = INF;
    while (x != s) {
      Edge& e = edges[p[x]];
      flow = min(flow, e.cap - e.flow);
      x = edges[p[x]].from;
    }
    x = t;
    while (x != s) {
      edges[p[x]].flow += flow;
      edges[p[x]^1].flow -= flow;
      x = edges[p[x]].from;
    }
    return flow;
  }

  T work(int s, int t) {
    this->s = s;
    this->t = t;
    T flow = 0;
    BFS();
    memset(num+1, 0, sizeof(int)*n);
    for (int i = 1; i <= n; i++) num[d[i]]++;
    int x = s;
    memset(cur+1, 0, sizeof(int)*n);
    while (d[s] < n) {
      if (x == t) {
        flow += Augment();
        x = s;
      }
      int ok = 0;
      for (int i = cur[x]; i < (int)G[x].size(); i++) {
        Edge& e = edges[G[x][i]];
        if (e.cap > e.flow && d[x] == d[e.to]+1) {
          ok = 1;
          p[e.to] = G[x][i];
          cur[x] = i;
          x = e.to;
          break;
        }
      }
      if (!ok) {
        int m = n-1;
        for (int i = 0; i < (int)G[x].size(); i++) {
          Edge& e = edges[G[x][i]];
          if (e.cap > e.flow) m = min(m, d[e.to]);
        }
        if (--num[d[x]] == 0) break;
        num[d[x] = m+1]++;
        cur[x] = 0;
        if (x != s) x = edges[p[x]].from;
      }
    }
    return flow;
  }
};
```

{% endspoiler %}

[Luogu版本](https://www.luogu.com.cn/blog/ONE-PIECE/jiu-ji-di-zui-tai-liu-suan-fa-isap-yu-hlpp)

{% spoiler "代码" %}
```cpp
template <typename T>
struct ISAP {
  struct EDGE {
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
  void clear() {
    for (int i = 0; i < (int)e.size(); i += 2) {
      e[i].w += e[i^1].w;
      e[i^1].w = 0;
    }
  }
  void modify_edge(const int &u, const int &v, const T &w) {
    for (int i = fir[u]; ~i; i = e[i].nex) if (e[i].v == v) {
      e[i].w = w;
      e[i^1].w = 0;
      break;
    }
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
    if (u == t) return maxflow += flow, flow;
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

{% endspoiler %}
#### HLPP
### 最小割
最小割等价最大流
### 费用流
#### MCMF
{% spoiler "代码" %}
```cpp
template <typename T>
struct MCMF {
  struct Edge {
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

{% endspoiler %}
#### 类Dinic
{% spoiler "代码" %}
```cpp
template <typename T>
struct Dinic {
  struct Edge {
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

{% endspoiler %}
#### Dijkstra
Primal-Dual 原始对偶算法
#### ZKW_SPFA
{% spoiler "代码" %}
```cpp
template <typename T>
struct ZKW_SPFA {
  struct Edge {
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
  ZKW_SPFA() { e.reserve(N<<4); }
  void init(const int &_n) {
    n = _n;
    maxflow = mincost = 0;
    e.clear();
    memset(fir, -1, sizeof(int)*(n+3));
  }
  void add_edge(const int &u, const int &v, const T &w = 1, const T &c = 0) {
    e.emplace_back(v, fir[u], w, c); fir[u] = e.size()-1;
    e.emplace_back(u, fir[v], 0, -c); fir[v] = e.size()-1;
  }
  pair<T, T> work(const int &_s, const int &_t) {
    s = _s; t = _t;
    while (spfa()) {
      vis[t] = 1;
      while (vis[t]) {
        memset(vis, 0, sizeof(int)*(n+3));
        maxflow += dfs(s, INF);
      }
    }
    return {maxflow, mincost};
  }
  private:
  bool spfa() {
    memset(dis, 0x3f, sizeof(T)*(n+3));
    memset(vis, 0, sizeof(int)*(n+3));
    deque<int> q;
    q.push_back(t);
    dis[t] = 0;
    vis[t] = 1;
    while (q.size()) {
      int u = q.front(); q.pop_front();
      for (int i = fir[u], v; ~i; i = e[i].nex) {
        v = e[i].v;
        if (!e[i^1].w || dis[v] <= dis[u]+e[i^1].c) continue;
        dis[v] = dis[u]+e[i^1].c;
        if (vis[v]) continue;
        vis[v] = 1;
        if (q.size() && dis[v] < dis[q.front()]) q.push_front(v);
        else q.push_back(v);
      }
      vis[u] = 0;
    }
    return dis[s] < INF;
  }
  T dfs(const int &u, const T &flow) {
    vis[u] = 1;
    if (u == t || flow <= 0) return flow;
    T res, used = 0;
    for (int i = fir[u], v; ~i; i = e[i].nex) {
      v = e[i].v;
      if (vis[v] || !e[i].w || dis[u] != dis[v]+e[i].c) continue;
      res = dfs(v, min(e[i].w, flow-used));
      if (!res) continue;
      mincost += res*e[i].c;
      e[i].w -= res;
      e[i^1].w += res;
      used += res;
      if (used == flow) break;
    }
    return used;
  }
};
```

{% endspoiler %}
### 上下界网络流

---
## 最短路
[弱化](https://www.luogu.org/problemnew/show/P3371)
[标准](https://www.luogu.org/problemnew/show/P4779)
### Floyd
略
### Dijkstra
邻接表+堆优化
{% spoiler "代码" %}
```cpp
inline void Dijkstra() {
  priority_queue<pair<int,int>,vector<pair<int,int> >,greater<pair<int,int> > >q;
  memset(dis, 0x7f, sizeof dis);
  dis[S] = 0;
  q.push(make_pair(0, S));
  pair<int, int> cur;
  while(q.size()) {
    cur = q.top(); q.pop();
    if(dis[cur.second] < cur.first) continue;
    for(int i = fir[cur.second], to, now; i; i = nex[i]) {
      to = ver[i];
      now = cur.first+w[i];
      if(now >= dis[to]) continue;
      dis[to] = now;
      q.push(make_pair(now, to));
    }
  }
}
```

{% endspoiler %}
### SPFA
{% spoiler "代码" %}
```cpp
inline void SPFA() {
  fill(dis+1, dis+n+1, INT_MAX);
  dis[S] = 0;
  head = tail = 0;
  q[++tail] = S;
  while(head < tail) {
    int cur = q[++head];
    for(int i = fir[cur], to, tmp; i; i = nex[i]) {
      to = ver[i];
      tmp = dis[cur]+w[i];
      if(tmp >= dis[to]) continue;
      dis[to] = tmp;
      q[++tail] = to;
    }
  }
}
```

{% endspoiler %}

---
## [负环](https://www.luogu.org/problemnew/show/P3385)
{% spoiler "代码" %}
```cpp
// 返回true有负环,返回false没负环
inline bool SPFA() {
  q[++tail] = 1;
  vis[1] = 1;
  cnt[1] = 1;
  dis[1] = 0;
  while(head < tail) {
    int cur = q[(++head)%Maxn];
    vis[cur] = 0;
    for(int i = fir[cur], to; i; i = nex[i]) {
      to = ver[i];
      if(dis[cur]+w[i] < dis[to]) {
        dis[to] = dis[cur]+w[i];
        if(!vis[to]) {
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

{% endspoiler %}

---
## [割点](https://www.luogu.org/problemnew/show/P3388)
{% spoiler "代码" %}
```cpp
void tarjan(int cur, int fa) {
  dfn[cur] = low[cur] = ++_dfn;
  int child = 0;
  for(auto i : e[cur]) {
    if(!dfn[i]) {
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

{% endspoiler %}

---
## [SCC强连通分量|Tarjan](https://www.luogu.com.cn/problem/P2863)
### 递归版本
{% spoiler "代码" %}
```cpp
int _dfn, _col, _top;
int dfn[N], low[N], vis[N], col[N], sta[N];

void tarjan(const int &u) {
  dfn[u] = low[u] = ++_dfn;
  vis[u] = 1;
  sta[++_top] = u;
  for (int v : e[u]) {
    if (!dfn[v]) {
      tarjan(v);
      low[u] = min(low[u], low[v]);
    } else if (vis[v]) {
      low[u] = min(low[u], low[v]);
    }
  }
  if (dfn[u] == low[u]) {
    ++_col;
    do {
      col[sta[_top]] = _col;
      vis[sta[_top]] = 0;
    } while (sta[_top--] != u);
  }
}
```

{% endspoiler %}

### 非递归版本
{% spoiler "代码" %}
```cpp
int _dfn, _col, _top;
int dfn[N], low[N], ins[N], col[N], sta[N];

struct Stack_Node {
  int u;
  int edge_info;
  int state;
} tarjan_stack[N];

void tarjan(const int &u) {
  static int stack_top;
  stack_top = 0;
  tarjan_stack[++stack_top] = {u, 0, 0};
  while (stack_top) {
    bool flag = false;
    int &u = tarjan_stack[stack_top].u;
    auto &i = tarjan_stack[stack_top].edge_info;
    switch (tarjan_stack[stack_top].state) {
    case 0 :
    tarjan_stack[stack_top].state = 1;
    flag = false;
      dfn[u] = low[u] = ++_dfn;
      ins[u] = 1;
      sta[++_top] = u;
      for (; i < (int)e[u].size(); ++i) {
#define v e[u][i]
        if (!dfn[v]) {
          tarjan_stack[++stack_top] = {v, 0, 0};
    flag = true;
          break;
    case 1 :
    flag = false;
          low[u] = min(low[u], low[v]);
        } else if (ins[v]) {
          low[u] = min(low[u], low[v]);
        }
#undef v
      }
    if (flag) break;
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

{% endspoiler %}

## [缩点](https://www.luogu.org/problemnew/show/P3387)
{% spoiler "代码" %}
```cpp
void tarjan(int u) {
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

inline void suodian() {
  for (int i = 1; i <= n; ++i) {
    if (!dfn[i]) tarjan(i);
  }
  for (int i = 1; i <= n; ++i) {
    for (int j : e[i]) {
      if (col[i] == col[j]) continue;
      e_col[col[i]].push_back(col[j]);
    }
  }
}
```

{% endspoiler %}
## [2-SAT](https://www.luogu.com.cn/problem/P4782)
### SCC Tarjan
$O(n+m)$
{% spoiler "代码" %}
```cpp
struct TWO_SAT { // node stkrt from 0
  int top, _dfn, _scc;
  int dfn[N<<1], low[N<<1], stk[N<<1], scc[N<<1], res[N];
  vector<int> e[N<<1];
  void init(const int &n) {
    top = 0;
    memset(dfn, 0, sizeof(int)*n*2);
    memset(low, 0, sizeof(int)*n*2);
    memset(scc, 0, sizeof(int)*n*2);
    for (int i = 0; i < n<<1; ++i) vector<int>().swap(e[i]);
  }
  // if u then v
  void add_edge(const int &u, const int &v) {
    e[u].emplace_back(v);
  }
  void add_edge(const int &u, const int &uv, const int &v, const int &vv) {
    e[u<<1^uv].emplace_back(v<<1^vv);
  }
  // pt i ==> i<<1 && i<<1|1 ==> 0 && 1
  inline bool work(const int &n) {
    for (int i = 0; i <= n<<1; ++i)
      if (!dfn[i]) tarjan(i);
    for (int i = 0; i < n; ++i) {
      if (scc[i<<1] == scc[i<<1|1]) return false;
      res[i] = scc[i<<1] > scc[i<<1|1];
    }
    return true;
  }
  void tarjan(const int &u) {
    dfn[u] = low[u] = ++_dfn;
    stk[++top] = u;
    for (int &v : e[u]) {
      if (!dfn[v]) {
        tarjan(v);
        low[u] = min(low[u], low[v]);
      } else if (!scc[v]) {
        low[u] = min(low[u], dfn[v]);
      }
    }
    if (dfn[u] == low[u]) {
      ++_scc;
      do {
        scc[stk[top]] = _scc;
      } while (stk[top--] != u);
    }
  }
};
```

{% endspoiler %}
### DFS
$O(nm)$
所求结果字典序最小
{% spoiler "代码" %}
```cpp
struct TWO_SAT {
  int n, cnt;
  int res[N], mem[N<<1], mark[N<<1];
  vector<int> e[N<<1];
  void init(const int &_n) {
    n = _n;
    memset(mark, 0, sizeof(int)*n*2);
    for (int i = 0; i < n<<1; ++i) vector<int>().swap(e[i]);
  }
  // if u then v
  void add_edge(const int &u, const int &v) {
    e[u].emplace_back(v);
  }
  // pt i ==> i<<1 && i<<1|1 ==> 0 && 1
  void add_edge(const int &u, const int &uv, const int &v, const int &vv) {
    e[u<<1|uv].emplace_back(v<<1|vv);
  }
  // tag 0 any 1 smallest
  bool work() {
    for (int i = 0; i < n; ++i) {
      if (mark[i<<1] || mark[i<<1|1]) continue;
      cnt = 0;
      if (!dfs(i<<1)) {
        while (cnt) mark[mem[cnt--]] = 0;
        if (!dfs(i<<1|1)) return false;
      }
    }
    for (int i = 0; i < n<<1; ++i) if (mark[i]) res[i>>1] = i&1;
    return true;
  }
  bool dfs(const int &u) {
    if (mark[u^1]) return false;
    if (mark[u]) return true;
    mark[mem[++cnt] = u] = 1;
    for (int v : e[u]) if (!dfs(v)) return false;
    return true;
  }
};
```

{% endspoiler %}

## [虚树](https://www.luogu.com.cn/problem/CF613D)

{% spoiler "代码" %}
```cpp
vector<int> ve[N];
void virtual_tree_clear(const int &u = 1) {
  for (const int &v : ve[u]) virtual_tree_clear(v);
  ve[u].clear();
}

// return the root of virtual tree
int virtual_tree_build(int vset[], const int &k) {
  static int stk[N], top;
  // id ==> dfn rank, d ==> depth
  int *id = hld.id, *d = hld.d;
  sort(vset+1, vset+k+1, [&](const int &x, const int &y) {
    return id[x] < id[y];
  });
  top = 0;
  int x, z;
  for (int i = 1; i <= k; ++i) {
    if (top && (z = hld.lca(vset[i], stk[top])) != stk[top]) {
      x = stk[top--];
      while (top && d[stk[top]] > d[z]) {
        ve[stk[top]].emplace_back(x);
        x = stk[top--];
      }
      ve[z].emplace_back(x);
      if (!top || stk[top] != z) stk[++top] = z;
    }
    stk[++top] = vset[i];
  }
  x = stk[top--];
  while (top) {
    ve[stk[top]].emplace_back(x);
    x = stk[top--];
  }
  // if (x != 1) ve[1].emplace_back(x); // force root at 1
  return x;
}
```

{% endspoiler %}

## 线段树优化建图

{% spoiler "代码" %}
```cpp
template <typename T>
struct SegmentTreeGarph {
  struct TreeNode {
    int l, r;
    int ls, rs;
  } tr[N*3];
  vector<pair<int, T>> *e;
  int tot, root[2];
  // op [down, 0] [up, 1]
  template <typename E>
  void build(const int &n, E *_e) {
    tot = n;
    e = _e;
    for (int i = 1; i <= n; ++i) tr[i].l = tr[i].r = i;
    build(1, n, root[0], 0);
    build(1, n, root[1], 1);
  }
  void build(const int &l, const int &r, int &i, const int &op) {
    if (l == r) return i = l, void();
    i = ++tot;
    tr[i].l = l; tr[i].r = r;
    int mid = (l+r)>>1;
    build(l, mid, tr[i].ls, op);
    build(mid+1, r, tr[i].rs, op);
    e[op ? tr[i].ls : i].emplace_back(op ? i : tr[i].ls, 0);
    e[op ? tr[i].rs : i].emplace_back(op ? i : tr[i].rs, 0);
  }
  void insert(const int &o, const int &l, const int &r, const T &w,
      const int &op) {
    if (l == r) e[op ? l : o].emplace_back(op ? o : l, w);
    else insert(o, l, r, w, op, root[op]);
  }
  void insert(const int &o, const int &l, const int &r, const T &w,
      const int &op, const int &i) {
    if (tr[i].l >= l && tr[i].r <= r) {
      e[op ? i : o].emplace_back(op ? o : i, w);
      return;
    }
    int mid = (tr[i].l+tr[i].r)>>1;
    if (l <= mid) insert(o, l, r, w, op, tr[i].ls);
    if (r >  mid) insert(o, l, r, w, op, tr[i].rs);
  }
};
```

{% endspoiler %}

### [+最短路](https://www.luogu.com.cn/problem/CF786B) 
### [+网络流](https://ac.nowcoder.com/acm/contest/5670/G)
### [+2-SAT](http://acm.hdu.edu.cn/showproblem.php?pid=6824)

## 矩阵树定理|Kirchhoff

解决一张图的生成树个数计数问题(详情见oi-wiki)

## 斯坦纳树

给定连通图 $G$ 中的 $n$ 个点 $m$ 条边与 $k$ 个关键点，连接 $k$ 个关键点，使得生成树的所有边的权值和最小。

我们使用状态压缩动态规划来求解。用 $f(i,S)$ 表示以 $i$ 为根的一棵树，包含集合 $S$ 中所有点的最小边权值和。

### 边权最小

- 首先对连通的子集进行转移， $f(i,S)\leftarrow \min(f(i,S),f(i,T)+f(i,S-T))$ 。

- 在当前的子集连通状态下进行边的松弛操作， $f(i,S)\leftarrow \min(f(i,S),f(j,S)+w(j,i))$ 

复杂度 $O(n\times 3^k+m\log m\times 2^k)$

{% spoiler "代码" %}
```cpp
int dp[N][1<<K];
vector<pair<int, int>> e[N]; // e[u] = {w, v}
priority_queue<pair<int, int>> q;

void dijkstra(int s) {
  while (q.size()) {
    int ud = -q.top().first;
    int u = q.top().second;
    q.pop();
    if (ud > dp[u][s]) continue;
    for (int i = 0, v, w; i < (int)e[u].size(); ++i) {
      w = e[u][i].first;
      v = e[u][i].second;
      if (dp[v][s] <= dp[u][s]+w) continue;
      dp[v][s] = dp[u][s]+w;
      q.push({-dp[v][s], v});
    }
  }
}

int steiner_tree(int *p) { // p[] is key point
  memset(dp, 0x3f, sizeof dp);
  for (int i = 0; i < k; ++i) dp[p[i]][1<<i] = 0;
  for (int s = 1; s < 1<<k; ++s) {
    for (int i = 1; i <= n; ++i) {
      for (int t = s&(s-1); t; t = s&(t-1))
        dp[i][s] = min(dp[i][s], dp[i][s^t]+dp[i][t]);
      if (dp[i][s] != INF) q.push({-dp[i][s], i});
    }
    dijkstra(s);
  }
  return dp[p[0]][(1<<k)-1];
}
```

{% endspoiler %}

### 点权最小

- $f(i,S)\leftarrow \min(f(i,S),f(i,T)+f(i,S-T)-a_i)$ 。由于此处合并时同一个点 $a_i$ ，会被加两次，所以减去。

- $f(i,S)\leftarrow \min(f(i,S),f(j,S)+w(j,i))$ 。

### 路径输出

{% spoiler "代码" %}
```cpp
void dfs(int u, int s){
    if(!pre[u][s].second) return;
    // print
    if(pre[u][s].first == u) dfs(u, s^pre[u][s].second);
    dfs(pre[u][s].first, pre[u][s].second);
}
```

{% endspoiler %}

## [树上背包](https://blog.csdn.net/m0_37809890/article/details/102827056)

时间复杂度 $O(n^2)$

{% spoiler "代码" %}
```cpp
void dfs(int u) {
  num[u] = 1;
  for (int &v : e[u]) {
    dfs(v);
    for (int i = min(m, num[u]+num[v]); j; --j) {
      for (int j = max(0, i-num[u]); j <= min(num[v], i); ++j) { // i-j >= num[u]
        dp[u][i] = max(dp[u][i], dp[u][i-j]+dp[v][j]);
      }
    }
    num[u] += num[v];
  }
}
```

{% endspoiler %}

---
# 数论
## 快排
{% spoiler "代码" %}
```cpp
void quick_sort(int l, int r) {
  if(l >= r) return;
  swap(a[l], a[l+rand()%(r-l)]);
  int i = l, j = r, mid = a[l];
  while(i < j) {
    while(i < j && a[j] >= mid) --j;
    swap(a[i], a[j]);
    while(i < j && a[i] < mid) ++i;
    swap(a[i], a[j]);
  }
  quick_sort(l, i-1);
  quick_sort(i+1, r);
}
```

{% endspoiler %}

---
## 求第K大数
[HDOJ 2665](http://acm.hdu.edu.cn/showproblem.php?pid=2665)
[POJ 2104](http://poj.org/problem?id=2104)
{% spoiler "代码" %}
```cpp
int kth_element(int l, int r, int k) {
  if(l == r) return a[l];
  swap(a[l], a[l+rand()%(r-l)]);
  int mid = a[l], i = l, j = r;
  while(i < j) {
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

{% endspoiler %}
STL (排序,无返回值)
{% spoiler "代码" %}
```cpp
nth_element(a+1, a+k+1, a+n+1);
```

{% endspoiler %}

---
## [求逆序对(归并排序)](https://www.luogu.org/problemnew/show/P1908)
{% spoiler "代码" %}
```cpp
void merge_sort(int l, int r) {
  if(l == r) return;
  int mid = (l+r)>>1;
  merge_sort(l, mid);
  merge_sort(mid+1, r);
  int i = l, j = mid+1, k = l;
  while(k <= r) {
    if(j <= r && (i > mid || a[j] < a[i])) {
      ans += mid-i+1;
      b[k++] = a[j++];
    }
    else b[k++] = a[i++];
  }
  memcpy(a+l, b+l, sizeof(int)*(r-l+1));
}
```

{% endspoiler %}

---
## 线性基
求最大值[Luogu3812](https://www.luogu.com.cn/problem/P3812)

求第k大数[HDOJ3949](http://acm.hdu.edu.cn/showproblem.php?pid=3949)
{% spoiler "代码" %}
```cpp
template <typename T>
struct LinearBase {
  int sz = sizeof(T)*8, zero;
  T tot;
  vector<T> b, rb, p;
  LinearBase(){ init(); }
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
  void merge(const LinearBase xj) {
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

{% endspoiler %}
[前缀和线性基](http://acm.hdu.edu.cn/showproblem.php?pid=6579)
vector跑贼鸡儿慢
{% spoiler "代码" %}
```cpp
template <class T>
struct PreSumLB {
  int tot, sz = sizeof(T)*8;
  vector<T> b[N];
  vector<int> p[N];
  PreSumLB() { init(); }
  void init() {
    tot = 0;
    vector<T>(sz, 0).swap(b[0]);
    vector<int>(sz, 0).swap(p[0]);
  }
  void append(T val) {
    int pos = ++tot;
    vector<T> &bb = b[tot];
    vector<int> &pp = p[tot];
    pp = p[tot-1];
    bb = b[tot-1];
    for (int i = sz-1; i >= 0; --i) if ((val>>i)&1) {
      if (bb[i]) {
        if (pos > pp[i]) swap(pos, pp[i]), swap(val, bb[i]);
        val ^= bb[i];
      } else {
        bb[i] = val;
        pp[i] = pos;
        return;
      }
    }
  }
  T query(const int &l, const int &r) {
    T res = 0;
    vector<T> &bb = b[r];
    vector<int> &pp = p[r];
    for (int i = sz-1; i >= 0; --i)
      if (pp[i] >= l) res = max(res, res^bb[i]);
    return res;
  }
};
```

{% endspoiler %}

---
## 矩阵
### [矩阵快速幂](https://www.luogu.org/problemnew/show/P3390)
### [矩阵求逆](https://www.luogu.com.cn/problem/P4783)
{% spoiler "代码" %}
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
  friend ostream& operator << (ostream &os, Martix<T> &mx) {
    for (int i = 1; i <= mx.n; ++i)
      for (int j = 1; j <= mx.m; ++j)
        os << mx[i][j] << " \n"[j==mx.m];
    return os;
  }
};
```

{% endspoiler %}
## [高斯消元](https://www.luogu.com.cn/problem/P3389)
{% spoiler "代码" %}
```cpp
struct GaussElimination {
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

{% endspoiler %}
### [异或方程组](http://www.cppblog.com/MatoNo1/archive/2012/05/20/175404.html)
[luogu 2962](https://www.luogu.com.cn/problem/P2962)

a[i][j] 第i个是否对j有影响

a[i][n+1] 第i个最后被翻转与否
{% spoiler "代码" %}
```cpp
// -1 : no solution, 0 : multi , 1 : one
template <typename T>
int XorGauss(T a[N], const int &n) {
  for (int i = 1, j, k; i <= n; ++i) {
    for (k = i; !a[k][i] && k <= n; ++k) {}
    if (k <= n) swap(a[k], a[i]);
    for (j = 1; j <= n; ++j) if (i != j && a[j][i])
      for (k = i; k <= n+1; ++k) a[j][k] ^= a[i][k];
      // a[j] ^= a[i]; // bitset<N> a[N]
  }
  for (int i = 1; i <= n; ++i) if (!a[i][i] && a[i][n+1]) return -1;
  for (int i = 1; i <= n; ++i) if (!a[i][i]) return 0;
  return 1;
}
// dfs(n, 0)
void dfs(const int &u, const int &num) {
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

{% endspoiler %}

---
## [拉格朗日插值](https://www.luogu.com.cn/problem/P4781)
{% spoiler "代码" %}
```cpp
template <typename T, typename H, typename P>
long long Largrange(const T &k, const int &n, const H x[], const P y[]) {
  k %= MOD;
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

{% endspoiler %}
{% spoiler "代码" %}
```cpp
template <typename T, typename P> // x[i] = i -> y[i] = f(i)
long long Largrange(const T &k, const int &n, const P y[]) {
  if (k <= n) return y[k];
  static long long pre[N], suf[N];
  long long res = 0;
  k %= MOD;
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

{% endspoiler %}

---
## [快速幂](https://www.luogu.org/problemnew/show/P1226)
{% spoiler "代码" %}
```cpp
template <typename T, typename H>
inline T qpow(const T &a, const H &p, const int &mo = MOD) {
  long long res = 1, x = a;
  for (H i = p; i; i >>= 1, x = x*x%mo)
    if (i&1) res = res*x%mo;
  return static_cast<T>(res);
}
```

{% endspoiler %}

---
## 快速乘
{% spoiler "代码" %}
```cpp
inline long long qmul(long long x, long long y, long long mo) {
  long long res = 0;
  while (y) {
    if (y&1) res = (res+x)%mo;
    x = (x<<1)%mo;
    y >>= 1;
  }
  return res;
}
```

{% endspoiler %}
{% spoiler "代码" %}
```cpp
inline long long qmul(long long x, long long y, long long mo) {
  return (long long)((__int128)x*y%mo);
}
```

{% endspoiler %}
{% spoiler "代码" %}
```cpp
inline long long qmul(long long x, long long y, long long mo) {
  // x*y - floor(x*y/mo)*mo
  typedef unsigned long long ull;
  typedef long double ld;
  return ((ull)x*y-(ull)((ld)x/mo*y)*mo+mo)%mo;
}
```

{% endspoiler %}

---
## 复数
{% spoiler "代码" %}
```cpp
struct comp {
  typedef double T; // maybe long double ?
  T real, imag;
  comp (const double &_real = 0, const double &_imag = 0) : real(_real), imag(_imag) {}
  friend comp operator + (const comp &c1, const comp &c2) { return comp(c1.real+c2.real, c1.imag+c2.imag); }
  friend comp operator - (const comp &c1, const comp &c2) { return comp(c1.real-c2.real, c1.imag-c2.imag); }
  friend comp operator * (const comp &c1, const comp &c2) { return comp(c1.real*c2.real-c1.imag*c2.imag, c1.real*c2.imag+c1.imag*c2.real); }
  comp& operator += (const comp &c) { return *this = *this+c; }
  comp& operator -= (const comp &c) { return *this = *this-c; }
  comp& operator *= (const comp &c) { return *this = *this*c; }
  friend istream& operator >> (istream &is, comp &c) { return is >> c.real >> c.imag; }
  friend ostream& operator << (ostream &os, comp &c) { return os << c.real << setiosflags(ios::showpos) << c.imag << "i";}
  comp conjugate() { return comp(real, -imag); }
  friend comp conjugate(const comp &c) { return comp(c.real, -c.imag); }
};
```

{% endspoiler %}

---
## [快速傅里叶变换|FFT](https://www.luogu.com.cn/problem/P3803)
{% spoiler "代码" %}
```cpp
// array [0, n)
namespace FFT {
  static const int SIZE = (1<<18)+3;
  int len, bit;
  int rev[SIZE];
  // #define comp complex<long double>
  void fft(comp a[], int flag = 1) {
    for (int i = 0; i < len; ++i)
      if (i < rev[i]) swap(a[i], a[rev[i]]);
    for (int base = 1; base < len; base <<= 1) {
      comp w, wn = {cos(PI/base), flag*sin(PI/base)};
      for (int i = 0; i < len; i += base*2) {
        w = { 1.0, 0.0 };
        for (int j = 0; j < base; ++j) {
          comp x = a[i+j], y = w*a[i+j+base];
          a[i+j] = x+y;
          a[i+j+base] = x-y;
          w *= wn;
        }
      }
    }
  }
  void work(comp f[], const int &n, comp g[], const int &m) {
    len = 1; bit = 0;
    while (len < n+m) len <<= 1, ++bit;
    // multi-testcase
    for (int i = n; i < len; ++i) f[i] = 0;
    for (int i = m; i < len; ++i) g[i] = 0;
    for (int i = 0; i < len; ++i)
      rev[i] = (rev[i>>1]>>1)|((i&1)<<(bit-1));
    fft(f, 1); fft(g, 1);
    for (int i = 0; i < len; ++i) f[i] *= g[i];
    fft(f, -1);
    for (int i = 0; i < n+m; ++i) f[i].real /= len;
  }
  /*
  template <class T>
  void work(T a[], const int &n) {
    static comp f[SIZE];
    len = 1; bit = 0;
    while (len < n+n) len <<= 1, ++bit;
    // multi-testcase
    for (int i = 0; i < n; ++i) f[i] = a[i];
    for (int i = n; i < len; ++i) f[i] = 0;
    for (int i = 0; i < len; ++i) rev[i] = (rev[i>>1]>>1)|((i&1)<<(bit-1));
    fft(f, 1);
    for (int i = 0; i <= len; ++i) f[i] *= f[i];
    fft(f, -1);
    for (int i = 0; i < n+n; ++i) a[i] = static_cast<T>(f[i].real/len+.5);
  }
  */
}
```

{% endspoiler %}

---
## 快速数论变换|NTT
{% spoiler "代码" %}
```cpp
// array [0, n)
namespace NTT {
  static const int SIZE = (1<<18)+3;
  const int G = 3;
  int len, bit;
  int rev[SIZE];
  long long f[SIZE], g[SIZE];
  template <class T>
  void ntt(T a[], int flag = 1) {
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
  template <class T>
  void work(T a[], const int &n, T b[], const int &m) {
    len = 1; bit = 0;
    while (len < n+m) len <<= 1, ++bit;
    for (int i = 0; i < n; ++i) f[i] = a[i];
    for (int i = n; i < len; ++i) f[i] = 0;
    for (int i = 0; i < m; ++i) g[i] = b[i];
    for (int i = m; i < len; ++i) g[i] = 0;
    for (int i = 0; i < len; ++i)
      rev[i] = (rev[i>>1]>>1)|((i&1)<<(bit-1));
    ntt(f, 1); ntt(g, 1);
    for (int i = 0; i < len; ++i) f[i] = f[i]*g[i]%MOD;
    ntt(f, -1);
    long long inv = qpow(len, MOD-2);
    for (int i = 0; i < n+m-1; ++i) f[i] = f[i]*inv%MOD;
  }
}
```

{% endspoiler %}
## [任意模数NTT|MTT](https://www.luogu.com.cn/problem/P4245)
{% spoiler "代码" %}
```cpp
namespace MTT {
  static const int SIZE = (1<<18)+7;
  int Mod = MOD;
  comp w[SIZE];
  int bitrev[SIZE];
  long long f[SIZE];
  void fft(comp *a, const int &n) {
    for (int i = 0; i < n; ++i) if (i < bitrev[i]) swap(a[i], a[bitrev[i]]);
    for (int i = 2, lyc = n >> 1; i <= n; i <<= 1, lyc >>= 1)
      for (int j = 0; j < n; j += i) {
        comp *l = a + j, *r = a + j + (i >> 1), *p = w;
        for (int k = 0; k < i>>1; ++k) {
          comp tmp = *r * *p;
          *r = *l - tmp, *l = *l + tmp;
          ++l, ++r, p += lyc;
        }
      }
  }
  template <class T>
  inline void work(T *x, const int &n, T *y, const int &m) {
    static int bit, L;
    static comp a[SIZE], b[SIZE];
    static comp dfta[SIZE], dftb[SIZE];

    for (L = 1, bit = 0; L < n+m-1; ++bit, L <<= 1);
    for (int i = 0; i < L; ++i) bitrev[i] = bitrev[i >> 1] >> 1 | ((i & 1) << (bit - 1));
    for (int i = 0; i < L; ++i) w[i] = comp(cos(2 * PI * i / L), sin(2 * PI * i / L));

    for (int i = 0; i < n; ++i) (x[i] += Mod) %= Mod, a[i] = comp(x[i] & 32767, x[i] >> 15);
    for (int i = n; i < L; ++i) a[i] = 0;
    for (int i = 0; i < m; ++i) (y[i] += Mod) %= Mod, b[i] = comp(y[i] & 32767, y[i] >> 15);
    for (int i = m; i < L; ++i) b[i] = 0;
    fft(a, L), fft(b, L);
    for (int i = 0; i < L; ++i) {
      int j = (L - i) & (L - 1);
      static comp da, db, dc, dd;
      da = (a[i] + conjugate(a[j])) * comp(.5, 0);
      db = (a[i] - conjugate(a[j])) * comp(0, -.5);
      dc = (b[i] + conjugate(b[j])) * comp(.5, 0);
      dd = (b[i] - conjugate(b[j])) * comp(0, -.5);
      dfta[j] = da*dc + da*dd*comp(0, 1);
      dftb[j] = db*dc + db*dd*comp(0, 1);
    }
    for (int i = 0; i < L; ++i) a[i] = dfta[i];
    for (int i = 0; i < L; ++i) b[i] = dftb[i];
    fft(a, L), fft(b, L);
    for (int i = 0; i < L; ++i) {
      int da = (long long)(a[i].real / L + 0.5) % Mod;
      int db = (long long)(a[i].imag / L + 0.5) % Mod;
      int dc = (long long)(b[i].real / L + 0.5) % Mod;
      int dd = (long long)(b[i].imag / L + 0.5) % Mod;
      f[i] = (da + ((long long)(db + dc) << 15) + ((long long)dd << 30)) % Mod;
    }
    for (int i = 0; i < n+m-1; ++i) (f[i] += Mod) %= Mod;
  }
}

```

{% endspoiler %}
## [分治FFT](https://www.luogu.com.cn/problem/P4721)
{% spoiler "代码" %}
```cpp
// give g[1, n) ask f[0, n)
// f[i] = sigma f[i-j]*g[j] (1 <= j <= i)
template <class T> // [l, r]
void cdq_fft(T f[], T g[], const int &l, const int &r) {
  if (r-l <= 1) return;
  int mid = (l+r)>>1;
  cdq_fft(f, g, l, mid);
  NTT::work(f+l, mid-l, g, r-l);
  for (int i = mid; i < r; ++i)
    (f[i] += NTT::f[i-l]) %= MOD;
  cdq_fft(f, g, mid, r);
}
// f[0] = 1; cdq_fft(f, g, 0, n);
```

{% endspoiler %}

## 快速沃尔什变换|FWT

[推导详解](https://www.luogu.com.cn/blog/command-block/wei-yun-suan-juan-ji-yu-ji-kuo-zhan)

[公式参考](https://www.cnblogs.com/GavinZheng/p/11721127.html)

[洛谷例题](https://www.luogu.com.cn/problem/P4717)

复杂度 $O(n\log n) | O(n2^n)$

$FWT(A\pm B)=FWT(A)\pm FWT(B)$

$FWT(cA)=cFWT(A)$

定义 $\bigoplus$ 为任意集合运算

$FWT(A\bigoplus B)=FWT(A)\times FWT(B)$

求 $C_i = \sum\limits_{i=j\bigoplus k}{a_j b_k}$

### 或运算
$FWT(A)[i] = \sum\limits_{j|i=i}{A[j]}$

$FWT(A) = [FWT(A_0),FWT(A_0+A_1)]$

$IFWT(A) = [IFWT(A_0),IFWT(A_1)-IFWT(A_0)]$


### 与运算
$FWT(A)[i] = \sum\limits_{i\&j=j}{A[i]}$

$FWT(A) = [FWT(A_0+A_1),FWT(A_1)]$

$IFWT(A) = [IFWT(A_0)-IFWT(A_1),IFWT(A_1)]$

### 异或运算
令 $d(x)$ 为 $x$ 在二进制下拥有的1的数量

$FWT(A)[i] = \sum\limits_{j}(-1)^{d(i\&j)}A[j]$

$FWT(A) = [FWT(A_0+A_1),FWT(A_0-A_1)]$

$IFWT(A) = [\frac{IFWT(A_1-A_0)}{2},\frac{IFWT(A_1+A_0)}{2}]$

### code

{% spoiler "代码" %}
```cpp
namespace FWT {
#define forforfor for (int l = 2; l <= len; l <<= 1)\
                  for (int i = 0, k = l>>1; i < len; i += l)\
                  for (int j = 0; j < k; ++j)

  const int SIZE = (1<<17)+3;
  int len;
  int f[SIZE], g[SIZE];
  template <class T> void init(T a[], const int &n, T b[], const int &m) {
    len = 1;
    while (len < max(n, m)) len <<= 1;
    for (int i = 0; i < n; ++i) f[i] = a[i];
    for (int i = n; i < len; ++i) f[i] = 0;
    for (int i = 0; i < m; ++i) g[i] = b[i];
    for (int i = m; i < len; ++i) g[i] = 0;
  }
  template <class T> void fwt_or(T a[], const int x = 1) {
    forforfor a[i+j+k] = (a[i+j+k]+1ll*a[i+j]*x)%MOD;
  }
  template <class T> void fwt_and(T a[], const int x = 1) {
    forforfor a[i+j] = (a[i+j]+1ll*a[i+j+k]*x)%MOD;
  }
  template <class T> void fwt_xor(T a[], const int x = 1) {
    forforfor {
      (a[i+j] += a[i+j+k]) %= MOD;
      a[i+j+k] = (a[i+j]-2*a[i+j+k]%MOD+MOD)%MOD;
      a[i+j] = 1ll*a[i+j]*x%MOD; a[i+j+k] = 1ll*a[i+j+k]*x%MOD;
    }
  }
  template <class T> void work_or(const T a[], const int &n, const T b[], const int &m) {
    init(a, n, b, m); fwt_or(f); fwt_or(g);
    for (int i = 0; i < len; ++i) f[i] = 1ll*f[i]*g[i]%MOD;
    fwt_or(f, MOD-1); // fwt_or(x, -1)
  }
  template <class T> void work_and(const T a[], const int &n, const T b[], const int &m) {
    init(a, n, b, m); fwt_and(f); fwt_and(g);
    for (int i = 0; i < len; ++i) f[i] = 1ll*f[i]*g[i]%MOD;
    fwt_and(f, MOD-1); // fwt_and(x, -1)
  }
  template <class T> void work_xor(const T a[], const int &n, const T b[], const int &m) {
    init(a, n, b, m); fwt_xor(f); fwt_xor(g);
    for (int i = 0; i < len; ++i) f[i] = 1ll*f[i]*g[i]%MOD;
    fwt_xor(f, mul_inverse(2)); // fwt_xor(x, 1/2)
  }
#undef forforfor
} // namespace FWT
```

{% endspoiler %}

## 快速莫比乌斯变换|FMT

据说 FWT 做的事情完全包含 FMT 且常数是一半(咕之

## 快速子集变换(子集卷积)|FST

$C_k = \sum\limits_{i\&j=0,i|j=k}{A_i B_j}$

复杂度 $O(n\log^2 n) | O(n^22^n)$

{% spoiler "代码" %}
```cpp
namespace FST {
  const int W = 20;
  const int N = 1<<W;
  int len, bit;
  int f[W+1][N], g[W+1][N], h[W+1][N], res[N];
  template <class T> void fwt(T a[], const int x = 1) {
    for (int l = 2; l <= len; l <<= 1)
    for (int i = 0, k = l>>1; i < len; i += l)
    for (int j = 0; j < k; ++j)
      a[i+j+k] = (a[i+j+k]+1ll*a[i+j]*x)%MOD;
  }
  template <class T> void work(const T a[], const int &n, const T b[], const int &m) {
    len = 1; bit = 0;
    while (len < max(n, m)) len <<= 1, ++bit;
    for (int i = 0; i <= bit; ++i)
      for (int j = 0; j < len; ++j)
        f[i][j] = g[i][j] = h[i][j] = 0;
    for (int i = 0; i < n; ++i) f[__builtin_popcount(i)][i] = a[i];
    for (int i = 0; i < m; ++i) g[__builtin_popcount(i)][i] = b[i];
    for (int i = 0; i <= bit; ++i) {
      fwt(f[i]); fwt(g[i]);
      for (int j = 0; j <= i; ++j)
        for (int k = 0; k < len; ++k)
          h[i][k] = (h[i][k]+1ll*f[j][k]*g[i-j][k])%MOD;
      fwt(h[i], MOD-1); // fwt(h[i], -1)
    }
    for (int i = 0; i < len; ++i) res[i] = h[__builtin_popcount(i)][i];
  }
} // namespace FST
```

{% endspoiler %}

### 倍增子集卷积

[hdu6851](http://acm.hdu.edu.cn/showproblem.php?pid=6851)

设多项式 $A = \sum\limits_{i=0}^{2^n-1}{a_i x^i},B=\sum\limits_{i=0}^{2^n-1}{b_i x^i}$

求 $C = A*B = \sum\limits_{i=0}^{2^n-1}{x^i \sum\limits_{d\subseteq i}{a_d b_{i-d} }}$

按照每个状态的最高位进行分组，然后卷 $n$ 次

复杂度 $O(\sum\limits_{i=1}^{n}{i^2 2^i}) = O(n^2 2^n)$

{% spoiler "代码" %}
```cpp
template <typename T> void vip_fst(T a[], const int &n) { // return a
  static int b[1<<B]; // warning: the type of b
  int len = 1; while (len < n) len <<= 1;
  memcpy(b, a, sizeof(T)*len);
  memset(a, 0, sizeof(T)*len); a[0] = 1;
  for (int i = 1; i < len; i <<= 1) {
    FST::work(a, i, b+i, i);
    for (int j = 0; j < i; ++j)
      a[i+j] = FST::h[__builtin_popcount(j)][j];
  }
}
```

{% endspoiler %}

---
## [第二类斯特林数](https://www.luogu.com.cn/problem/P5395)
{% spoiler "代码" %}
```cpp
inline void stirling(const int &n) {
  S[0][0] = 1;
  // 注意取模
  for (int i = 1; i <= n; ++i)
    for (int j = 1; j <= i; ++j)
      S[i][j] = S[i-1][j-1]+S[i-1][j]*j;
}
```

{% endspoiler %}
{% spoiler "代码" %}
```cpp
void stirling(const int &n) {
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

{% endspoiler %}

---
## 约瑟夫环
### [O(n)](https://blog.csdn.net/weixin_42659809/article/details/82596676)
{% spoiler "代码" %}
```cpp
int solve(int n, int v) { return n == 1 ? 0 : (solve(n-1, v)+v)%n; }
// res = solve(num, step)+1
```

{% endspoiler %}

---
## 最大公因数 gcd
{% spoiler "代码" %}
```cpp
__gcd(a, b); // <algorithm>
int gcd(int a, int b) { return b ? gcd(b, a%b) : a; }
inline int gcd(int a, int b) { while (b) a %= b, swap(a, b); return a; }
```

{% endspoiler %}
## 最小公倍数 lcm
$LCM(\frac{a}{b},\frac{c}{d})=\frac{LCM(a, c)}{GCD(b,d)}$

$LCM(\frac{a_1}{b_1},\frac{a_2}{b_2},...)=\frac{LCM(a1, a2,...)}{GCD(b1, b2,...)}$
{% spoiler "代码" %}
```cpp
inline int lcm(int a, int b) { return a/gcd(a, b)*b; }
```

{% endspoiler %}
## 扩展欧几里得([同余方程](https://www.luogu.org/problemnew/show/P1082))
{% spoiler "代码" %}
```cpp
template <typename T>
T exgcd(const T a, const T b, T &x, T &y) {
  if (!b) return x = 1, y = 0, a;
  T d = exgcd(b, a%b, y, x);
  y -= a/b*x;
  return d;
}
```

{% endspoiler %}

---
## [乘法逆元](https://www.luogu.org/problemnew/show/P3811)
### 拓展欧几里得
{% spoiler "代码" %}
```cpp
template <typename T>
inline T mul_inverse(const T &a, const T &mo = MOD) {
  T x, y;
  exgcd(a, mo, x, y);
  return (x%mo+mo)%mo;
}
```

{% endspoiler %}
### 费马小定理
{% spoiler "代码" %}
```cpp
template <typename T>
inline T mul_inverse(const T &a, const int &mo = MOD) {
  return qpow(a, mo-2);
}
```

{% endspoiler %}
### 线性递推
{% spoiler "代码" %}
```cpp
template <typename T>
inline void mul_inverse(T *inv, int mod = MOD) {
  inv[0] = inv[1] = 1;
  for(int i = 2; i <= n; ++i)
    inv[i] = 1ll*(mod-mod/i)*inv[mod%i]%mod;
}
```

{% endspoiler %}

---
## [中国剩余定理](https://blog.csdn.net/niiick/article/details/80229217)
### [中国剩余定理CRT(m互质)](https://www.luogu.org/problem/P3868)
{% spoiler "代码" %}
```cpp
inline long long CRT(int a[], int m[]) {
  long long res = 0, M = 1;
  for (int i = 1; i <= n; ++i)
    M *= m[i];
  for (int i = 1; i <= n; ++i)
    res = (res + a[i]*(M/m[i])*mul_inverse(M/m[i], m[i]))%M;
  return (res+M)%M;
}
```

{% endspoiler %}
### [扩展中国剩余定理EXCRT(m不互质)](https://www.luogu.org/problem/P4777)
{% spoiler "代码" %}
```cpp
inline long long EXCRT(long long a[], long long m[]) {
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

{% endspoiler %}

---
## 排列组合
{% spoiler "代码" %}
```cpp
struct Combination {
  int fac[N], inv[N];
  void init(const int &n) {
    fac[0] = inv[0] = fac[1] = inv[1] = 1;
    for (int i = 2; i <= n; ++i) {
      fac[i] = 1ll*fac[i-1]*i%MOD;
      inv[i] = 1ll*(MOD-MOD/i)*inv[MOD%i]%MOD;
    }
    for (int i = 2; i <= n; ++i) {
      inv[i] = 1ll*inv[i]*inv[i-1]%MOD;
    }
  }
  int A(const int &p, const int &q) {
    return p >= q ? 1ll*fac[p]*inv[p-q]%MOD : 0;
  }
  int C(const int &p, const int &q) {
    return p >= q ? 1ll*fac[p]*inv[q]%MOD*inv[p-q]%MOD : 0;
  }
};
```

{% endspoiler %}

### [奇偶性](https://blog.csdn.net/baodream/article/details/77822072)
C(n,k) 当 `n&k == k` 为奇数反之偶数

---
## 欧拉函数
{% spoiler "代码" %}
```cpp
template <typename T> inline T phi(T x) {
  T res = x;
  for (T i = 2; i*i <= x; ++i) {
    if (x%i) continue;
    res = res/i*(i-1);
    while (x%i == 0) x /= i;
  }
  if (x > 1) res = res/x*(x-1);
  return res;
}
```

{% endspoiler %}
### 筛法
{% spoiler "代码" %}
```cpp
struct Euler {
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

{% endspoiler %}

## 莫比乌斯函数

{% spoiler "代码" %}
```cpp
template <typename T> inline T miu(T x) {
  int t = 0;
  for (T i = 2, k; i*i <= x; ++i) {
    if (x%i) continue;
    for (k = 0, ++t; x %i == 0; x /= i, ++k) {}
    if (k >= 2) return 0;
  }
  if (x > 1) ++t;
  return t&1 ? -1 : 1;
}
```

{% endspoiler %}

---
## 线性筛素数
{% spoiler "代码" %}
```cpp
struct Euler {
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

{% endspoiler %}

---
## 判断素数(质数)
 某较优方法
{% spoiler "代码" %}
```cpp
inline bool is_prime(long long x) {
  if(x == 1) return false;
  if(x == 2 || x == 3) return true;
  if(x%6 != 1 && x%6 != 5) return false;
  for(long long i = 5; i*i <= x; i += 6)
    if(x%i == 0 || x%(i+2) == 0) return false;
  return true;
}
```

{% endspoiler %}
### Miller-Rabin 素性测试
{% spoiler "代码" %}
```cpp
inline bool MillerRabin(int x) {
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

{% endspoiler %}

## [线性筛GCD](https://www.luogu.com.cn/problem/P2568)
{% spoiler "代码" %}
```cpp
inline void gcd_init(const int &n) {
  for (int i = 1; i <= n; ++i)
  for (int j = 1; j <= n; ++j) if (!g[i][j])
  for (int k = 1; k <= n/i; ++k)
    g[k*i][k*j] = k;
}
```

{% endspoiler %}

---
## [分解质因数](https://www.luogu.org/problemnew/show/P1075)
{% spoiler "代码" %}
```cpp
// x = pi^ki...
for(int i = 2; i*i <= x; ++i)
  if(x%i == 0) {
    p[++tot] = i;
    for(; x%i == 0; x /= i) k[tot]++;
  }
if(x > 1) p[++tot] = x, k[tot] = 1;
```

{% endspoiler %}

---
## BSGS

[luogu 4884](https://www.luogu.org/problemnew/show/P4884)

求解关于 $t$ 的方程 $a^t \equiv x(mod m),\gcd(a, m) = 1$

{% spoiler "代码" %}
```cpp
// map<long long, int> mmp; // a^n = x
inline long long BSGS(long long a, long long x, long long m) {
  long long t = (long long)ceil(sqrt(m)); // b = a^i
  for(int i = 0; i < t; ++i)
    mmp[mul(x, qpow(a, i))] = i;
  a = qpow(a, t);
  long long now, ans; // now = (a^t)^i
  for(int i = 0; i <= t; ++i) {
    now = qpow(a, i);
    if(mmp.count(now)) {
      ans = t*i-mmp[now];
      if(ans > 0) return ans;
    }
  }
  return -1;
}
```

{% endspoiler %}

## 拓展BSGS

$\gcd(a, m) \neq 1$

## 错排

$D_1 = 0$

$D_2 = 1$

$D_n = (n-1)(D_{n-1}+D_{n-2})$

## 原根

[参考博客](https://blog.csdn.net/zhouyuheng2003/article/details/80163139#comments)

复杂度 $O(\sqrt{m}+g\times\log^2m)$

{% spoiler "代码" %}
```cpp
inline int getG(const int &m) {
  static int q[100000+7];
  int _phi = phi(m), x = _phi, tot = 0;
  for (int i = 2; i*i <= _phi; ++i) {
    if (x%i) continue;
    q[++tot] = _phi/i;
    while (x%i == 0) x /= i;
  }
  if (x > 1) x = q[++tot] = _phi/x;
  for (int g = 2, flag; ; ++g) {
    flag = 1;
    if (qpow(g, _phi, m) != 1) continue;
    for (int i = 1; i <= tot; ++i) {
      if (qpow(g, q[i], m) == 1) {
        flag = 0;
        break;
      }
    }
    if (flag) return g;
  }
}
```

{% endspoiler %}

## [大数阶乘](https://www.cnblogs.com/cax1165/p/6070902.html)

分块打表

{% spoiler "代码" %}
```cpp
namespace BigFac {
#define lon long long
lon a[110]={1,682498929,491101308,76479948,723816384,67347853,27368307,
625544428,199888908,888050723,927880474,281863274,661224977,623534362,
970055531,261384175,195888993,66404266,547665832,109838563,933245637,
724691727,368925948,268838846,136026497,112390913,135498044,217544623,
419363534,500780548,668123525,128487469,30977140,522049725,309058615,
386027524,189239124,148528617,940567523,917084264,429277690,996164327,
358655417,568392357,780072518,462639908,275105629,909210595,99199382,
703397904,733333339,97830135,608823837,256141983,141827977,696628828,
637939935,811575797,848924691,131772368,724464507,272814771,326159309,
456152084,903466878,92255682,769795511,373745190,606241871,825871994,
957939114,435887178,852304035,663307737,375297772,217598709,624148346,
671734977,624500515,748510389,203191898,423951674,629786193,672850561,
814362881,823845496,116667533,256473217,627655552,245795606,586445753,
172114298,193781724,778983779,83868974,315103615,965785236,492741665,
377329025,847549272,698611116};
lon fac(lon n, lon p = MOD) {
  if (n >= p) {
    return 0;
  } else if (p==1000000007) {
    lon now=n/10000000;
    lon ans=a[now];
    for(lon i=now*10000000+1;i<=n;i++)
      ans=ans*i%p;
    return ans%p;
  } else {
    lon ans=1;
    for(int i=1;i<=n;i++)
      ans=ans*i%p;
    return ans%p;
  }
}
}
```

{% endspoiler %}

---
# 动态规划 DP
(我全都不会)
## 记忆化搜索
## 线性DP
### [最长上升子序列LIS](http://codevs.cn/problem/1576/)
{% spoiler "代码" %}
```cpp
for(int i = 1; i <= n; ++i) {
  f[i] = 1;
  for(int j = 1; j < i; ++j)
    if(a[i] > a[j]) f[i] = max(f[i],f[j]+1);
}
```

{% endspoiler %}
### 最长公共子序列LCS
{% spoiler "代码" %}
```cpp
f[i][j] = max{  f[i-1][j],
                f[i][j-1],
                f[i-1][j-1]+1 (if A[i] == B[j])}
```

{% endspoiler %}
### 数字三角形
## 区间DP
## [树形DP](https://www.luogu.org/problemnew/show/P1352)
## 状压DP
### [枚举子集](https://cp-algorithms.com/algebra/all-submasks.html)
{% spoiler "代码" %}
```cpp
for (int i = s; i; i = (i-1)&s) {}
```

{% endspoiler %}
### 枚举n个元素,大小为k的二进制子集
{% spoiler "代码" %}
```cpp
int s=(1<<k)-1;
while(s<(1<<n)){
  work(s);
  int x=s&-s,y=s+x;
  s=((s&~y)/x>>1)|y; //这里有一个位反~
}
```

{% endspoiler %}

## 背包问题
### 01背包
### 完全背包
### 混合背包
### [分组背包](https://www.luogu.org/problemnew/show/P1757)
### [多重背包](https://www.luogu.org/problemnew/show/P1776)
二进制拆分
{% spoiler "代码" %}
```cpp
for(int i = 1, cnt, vi, wi, m; i <= n; ++i) {
  scanf("%d%d%d", &vi, &wi, &m);
  cnt = 1;
  while(m-cnt > 0) {
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

{% endspoiler %}
单调队列
{% spoiler "代码" %}
```cpp
for(int i = 1; i <= n; ++i) {
  scanf("%d%d%d", &v, &w, &m);
  for(int u = 0; u < w; ++u) {
    int maxp = (W-u)/w;
    head = 1; tail = 0;
    for(int k = maxp-1; k >= max(0, maxp-m); --k) {
      while(head <= tail && calc(u, q[tail]) <= calc(u, k)) tail--;
      q[++tail] = k;
    }
    for(int p = maxp; p >= 0; --p) {
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

{% endspoiler %}

---
## [SOS DP](https://codeforces.com/blog/entry/45223)
## 斜率优化

若dp方程为 $dp[i]=a[i] \cdot b[j]+c[i]+d[j]$ 时,由于存在$a[i] \cdot b[j]$ 这个既有 $i$ 又有 $j$ 的项,就需要使用斜率优化

### [「HNOI2008」玩具装箱 TOY](https://www.luogu.com.cn/problem/P3195)

$dp[i]=min(dp[j]+(sum[i]+i−sum[j]−j−L−1)^2)(j<i)$

令$a[i]=sum[i]+i,b[i]=sum[i]+i+L+1$

$dp[i]=dp[j]+(a[i]-b[j])^2$

$dp [ i ] = dp [ j ] + a [ i ] ^ 2 - 2 a [ i ] b [ j ] + b [ j ] ^ 2$

$2 a [ i ] b [ j ] + dp [ i ] - a [ i ] ^ 2 = dp [ j ] + b [ j ] ^ 2$

将 $b[j]$ 看作 $x,dp[j]+b[j]^2$ 看作 $y$，这个式子就可以看作一条斜率为 $2 a[i]$ 的直线

而对于每个 $i$ 来说, $a[i]$ 都是确定的, 类似线性规划

$dp[i]$ 的含义转化为：当上述直线过点 $P(b[j],dp[j]+b[j]^2)$ 时，直线在 $y$ 轴的截距加上 $a[i]^2$ (一个定值) 而题目即为找这个截距的最小值

## 四边形不等式

### 2D1D

$f_{l,r}=\min\limits_{k=l}^{r-1} \{f_{l,k}+f_{k+1,r}\}+w(l,r) \ \ (1\leq l \leq r \leq n)$

当 $w(l,r)$ 满足特定性质

- 区间包含单调性 ：如果对于任意 $l\leq l' \leq r' \leq r$ ，均有 $w(l',r')\leq w(l,r)$ 成立，则称函数 $w$ 对于区间包含关系具有单调性。
  
- 四边形不等式 ：如果对于任意 $l_1 \leq l_2 \leq r_1 \leq r_2$ ，均有 $w(l_1,r_1)+w(l_2,r_2) \leq w(l_1,r_2)+w(l_2,r_1)$ 成立，则称函数 $w$ 满足四边形不等式（简记为“交叉小于包含”）。若等号永远成立，则称函数 $w$ 满足 四边形恒等式 。

> 引理 1 ：若满足关于区间包含的单调性的函数 $w(l,r)$ 满足四边形不等式，则状态 $f_{l,r}$ 也满足四边形不等式。

> 定理 1 ：若状态 $f$ 满足四边形不等式，记 $m_{l,r}=\min\{k:f_{l,r}=g_{k,l,r}\}$ 表示最优决策点，则有 $m_{l,r-1} \leq m_{l,r} \leq m_{l+1,r}$

### 1D1D

$f_r = \min\limits_{l=1}^{r-1} \{f_l+w(l,r)\} \ \ (1\leq r \leq n)$

> 定理 2 ：若函数 $w(l,r)$ 满足四边形不等式，记 $h_{l,r}=f_l+w(l,r)$ 表示从 $l$ 转移过来的状态 $r$ , $k_r=\min\{l|f_r=h_{l,r}\}$ 表示最优决策点，则有 $\forall r_1 \leq r_2 : k_{r1} \leq k_{r2}$

{% spoiler "代码" %}
```cpp
void DP(int l, int r, int k_l, int k_r) {
  int mid = (l + r) / 2, k = k_l;
  // 求状态f[mid]的最优决策点
  for (int i = k_l; i <= min(k_r, mid - 1); ++i)
    if (w(i, mid) < w(k, mid)) i = k;
  f[mid] = w(k, mid);
  // 根据决策单调性得出左右两部分的决策区间，递归处理
  if (l < mid) DP(l, mid - 1, k_l, k);
  if (r > mid) DP(mid + 1, r, k, k_r);
}
```

{% endspoiler %}

### 满足四边形不等式的函数类

- 性质 1 ：若函数 $w_1(l,r),w_2(l,r)$ 均满足四边形不等式（或区间包含单调性），则对于任意 $c_1,c_2\geq 0$ ，函数 $c_1w_1+c_2w_2$ 也满足四边形不等式（或区间包含单调性）。

- 性质 2 ：若存在函数 $f(x),g(x)$ 使得 $w(l,r) = f(r)-g(l)$ ，则函数 $w$ 满足四边形恒等式。当函数 $f,g$ 单调增加时，函数 $w$ 还满足区间包含单调性。

- 性质 3 ：设 $h(x)$ 是一个单调增加的凸函数，若函数 $w(l,r)$ 满足四边形不等式并且对区间包含关系具有单调性，则复合函数 $h(w(l,r))$ 也满足四边形不等式和区间包含单调性。

- 性质 4 ：设 $h(x)$ 是一个凸函数，若函数 $w(l,r)$ 满足四边形恒等式并且对区间包含关系具有单调性，则复合函数 $h(w(l,r))$ 也满足四边形不等式。

首先需要澄清一点，凸函数（Convex Function）的定义在国内教材中有分歧，此处的凸函数指的是（可微的）下凸函数，即一阶导数单调增加的函数。

## [插头DP|轮廓线DP](https://www.cnblogs.com/y2823774827y/p/10140757.html)
### 一个闭合回路
{% spoiler "代码" %}
```cpp
const int P = 299987;
const int M = 1<<21;
const int N = 15;

int n, m;
int a[N][N];
long long dp[2][M];
int head[2][P], nex[2][M], tot[2], ver[2][M];
// long long dp[2][P];
// int head[2][P], nex[2][P], tot[2], ver[2][P];

inline void clear(const int &u) {
  for (int i = 1; i <= tot[u]; ++i) {
    dp[u][i] = 0; //
    nex[u][i] = 0; //
    head[u][ver[u][i]%P] = 0;
  }
  tot[u] = 0;
}

template <typename T, typename U>
inline void insert(const int &u, const T &x, const U &v) {
  int p = x%P;
  for (int i = head[u][p]; i; i = nex[u][i]) {
    if (ver[u][i] == x) return dp[u][i] += v, void();
  }
  ++tot[u]; assert(tot[u] < M);
  ver[u][tot[u]] = x;
  nex[u][tot[u]] = head[u][p];
  head[u][p] = tot[u];
  dp[u][tot[u]] = v;
}

template <typename T>
inline int get_val(const int &u, const T &x) {
  int p = x%P;
  for (int i = head[u][p]; i; i = nex[u][i]) {
    if (ver[u][i] == x) return dp[u][i];
  }
  return 0;
}

inline long long solve() {
  int u = 0, base = (1<<m*2+2)-1;
  long long res = 0;
  clear(u);
  insert(u, 0, 1);
  for (int i = 1; i <= n; ++i) {
    for (int j = 1; j <= m; ++j) {
      clear(u ^= 1);
      for (int k = 1; k <= tot[u^1]; ++k) {
        int state = ver[u^1][k];
        long long val = dp[u^1][k];
        if (j == 1) state = (state<<2)&base;
        // b1 right b2 down
        // 0 no 1 left 2 right
        int b1 = (state>>j*2-2)%4, b2 = (state>>j*2)%4;
        if (!a[i][j]) {
          if (!b1 && !b2) insert(u, state, val);
        } else if (!b1 && !b2) {
          if (a[i+1][j] && a[i][j+1]) insert(u, state+(1<<j*2-2)+(2<<j*2), val);
        } else if (!b1 && b2) {
          if (a[i][j+1]) insert(u, state, val);
          if (a[i+1][j]) insert(u, state+(b2<<j*2-2)-(b2<<j*2), val);
        } else if (b1 && !b2) {
          if (a[i+1][j]) insert(u, state, val);
          if (a[i][j+1]) insert(u, state-(b1<<j*2-2)+(b1<<j*2), val);
        } else if (b1 == 1 && b2 == 1) { // find 2 turn to 1
          for (int k = j+1, t = 1; k <= m; ++k) {
            if ((state>>k*2)%4 == 1) ++t;
            if ((state>>k*2)%4 == 2) --t;
            if (!t) { insert(u, state-(1<<j*2-2)-(1<<j*2)-(1<<k*2), val); break; }
          }
        } else if (b1 == 2 && b2 == 2) { // find 1 turn to 2
          for (int k = j-2, t = 1; k >= 0; --k) {
            if ((state>>k*2)%4 == 1) --t;
            if ((state>>k*2)%4 == 2) ++t;
            if (!t) { insert(u, state-(2<<j*2-2)-(2<<j*2)+(1<<k*2), val); break; }
          }
        } else if (b1 == 2 && b2 == 1) {
          insert(u, state-(2<<j*2-2)-(1<<j*2), val);
        } else if (i == ex && j == ey) { // b1 == 1, b2 == 2
          res += val;
        }
      }
    }
  }
  return res;
}
```

{% endspoiler %}

### 多个闭合回路
{% spoiler "代码" %}
```cpp
        else if (b1 == 1 && b2 == 2) {
          if (i == ex && j == ey) res += val;
          else dp[u][bit-(1<<j*2-2)-(1<<j*2+1)] += val;
        }
```

{% endspoiler %}

### 联通块

{% spoiler "代码" %}
```cpp
int n, u, res = -INF;
int a[N][N];
unordered_map<int, int> dp[2];

inline void decode(const int &state, int *const s) {
  for (int i = 1; i <= n; ++i) s[i] = (state>>i*3-3)%8;
}

inline void insert(const int *const s, const int &val) {
  static int vis[N];
  int state = 0, cnt = 0;
  memset(vis, 0, sizeof vis);
  for (int i = 1; i <= n; ++i) {
    if (s[i] && !vis[s[i]]) vis[s[i]] = ++cnt;
    state |= (vis[s[i]]<<i*3-3);
  }
  if (dp[u].count(state)) dp[u][state] = max(dp[u][state], val);
  else dp[u].insert({state, val});
  if (cnt == 1) res = max(res, val);
}

inline void solve() {
  static int s[N];
  dp[u = 0].clear();
  dp[u][0] = 0;
  for (int i = 1; i <= n; ++i) {
    for (int j = 1; j <= n; ++j) {
      dp[u ^= 1].clear();
      for (const auto &p : dp[u^1]) {
        decode(p.first, s);
        int b1 = s[j-1], b2 = s[j];
        // not choose
        s[j] = 0;
        int cnt = 0;
        for (int k = 1; k <= n; ++k) cnt += s[k] == b2;
        if (!b2 || cnt) insert(s, p.second);
        s[j] = b2;
        // choose
        if (!b1 && !b2) {
          s[j] = 7;
        } else {
          if (b1 > b2) swap(b1, b2); // in case b2 == 0
          s[j] = b2;
          if (b1) for (int k = 1; k <= n; ++k) if (s[k] == b1) s[k] = b2;
        }
        insert(s, p.second+a[i][j]);
      }
    }
  }
  cout << res << endl;
}
```

{% endspoiler %}

### L型

L 型地板：拐弯且仅拐弯一次。

发现没有，一个存在的插头只有两种状态：拐弯过和没拐弯过，因此我们这样定义插头：

0表没有插头，1表没拐过的插头，2表已经拐过的插头。b1代表当前点的右插头,b2代表当前点的下插头

---
# STL
## unordered_map 重载
{% spoiler "代码" %}
```cpp
struct Node {
  int a, b;
  // 重载 ==
  friend bool operator == (const Node &x, const Node &y) {
    return x.a == y.a && x.b == y.b;
  }
};

// 方法一
namespace std {
  template <>
  struct hash<Node> {
    size_t operator () (const Node &x) const {
      return hash<int>()(x.a)^hash<int>()(x.b);
    }
  };
}
unordered_map<Node, int> mp;

// 方法二
struct KeyHasher {
  size_t operator () (const Node &x) const {
    return hash<int>()(x.a)^hash<int>()(x.b);
  }
};
unordered_map<Node, int, KeyHasher> mmp;
```

{% endspoiler %}
## 定义函数
{% spoiler "代码" %}
```cpp
function<void(int&, int)> f = [&](int &x, int y) -> void {
  x += y;
};
```

{% endspoiler %}

---
# 分数
`warning:未完全验证`
{% spoiler "代码" %}
```cpp
template <class T>
struct Fraction {
  T a, b;
  void normalize() {
    if (a == 0) return (void)(b = 1);
    T g = __gcd(a, b);
    a /= g; b /= g;
  }
  Fraction(const T &_a = 0, const T &_b = 1) : a(_a), b(_b) { this->normalize(); }
  friend bool operator < (const Fraction &f1, const Fraction &f2) { return f1.a*f2.b < f2.a*f1.b; }
  friend bool operator > (const Fraction &f1, const Fraction &f2) { return f1.a*f2.b > f2.a*f1.b; }
  friend bool operator == (const Fraction &f1, const Fraction &f2) { return f1.a*f2.b == f2.a*f1.b; }
  friend bool operator != (const Fraction &f1, const Fraction &f2) { return f1.a*f2.b != f2.a*f1.b; }
  friend bool operator <= (const Fraction &f1, const Fraction &f2) { return f1 < f2 || f1 == f2; }
  friend bool operator >= (const Fraction &f1, const Fraction &f2) { return f1 > f2 || f1 == f2; }
  friend Fraction operator + (const Fraction &f1, const Fraction &f2) { return Fraction(f1.a*f2.b+f2.a*f1.b, f1.b*f2.b); }
  friend Fraction operator - (const Fraction &f1, const Fraction &f2) { return Fraction(f1.a*f2.b-f2.a*f1.b, f1.b*f2.b); }
  friend Fraction operator * (const Fraction &f1, const Fraction &f2) { return Fraction(f1.a*f2.a, f1.b*f2.b); }
  friend Fraction operator / (const Fraction &f1, const Fraction &f2) { return Fraction(f1.a*f2.b, f1.b*f2.a); }
  Fraction& operator += (const Fraction &f) { return *this = *this+f; }
  Fraction& operator -= (const Fraction &f) { return *this = *this-f; }
  Fraction& operator *= (const Fraction &f) { return *this = *this*f; }
  Fraction& operator /= (const Fraction &f) { return *this = *this/f; }
  friend istream& operator >> (istream &is, Fraction &f) {
    is >> f.a >> f.b;
    f.normalize();
    return is;
  }
  friend ostream& operator << (ostream &os, Fraction &f) {
    if (f.a == 0) return os << 0;
    if (f.b == 1) return os << f.a;
    return os << f.a << "/" << f.b;
  }
};
using fraction = Fraction<long long>;
```

{% endspoiler %}

---
# 模数
## 弟弟操作
{% spoiler "代码" %}
```cpp
template <int _MOD> struct Mint {
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

{% endspoiler %}
## tourist的模板(用不来)
[某出处](https://codeforces.com/contest/1383/submission/87883167)

注: `#ifdef _WIN32`部分可能导致 CE

{% spoiler "代码" %}
```cpp
template <typename T>
T inverse(T a, T m) {
  T u = 0, v = 1;
  while (a != 0) {
    T t = m / a;
    m -= t * a; swap(a, m);
    u -= t * v; swap(u, v);
  }
  assert(m == 1);
  return u;
}
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

{% endspoiler %}

---
# 高精度
食用前请必须注意位数是否足够!
## vector版本
[压位+vector+符号 版本](https://github.com/KaizynX/Oier/blob/master/BigInteger/BigInteger.cpp)
## int[]版本
[一本通习题](http://ybt.ssoier.cn:8088/)
[洛谷习题](https://www.luogu.org/problemnew/lists?name=a%2Bb)

此版本 压位+数组,支持cin,cout,string,long long转换,比较运算符,四则运算(包括高精度乘/除低精度,取模),支持带符号的减法运算,支持幂运算,开根运算

可以通过开根外所有习题
{% spoiler "代码" %}
```cpp
struct BigInteger {
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

{% endspoiler %}

---