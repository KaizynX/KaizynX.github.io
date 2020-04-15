---
title: 多线程
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2020-04-03 18:00:00
mathjax: true
categories:
  - 技术
tags:
  - 多线程
description: C++多线程学习笔记
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/MultiThread/cover.jpg 
---
# 线程管理
## 线程管理基础
```cpp
void do_some_work();
std::thread my_thread(do_some_work);
my_thread.detach(); // 分离,不管这个线程了继续执行
if(my_thread.joinable()) my_thread.join(); // 等这个线程执行完
```
## 向线程函数传递参数
```cpp
void f(int i, std::string const& s);
std::thread t(f, 3, "hello");
```
### 引用传递
std::thread 的构造函数无视函数期待的参数类型，并盲目地拷贝已提供的变量...
```cpp
void f(int a, int &b);
int c, d;
std::thread my_thread(f, c, std::ref(d));
```
## 线程数量

```cpp
std::thread::hardware_concurrency()
```
这个函数会返回能并发在一个程序中的线程数量。例如，多核系统中，返回值可以是CPU核芯的数量。返回值也仅仅是一个提示，当系统信息无法获取时，函数也会返回0。

## 标识线程
```cpp
std::thread::id // 数据类型
my_thread.get_id()
std::this_thread::get_id() // 自己线程id
```
# 线程间共享数据

## 使用互斥量保护共享数据

### C++中使用互斥量
C++标准库为互斥量提供了一个RAII语法的模板类std::lock_guard，在构造时就能提供已锁的互斥量，并在析构的时候进行解锁，从而保证了一个已锁互斥量能被正确解锁。
```cpp
std::list<int> some_list;    // 1
std::mutex some_mutex;    // 2

void add_to_list(int new_value)
{
  std::lock_guard<std::mutex> guard(some_mutex);    // 3
  some_list.push_back(new_value);
}

bool list_contains(int value_to_find)
{
  std::lock_guard<std::mutex> guard(some_mutex);    // 4
  return std::find(some_list.begin(),some_list.end(),value_to_find) != some_list.end();
}
```
菜鸡理解:`some_mutex` 作为一个标记，当执行3,4时，标记置为 lock，则无法访问，函数结束后自行 unlock


一意咕行
# pthread
thread 是 C++11 对 pthread 的封装
```cpp
#include <pthread.h>
const int THREAS_COUNT = 8;
struct Info {
  int start, end;
};
void* f(void *_arg) {
  Info arg = *(Info*)_arg;
  // do something
}
int main() {
  int part = n / THREAD_COUNT;
  pthread_attr_t attr;
  void *status;
  pthread_attr_init(&attr);
  pthread_attr_setdetachstate(&attr, PTHREAD_CREATE_JOINABLE);
  Info info[THREAD_COUNT];
  pthread_t threads[THREAD_COUNT];
  for (re int i = 0; i < THREAD_COUNT; ++i) {
    info[i].start = i * part, info[i].end = i * part + part;
  }
  info[THREAD_COUNT-1].end = n;
  for (re int i = 0; i < THREAD_COUNT; ++i) {
    int rc = pthread_create(&threads[i], NULL, f, (void*)&(info[i]));
    if (rc) cerr << "Wrong!" << endl;
  }
  pthread_attr_destroy(&attr);
  for (re int i = 0; i < THREAD_COUNT; ++i) {
    int rc = pthread_join(threads[i], &status);
    if (rc) cerr << "Wrong!" << endl;
  }
}
```

# 参考资料
[《C++ Concurrency in Action - SECOND EDITION》](https://github.com/xiaoweiChen/CPP-Concurrency-In-Action-2ed-2019)
[linux下把进程/线程绑定到特定cpu核上运行](https://blog.csdn.net/guotianqing/article/details/80958281)