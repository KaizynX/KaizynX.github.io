---
title: Metabit 面试
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2022-04-28 23:00:00
categories:
  - interview
tags:
description: 一条龙
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/Metabit_interview/cover.jpg
---
# 前言

关于 「乾象投资 Metabit Trading」，我只知道这是家实习生日薪 1k 的公司 (

两轮技术面一轮HR面

被 HR 安排在了一起，三连面一条龙送走

<img src="https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/Metabit_interview//interview.png" data-action="zoom">

# 一面

先自我介绍，着重问了字节的实习经历，但我确实没做什么

然后开始写题

## 第一题

给一颗二叉树，设根的坐标是(0, 0)

一个节点(x, y) 的左儿子坐标 (x - 1, y + 1) 右儿子 (x + 1, y + 1)

返回 `vector<vector<int>>` 第一维按照 x 坐标分

第二维按照 `<y, val>` 关键字排序

直接开 `map<int, pair<int, int>>>` ， `mp[x].emplace_back(y, val)` 然后嗯排

## 第二题

给定

```cpp
class BlockReader {
  // 读取文件 4k 内容放入buffer，返回读取长度
  // 如果不足 4k 就剩多少返回多少
  size_t read4k(char *buffer) {
    ;
  }
}
```

实现任意长度的读取

```cpp
class Reader {
  BlockReader reader_;
  size_t read(size_t size, char *buffer) {
    ;
  }
}
```

做法就是开一个开一个buffer_，每次读4k存着

然后用一个循环，读取的小于所求的就一直循环从 buffer_ 载入

知道达到要求或者文件读完了结束

写完后他说有没有优化，思考一阵未果

他说如果有很多 > 4k 的读取操作呢

那就直接读到 buffer, 按照之前缓存到 buffer_多了一次拷贝

但是又不能一下子就直接读，要先把 buffer_ 给填了再 4k 读

```cpp
class Reader {
public:
  size_t read(size_t size, char *buffer) {
    size_t offset = 0;
    if (offset_ < size_) {
      read_once(buffer, size, offset);
    }
    while (size - offset >= 4096) {
      size_t len = reader_.read4k(buffer + offset);
      if (len == 0) return;
      offset += len;
    }
    if (offset < size) {
      read_once(buffer, size, offset);
    }
  }
private:
  BlockReader reader_;
  size_t size_, offset_;
  char buffer_[4096];
  void read_once(char *buffer, size_t size, size_t &offset) {
    if (offset_ >= size_) {
      size_ = reader_.read4k(buffer);
      if (size_ == 0) return;
      offset = 0;
    }
    size_t len = min(size_ - offset_, size - offset);
    memcpy(buffer + offset, buffer_ + offset_, len);
    offset += len;
    offset_ += len;
  }
}
```

末了问我有什么问题也没什么问题

# 二面

没有自我介绍

又嘴了一遍我的实习经历，因为我只写了两句写得并不清楚

开始搞题

题目是说有一些 task，task 有执行先后顺序条件 形成DAG，每个任务有执行所需时间

有若干个 worker，每次可以执行一个任务（不中断

问怎样安排使得最优(完成所有任务时间最短)

想不出来哇，然后她提示说可以先考虑 worker 无限或者 worker 只有一个的情况

我说无限的话乱执行，时间就是最长的那条链

只有一个的话就是全部任务时间和

想不到最优解，我就说贪心，每次贪心选取能执行的任务中时间最长的先执行

显然不是最优，再度思考

举个反例，就是如果一个任务时间很长但是没有子任务，另一个短但是有很多后续任务，但是因为没有执行这个后面的被阻塞了，那么显然贪心不够优秀

我还真不知道最优解，然后口胡说启发式搜索，假设评估一个任务采用他后续的任务的执行时间

到这就差不多了面试官好像说这个题就不存在最优解的什么算法，让我按照每次优先选取可以选的任务中，他的后续所有任务时间和最大的

开写，先拓扑序倒过来，求那个评估函数

然后搞个优先队列写拓扑

写罢她指出一个问题，就是这个 worker 执行任务是异步的

我把 task 交给 worker 后直接减少入度加队列这样会有问题，应该等任务执行完

于是可以分成两个独立的部分，一部分不断从队列中取出任务给 worker

一部分不断从worker 取出已经执行完成的任务，搞拓扑

以下是本次面试仅保留的代码

```cpp
    map<Task, int> dependents_sum_time; // 前面求的
    map<Task, int> deg;
    struct Queue_task {
      Task *task;
      bool operator < (Queue_task &rhs) const {
        return dependents_sum_time[*task] < dependents_sum_time[*rhs->task];
      }
    };
    priority_queue<Queue_task> q;
    for (auto &task : graph.tasks) {
      for (auto &depend_task : task.dependents) {
        ++deg[depend_task];
      }
    }
    for (auto &task : graph.tasks) {
      if (deg[task] == 0) {
        q.push(Queue_task{&task});
      }
    }
    int finished = 0;
    int task_num = graph.tasks.size();
    mutex lock;
    function<void()> f1 = [&]() {
      while (finished < task_num) {
        Task *task;
        {
          lock_guard<mutex> guard(lock);
          if (q.empty()) continue;
          task = q.top();
          q.pop();
        }
        // 当有可用 worker 时才能计划任务
        while (cluster.GetAvailableWrokers() == 0) { sleep(1); }
        cluster.Schedule(*task);
      }
    };
    function<void()> f2 = [&]() {
      while (finished < task_num) {
        Task *task = cluster.Poll(); // 返回一个已完成任务
        if (task == nullptr) continue;
        ++finished;
        for (auto &depend_task : task->dependents) {
          --deg[depend_task];
          if (deg[depend_task] == 0) {
            lock_guard<mutex> guard(lock);
            q.push(Queue_task{&depend_task});
          }
        }
      }
    }
    thead t1(f1), t2(f2);
    t1.join();
    t2.join();
  }
​
 private:
  Graph graph_; // 图里有 vector<Task>
  Cluster cluster_; // 里面是 worker
};
```

写完15:32超了些，没什么问题结束

# HR 面

又自我介绍，简单问了实习

其他不多说

hr说前两面反馈也不错 (大概率就是稳了