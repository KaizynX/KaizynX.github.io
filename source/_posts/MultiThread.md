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

一意咕行

# 参考资料
[《C++ Concurrency in Action - SECOND EDITION》](https://github.com/xiaoweiChen/CPP-Concurrency-In-Action-2ed-2019)